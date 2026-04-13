# Shell 脚本编程教程
---
## 循环

大多数编程语言都支持循环特性：如果我们需要重复执行某个任务20次，总不可能手动写20遍代码，每次还可能要做小幅修改。
因此Bourne Shell中提供了`for`和`while`两种循环，虽然相比其他语言功能少一些，但Shell编程的定位本就不是和C这类高级语言比功能丰富度。

---
## for 循环
`for`循环的作用是遍历一组给定的值，直到列表全部遍历完成后结束：

### 示例1：遍历数字列表（for.sh）
```bash
#!/bin/sh
for i in 1 2 3 4 5
do
  echo "循环中... 当前数字是 $i"
done
```
运行这段代码即可看到效果，注意`for`循环遍历的值可以是任意类型，不一定是数字。

### 示例2：遍历混合类型列表（for2.sh）
```bash
#!/bin/sh
for i in hello 1 \* 2 goodbye 
do
  echo "循环中... 变量i的值为 $i"
done
```
这个示例非常值得反复测试理解：
1. 先去掉`\*`运行，搞懂基本遍历逻辑；
2. 再重读[通配符](https://www.shellscript.sh/wildcards.html)章节，保留`*`（不转义）测试，观察通配符展开效果；
3. 可以在不同目录下运行，对比结果差异；
4. 分别测试用双引号包裹`"*"`、用反斜杠转义`\*`的不同效果。

如果你现在手边没有Shell环境可以实操（读教程的时候手边有Shell运行测试非常重要），以上两个脚本的运行结果分别如下：
第一个`for.sh`的输出：
```
循环中... 当前数字是 1
循环中... 当前数字是 2
循环中... 当前数字是 3
循环中... 当前数字是 4
循环中... 当前数字是 5
```
第二个`for2.sh`的输出（*未转义时，会被Shell自动展开为当前目录下的所有文件名）：
```
循环中... 变量i的值为 hello
循环中... 变量i的值为 1
循环中... 变量i的值为 当前目录下第一个文件的文件名
... 依次遍历当前目录下所有文件名 ...
循环中... 变量i的值为 当前目录下最后一个文件的文件名
循环中... 变量i的值为 2
循环中... 变量i的值为 goodbye
```
由此可见：`for`循环会遍历所有给定的输入项，直到没有更多输入项就自动结束。

---
## while 循环
`while`循环的玩法要更多一些😉（至于有没有意思，得看你对「乐趣」的定义，以及你是不是个天天宅家敲代码的人）

### 示例1：交互式循环（while.sh）
```bash
#!/bin/sh
INPUT_STRING=hello
while [ "$INPUT_STRING" != "bye" ]
do
  echo "请输入内容（输入bye退出）"
  read INPUT_STRING
  echo "你输入的是：$INPUT_STRING"
done
```
这段代码会一直运行，直到你输入`bye`才会退出。
可以复习[《变量（第一部分）》](https://www.shellscript.sh/variables1.html)理解为什么要在循环前先给`INPUT_STRING`赋值为`hello`：这是为了保证第一次循环一定能执行，相当于「先执行后判断」的repeat循环，而非传统的「先判断后执行」的while循环。

---
Shell中的冒号`:`是一个特殊内置命令，永远返回真值，因此`while :`可以直接创建死循环。虽然这种写法有时候必要，但更推荐给循环设置明确的退出条件。对比以下示例和上面的示例，看看哪种写法更优雅，再思考下各自适用的场景：

### 示例2：死循环（while2.sh）
```bash
#!/bin/sh
while :
do
  echo "请输入内容（按Ctrl+C强制退出）"
  read INPUT_STRING
  echo "你输入的是：$INPUT_STRING"
done
```
---
另一个非常实用的技巧是`while read`循环，用于逐行读取文件内容。下面的示例用到了后续会讲解的`case`分支语句，功能是读取`myfile.txt`的每一行，判断对应的问候语属于哪种语言：
> ⚠️ 注意：文件的每一行必须以换行符（LF，Line Feed）结尾，如果`myfile.txt`最后一行没有换行（即文件末尾不是空行），那么最后一行不会被处理，这是Shell `read`命令的常见坑。

`while read`会逐行读取`myfile.txt`的内容，存入变量`$input_text`，然后用`case`语句判断：如果内容是`hello`就输出「English」，是`gday`就输出「Australian」，如果没有匹配的模式，就走默认的`*`分支输出「未知语言」。

### 示例3：逐行读取文件（while3.sh）
```bash
#!/bin/sh
while read input_text
do
  case $input_text in
        hello)          echo English    ;;
        howdy)          echo American   ;;
        gday)           echo Australian ;;
        bonjour)        echo French     ;;
        "guten tag")    echo German     ;;
        *)              echo 未知语言: $input_text
                ;;
   esac
done < myfile.txt
```
这里的`< myfile.txt`是输入重定向，将文件内容作为循环的标准输入，因此`read`会从文件读取内容，而非等待用户键盘输入。

假设我们的`myfile.txt`内容如下（共5行）：
```
this file is called myfile.txt and we are using it as an example input.
hello
gday
bonjour
hola
```
运行脚本的结果如下：
```bash
$ ./while3.sh
未知语言: this file is called myfile.txt and we are using it as an example input.
English
Australian
French
未知语言: hola
```
---
## Bash 专属小技巧：大括号扩展
我最近从[Linux From Scratch（LFS，从零构建Linux）](http://www.linuxfromscratch.org/)项目中学到了一个非常方便的Bash特性（注意：仅支持Bash，不兼容标准Bourne Shell）：大括号扩展。
比如要批量创建`rc0.d`到`rc6.d`加上`rcS.d`目录，你不用写循环：
```bash
# 繁琐的传统循环写法
for runlevel in 0 1 2 3 4 5 6 S
do
  mkdir rc${runlevel}.d
done
```
直接用大括号扩展一行就能搞定：
```bash
mkdir rc{0,1,2,3,4,5,6,S}.d
```
大括号扩展还支持嵌套，比如下面的命令可以列出根目录、`usr`目录、`usr/local`目录下的`bin`、`sbin`、`lib`目录的属性：
```bash
$ cd /
$ ls -ld {,usr,usr/local}/{bin,sbin,lib}
drwxr-xr-x    2 root     root         4096 Oct 26 01:00 /bin
drwxr-xr-x    6 root     root         4096 Jan 16 17:09 /lib
drwxr-xr-x    2 root     root         4096 Oct 27 00:02 /sbin
drwxr-xr-x    2 root     root        40960 Jan 16 19:35 usr/bin
drwxr-xr-x   83 root     root        49152 Jan 16 17:23 usr/lib
drwxr-xr-x    2 root     root         4096 Jan 16 22:22 usr/local/bin
drwxr-xr-x    3 root     root         4096 Jan 16 19:17 usr/local/lib
drwxr-xr-x    2 root     root         4096 Dec 28 00:44 usr/local/sbin
drwxr-xr-x    2 root     root         8192 Dec 27 02:10 usr/sbin
```
---
版权所有 © 2000 - 2026 Steve Parker
本文转自 [https://www.shellscript.sh/loops.html](https://www.shellscript.sh/loops.html)，如有侵权，请联系删除。

---
### 核心术语对照表
| 英文术语 | 标准译法 | 说明 |
|----------|----------|------|
| Bourne Shell | 伯恩Shell | 兼容POSIX标准的原生Shell，是绝大多数Shell语法的基础 |
| Bash | Bash Shell | 伯恩增强Shell（Bourne Again Shell），是目前Linux、macOS默认使用的Shell，支持更多扩展特性 |
| Wildcards | 通配符 | Shell中用于匹配文件名的特殊符号，比如`*`匹配任意数量任意字符 |
| Brace Expansion | 大括号扩展 | Bash专属特性，可快速生成字符串组合，简化批量操作 |
| `read` 命令 | `read` 输入命令 | 用于从标准输入（键盘/文件）读取内容并存入变量 |
| Input Redirection | 输入重定向 | 用`<`符号将文件内容作为命令/循环的标准输入，替代键盘输入 |
| Infinite Loop | 死循环/无限循环 | 永远不会自动退出的循环，需要手动中断或满足内部退出条件 |
