# 变量（上）

几乎所有现存编程语言都有**变量（Variable）**的概念：变量是一块内存空间的符号名称，我们可以对这块内存赋值、读取并操作其内容。Bourne Shell（Bourne Shell）也不例外，本章节将介绍相关概念，更深入的内容可查看[变量中](https://www.shellscript.sh/variables2.html)，该章节会讲解系统环境预设的变量。
我们回顾一下第一个Hello World示例，它也可以用变量实现——不过这个例子太过简单，其实没必要用变量！
注意变量赋值的`=`号两侧绝对不能有空格：`VAR=value`是正确写法；`VAR = value`无法正常执行。第一种情况Shell识别到`=`符号，会将该命令判定为**变量赋值（Variable Assignment）**操作；第二种情况Shell会认为`VAR`是一个命令的名称，并尝试执行它。
仔细想想这个设计是合理的：不然你要怎么执行名为`VAR`的命令，且传入第一个参数为`=`、第二个参数为`value`呢？
将以下代码写入`var.sh`：
[var.sh](https://www.shellscript.sh/eg/var.sh.txt)

```bash
#!/bin/sh
MY_MESSAGE="Hello World"
echo $MY_MESSAGE
```
这段代码将字符串`Hello World`赋值给变量`MY_MESSAGE`，然后用`echo`输出该变量的值。
注意字符串`Hello World`必须包裹在引号中。我们可以直接执行`echo Hello World`，因为`echo`支持传入任意数量的参数，但变量只能存储一个值，因此带空格的字符串必须加引号，才能让Shell把整段内容识别为单个值。否则Shell会先执行`MY_MESSAGE=Hello`的赋值操作，然后尝试执行名为`World`的命令。

Shell不限制变量的类型，你可以用它存储字符串、整数、实数等任意内容。
习惯了Perl的开发者会很适应这种设计；如果你是先学C、Pascal，甚至更严谨的Ada入门的，可能会觉得这种设计非常奇怪。实际上Shell中的变量本质上都以字符串形式存储，只是需要数值的程序会把它们当作数字处理。
如果你给变量赋值为字符串，之后尝试给它加1，就会报错：
```bash
$ x="hello"
$ expr $x + 1
expr: non-numeric argument
$
```
这是因为外部命令`expr`仅接受数值作为参数，但以下几种写法在Shell语法层面没有任何区别：
```bash
MY_MESSAGE="Hello World"
MY_SHORT_MESSAGE=hi
MY_NUMBER=1
MY_PI=3.142
MY_OTHER_PI="3.142"
MY_MIXED=123abc
```
注意变量值中的特殊字符必须正确转义，避免被Shell解析为语法符号。相关内容会在第6章[转义字符（Escape Characters）](https://www.shellscript.sh/escape.html)中进一步讨论。

我们可以用`read`命令实现交互性的变量赋值，以下脚本会询问你的姓名，然后输出个性化问候：
[var2.sh](https://www.shellscript.sh/eg/var2.sh.txt)
```bash
#!/bin/sh
echo What is your name?
read MY_NAMEecho "Hello $MY_NAME - hope you're well."
echo "Hello $MY_NAME - hope you're well."
```
在本章节的早期草稿中，我漏掉了最后一行的双引号，导致`you're`里的单引号没有匹配，直接触发语法错误。这类问题经常把Shell开发者逼疯，一定要多加注意！

这里用到了**Shell内置命令（Shell Builtin Command）**`read`，它会从**标准输入（Standard Input）**读取一行内容，存入指定的变量中。
注意就算你输入的全名带空格，且`echo`命令没有加双引号，输出仍然是正常的。这是为什么？之前我们给`MY_MESSAGE`赋值的时候必须加双引号才能处理空格，这里有什么区别？
原因是`read`命令会自动给输入内容加上引号，因此空格会被正确识别。（当然输出的时候你仍然需要加引号，比如`echo "$MY_MESSAGE"`。）

---

## 变量作用域（Scope of Variables）
Bourne Shell的变量不需要像C语言那样提前声明，如果你尝试读取一个未声明的变量，得到的结果是空字符串，不会有任何警告或报错。这可能导致一些非常隐蔽的Bug：比如你赋值`MY_OBFUSCATED_VARIABLE=Hello`，然后执行`echo $MY_OSFUCATED_VARIABLE`，最终会输出空值（因为第二个变量名里的`OBFUSCATED`拼写错误）。

有一个名为`export`的命令，对变量作用域有根本性的影响。要完全理解变量的行为逻辑，你需要掌握`export`的用法。
创建一个小脚本`myvar2.sh`：
[myvar2.sh](https://www.shellscript.sh/eg/myvar2.sh.txt)
```bash
#!/bin/sh
echo "MYVAR is: $MYVAR"
MYVAR="hi there"
echo "MYVAR is: $MYVAR"
```
现在执行该脚本：
```bash
$ ./myvar2.sh
MYVAR is:
MYVAR is: hi there
```
一开始`MYVAR`没有被赋值，所以输出为空。之后我们给它赋值，就得到了预期的结果。
现在执行以下命令：
```bash
$ MYVAR=hello
$ ./myvar2.sh
MYVAR is:
MYVAR is: hi there
```
脚本里还是没读到`MYVAR`的值！这是为什么？
当你从交互Shell中调用`myvar2.sh`时，系统会**派生（Spawn）**一个新的Shell进程来运行该脚本。这一定程度上是因为脚本开头的`#!/bin/sh`行，我们在之前的章节已经讨论过它的作用。
我们需要用**export命令（export）**导出变量，才能让其他程序（包括Shell脚本）继承该变量。输入：
```bash
$ export MYVAR
$ ./myvar2.sh
MYVAR is: hello
MYVAR is: hi there
```
看脚本的第三行，我们在脚本里修改了`MYVAR`的值，但这个修改不会同步回你当前的交互Shell。尝试读取`MYVAR`的值：
```
$ echo $MYVAR
hello
$
```
Shell脚本退出后，它的运行环境就被销毁了。但你当前交互Shell中的`MYVAR`仍然保留着`hello`的值。
如果想要让脚本对环境变量的修改同步回当前Shell，我们必须**source（加载执行）**该脚本——这种方式会直接在当前交互Shell内部运行脚本，而非派生新的Shell进程来执行。
我们可以通过`.`（点）命令来source一个脚本：
```
$ MYVAR=hello
$ echo $MYVAR
hello
$ . ./myvar2.sh
MYVAR is: hello
MYVAR is: hi there
$ echo $MYVAR
hi there
```
现在修改已经同步到当前Shell了！你的系统中的`.profile`或`.bash_profile`配置文件就是用这种机制生效的。注意这种场景下我们不需要提前`export MYVAR`。
新手常犯的一个错误是写`echo MYVAR`而不是`echo $MYVAR`——和大多数编程语言不同，Shell中读取变量值的时候必须加美元符号`$`，但给变量赋值的时候绝对不能加`$`，这是Shell入门阶段很容易踩的坑。
关于变量还有一个值得注意的点，看下面的脚本：
```
#!/bin/sh
echo "What is your name?"
read USER_NAMEecho "Hello $USER_NAME"
echo "I will create you a file called $USER_NAME_file"
touch $USER_NAME_file
```
想想你预期的执行结果是什么？比如你输入`steve`作为`USER_NAME`，脚本应该会创建`steve_file`对吗？
实际上并不会。除非存在名为`USER_NAME_file`的变量，否则这行代码会报错——Shell无法区分变量名的结束位置和后续普通字符的开始位置。要怎么解决这个问题？
解决方法是用**花括号（Curly Brackets）**把变量名本身包裹起来：
[user.sh](https://www.shellscript.sh/eg/user.sh.txt)
```bash
#!/bin/sh
echo "What is your name?"
read USER_NAMEecho "Hello $USER_NAME"
echo "I will create you a file called ${USER_NAME}_file"
touch "${USER_NAME}_file"
```
现在Shell就能识别出我们要引用的是`USER_NAME`变量，后面拼接的是字符串`_file`。这是很多Shell脚本新手容易踩的坑，因为问题根源很难排查。
另外注意`"${USER_NAME}_file"`外面的引号：如果用户输入的是`Steve Parker`（中间带空格），不加引号的话，传给`touch`的参数会变成`Steve`和`Parker_file`，相当于执行`touch Steve Parker_file`，会创建两个文件而非一个。引号可以避免这个问题，感谢Chris指出这一点。

[上一篇：第一个脚本](https://www.shellscript.sh/first.html)  [下一篇：通配符](https://www.shellscript.sh/wildcards.html)

---
版权所有 © 2000 - 2026 Steve Parker
本文转自 [https://www.shellscript.sh/variables1.html](https://www.shellscript.sh/variables1.html)，如有侵权，请联系删除。

【核心术语对照表】
| 英文原文 | 标准译法 | 概念说明 |
|----------|----------|----------|
| Variable | 变量 | 编程语言中用于指代内存存储区域的符号名称，可存储、修改、读取数据，是程序的基础组成元素。 |
| Bourne Shell | Bourne Shell | 1977年发布的经典Unix Shell，是POSIX Shell规范的原型，绝大多数现代类Unix系统的Shell都兼容其语法。 |
| Variable Assignment | 变量赋值 | 给变量绑定具体值的操作，Shell中赋值语法要求等号两侧不能有空格。 |
| expr | expr命令 | 类Unix系统的外部命令，用于执行整数运算、字符串匹配等表达式计算操作。 |
| read | read命令 | Shell内置命令，用于从标准输入读取一行内容，赋值给指定变量。 |
| Shell Builtin Command | Shell内置命令 | 由Shell程序自身提供的命令，无需加载外部可执行文件，执行效率高于外部命令。 |
| Standard Input | 标准输入 | 类Unix系统中程序默认的输入流，通常对应终端的键盘输入，文件描述符为0。 |
| Escape Characters | 转义字符 | 用于消除特殊字符的原有语法含义、使其作为普通字符处理的标记，Shell中常用反斜杠`\`作为转义符。 |
| Scope of Variables | 变量作用域 | 变量在程序中可以被访问、修改的有效范围，Shell中变量默认仅在当前进程生效。 |
| export | export命令 | Shell内置命令，用于将当前Shell的变量导出为环境变量，使子进程可以继承该变量。 |
| Spawn | 派生（进程） | 操作系统中父进程创建新子进程的操作，Shell执行外部脚本时默认会派生新的子Shell进程。 |
| source | source命令/点命令 | Shell内置命令，用于在当前Shell进程中直接执行指定脚本，无需派生新子进程，常用来加载配置文件。 |
| Curly Brackets | 花括号 | Shell语法中用于明确标识变量名边界的符号，可避免变量名与后续字符拼接时产生歧义。 |


【背景补充说明】
本文讲解的变量赋值规则、作用域逻辑、花括号边界标识等内容是Bourne Shell语法的核心基础，被Bash、Dash等绝大多数现代类Unix Shell兼容，也是运维自动化、批量脚本开发的必备知识点，相关规则错误是Shell脚本高频Bug来源。

【歧义说明】
1. 原文代码块中出现的`MY\_MESSAGE\="Hello World"`、`MYVAR\="hi there"`等写法里的反斜杠、下划线前的反斜杠均为Markdown排版转义冗余，实际Shell语法中变量赋值不需要对下划线、等号加转义符，正确写法为`MY_MESSAGE="Hello World"`、`MYVAR="hi there"`。
2. 原文代码块中`read MY\_NAMEecho "Hello $MY_NAME - hope you're well."`、`read USER\_NAMEecho "Hello $USER_NAME"`是排版时换行丢失导致的错误，实际应为两行代码：先执行`read MY_NAME`（或`read USER_NAME`），再换行执行对应的`echo`命令。
