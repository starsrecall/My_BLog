

[课程主页](https://missing.csail.mit.edu/) 
[2026 讲座列表](https://missing.csail.mit.edu/2026/) [往期课程](https://missing.csail.mit.edu/past/) [关于课程](https://missing.csail.mit.edu/about/)

# 命令行环境

正如上一讲所介绍的，绝大多数Shell（壳程序）并非只是启动其他程序的启动器，实际上它提供了一整套具备通用模式与抽象机制的编程语言。但和大多数编程语言不同的是，Shell脚本的所有设计都围绕「运行程序、让程序之间简单高效地通信」这一目标展开。

尤其需要注意的是，Shell脚本高度依赖**约定规范**。一个命令行界面（Command Line Interface, CLI）程序要能在 Shell 生态中良好运行，就需要遵循一系列通用模式。接下来我们会介绍理解命令行程序运行原理所需的核心概念，以及使用、配置这类程序的通用约定。

## 命令行界面
在大多数编程语言中，编写函数的逻辑大致如下：
```python
def add(x: int, y: int) -> int:
    return x + y
```

在这段代码中我们可以清晰看到程序的输入和输出。相比之下，Shell脚本乍看之下会有很大区别。
```bash
#!/usr/bin/env bash

if [[ -f $1 ]]; then
    echo "Target file already exists"
    exit 1
else
    if $DEBUG; then
        grep 'error' - | tee $1
    else
        grep 'error' - > $1
    fi
    exit 0
fi
```

要正确理解这类脚本的运行逻辑，我们首先需要介绍Shell程序之间、以及Shell程序与Shell环境通信时经常出现的几个核心概念：
*   参数（Arguments）
*   流（Streams）
*   环境变量（Environment Variables）
*   返回码（Return Codes）
*   信号（Signals）

---
### 参数（Arguments）
Shell程序在执行时会接收一组参数列表。在Shell中参数都是纯字符串，如何解析这些参数由程序自身决定。例如执行`ls -l folder/`时，我们实际是运行`/bin/ls`程序，并传入参数`['-l', 'folder/']`。

在Shell脚本中，我们可以通过特殊的Shell语法访问这些参数：`$1`对应第一个参数，`$2`对应第二个，依此类推直到`$9`；`$@`用于获取所有参数组成的列表，`$#`用于获取参数的总数；此外`$0`可以获取当前程序的名称。

对大多数程序而言，参数会由**选项（Flags）**和普通字符串混合组成。选项的特征是以单横杠（`-`）或双横杠（`--`）开头，通常是可选的，作用是修改程序的运行行为。例如`ls -l`就改变了`ls`的输出格式。

你会见到双横杠开头的长选项，比如`--all`，以及单横杠开头、通常跟单个字母的短选项，比如`-a`。同一个功能往往同时支持两种写法，例如`ls -a`和`ls --all`是等价的。单横杠的短选项还可以合并书写，因此`ls -l -a`和`ls -la`也等价。选项的顺序通常也不影响结果，`ls -la`和`ls -al`的输出完全一致。有一些选项非常通用，随着你对Shell环境越来越熟悉，会自然而然地使用它们，例如`--help`（查看帮助）、`--verbose`（输出详细日志）、`--version`（查看版本）。

> 选项是Shell约定规范的第一个典型例子。Shell语言本身并没有强制要求程序必须用`-`或`--`来表示选项，你完全可以写出支持`myprogram +myoption myfile`这种语法的程序，但这会造成用户困惑，因为行业默认约定就是用横杠表示选项。实际开发中，大多数编程语言都提供了CLI选项解析库（例如Python的`argparse`），用于解析符合横杠语法的参数。

CLI程序的另一个通用约定是支持接收任意数量的同类型参数，收到这类参数时，命令会对每个参数执行相同的操作。
```bash
mkdir src
mkdir docs
# 等价于
mkdir src docs
```

这种语法糖乍看之下似乎没必要，但和**通配符展开（Globbing）**结合后会变得非常强大。通配符展开（也叫glob匹配）是一类特殊的模式，Shell会在调用程序前先把这类模式展开为对应的实际内容。

假设我们要非递归地删除当前目录下所有.py文件，用上一讲学到的知识，我们可以这么实现：
```bash
for file in $(ls | grep -P '\.py$'); do
    rm "$file"
done
```

但我们只用`rm *.py`就能实现同样的效果！

当我们在终端中输入`rm *.py`时，Shell并不会给`/bin/rm`程序传入参数`['*.py']`，而是会在当前目录下搜索匹配模式`*.py`的文件——其中`*`可以匹配零个或多个任意字符。如果当前目录下有`main.py`和`utils.py`，那么`rm`程序实际收到的参数是`['main.py', 'utils.py']`。

最常用的glob模式包括通配符`*`（匹配零个或多个任意字符）、`?`（匹配恰好一个任意字符），以及花括号展开。花括号`{}`可以将逗号分隔的模式列表展开为多个参数。

实际使用中，通过例子理解glob模式的效果最好：
```bash
touch folder/{a,b,c}.py
# 会展开为
touch folder/a.py folder/b.py folder/c.py

convert image.{png,jpg}
# 会展开为
convert image.png image.jpg

cp /path/to/project/{setup,build,deploy}.sh /newpath
# 会展开为
cp /path/to/project/setup.sh /path/to/project/build.sh /path/to/project/deploy.sh /newpath

# glob模式还可以组合使用
mv *{.py,.sh} folder
# 会移动所有.py和.sh文件
```

> 部分Shell（例如zsh）支持更高级的glob模式，比如`**`会递归匹配所有子目录，因此`rm **/*.py`会递归删除所有.py文件。

---
### 流（Streams）

当我们执行类似下面的程序管道时：
```bash
cat myfile | grep -P '\d+' | uniq -c
```

可以看到`grep`程序同时和`cat`、`uniq`两个程序通信。

这里要注意一个重要特性：这三个程序是同时运行的。也就是说，Shell并不是先运行cat，再运行grep，再运行uniq，而是同时启动三个程序，将cat的输出连接到grep的输入，再将grep的输出连接到uniq的输入。使用管道符`|`时，Shell是基于**数据流**来处理的，数据会沿着管道链从一个程序流向下一个程序。

我们可以验证这种并发性：管道中的所有命令都会立即启动：
```bash
$ (sleep 15 && cat numbers.txt) | grep -P '^\d$' | sort | uniq  &
[1] 12345
$ ps | grep -P '(sleep|cat|grep|sort|uniq)'
  32930 pts/1    00:00:00 sleep
  32931 pts/1    00:00:00 grep
  32932 pts/1    00:00:00 sort
  32933 pts/1    00:00:00 uniq
  32948 pts/1    00:00:00 grep
```

可以看到除了`cat`之外的所有进程都立刻启动了。Shell会在所有进程运行结束前就创建好所有进程，并连接好它们的数据流。`cat`要等sleep运行结束后才会启动，它的输出会发送给grep，再依次传递下去。

每个程序都有一个输入流，称为**标准输入（stdin, standard input）**。使用管道时，标准输入会被自动连接。在脚本中，很多程序接受`-`作为特殊文件名，代表「从标准输入读取内容」：
```bash
# 当数据来自管道时，这两个命令等价
echo "hello" | grep "hello"
echo "hello" | grep "hello" -
```

类似地，每个程序有两个输出流：**标准输出（stdout, standard output）**和**标准错误（stderr, standard error）**。标准输出是最常见的流，用于将程序的输出通过管道传递给下一个命令；标准错误是独立的输出流，用于程序输出警告和其他错误信息，避免这类输出被管道下游的命令解析。
```bash
$ ls /nonexistent
ls: cannot access '/nonexistent': No such file or directory
$ ls /nonexistent | grep "pattern"
ls: cannot access '/nonexistent': No such file or directory
# 错误信息依然会显示，因为标准错误没有被管道传递
$ ls /nonexistent 2>/dev/null
# 没有输出——标准错误被重定向到了/dev/null
```

Shell提供了重定向这些流的语法，以下是一些示例：
```bash
# 将标准输出重定向到文件（覆盖写入）
echo "hello" > output.txt

# 将标准输出重定向到文件（追加写入）
echo "world" >> output.txt

# 将标准错误重定向到文件
ls foobar 2> errors.txt

# 将标准输出和标准错误都重定向到同一个文件
ls foobar &> all_output.txt

# 从文件读取内容到标准输入
grep "pattern" < input.txt

# 将输出重定向到/dev/null来丢弃所有输出
cmd > /dev/null 2>&1
```

另一个体现Unix哲学的强大工具是[`fzf`](https://github.com/junegunn/fzf)——一款模糊查找器。它可以从标准输入读取行内容，提供交互式界面供用户过滤和选择：
```bash
$ ls | fzf
$ cat ~/.bash_history | fzf
```

`fzf`可以和很多Shell操作集成，我们在后续讲Shell定制时会介绍更多它的用法。

---
### 环境变量（Environment Variables）

在bash中赋值变量的语法是`foo=bar`，之后通过`$foo`语法访问变量的值。注意`foo = bar`是非法语法，因为Shell会将其解析为运行`foo`程序，并传入参数`['=', 'bar']`。在Shell脚本中，空格的作用是分割参数，这个特性很容易造成混淆，需要特别注意。

Shell变量没有类型，所有变量都是字符串。注意Shell中的单引号和双引号作用不同，不可以互换：单引号`'`包裹的是字面字符串，不会展开变量、执行命令替换或处理转义序列；而双引号`"`包裹的字符串会执行这些操作。
```bash
foo=bar
echo "$foo"
# 输出bar
echo '$foo'
# 输出$foo
```

要将命令的输出保存到变量中，我们可以使用**命令替换（Command Substitution）**。执行以下代码时：
```bash
files=$(ls)
echo "$files" | grep README
echo "$files" | grep ".py"
```

ls的输出（具体来说是标准输出）会被存入`$files`变量，供后续使用。`$files`变量会保留ls输出中的换行符，因此`grep`这类程序可以逐行处理每个条目。

还有一个较少用到的类似特性是**进程替换（Process Substitution）**：`<( CMD )`会执行`CMD`，将输出存入临时文件，然后把`<()`替换为该临时文件的名称。当命令要求通过文件而非标准输入传入数据时，这个特性非常有用。例如`diff <(ls src) <(ls docs)`可以比较`src`和`docs`两个目录下文件列表的差异。

当Shell程序调用其他程序时，会传递一组变量，这组变量就是环境变量。在Shell中可以通过`printenv`命令查看当前所有环境变量。要显式为某个命令传递环境变量，可以在命令前加上变量赋值语句：

> 环境变量约定俗成使用全大写命名（例如`HOME`、`PATH`、`DEBUG`）。这只是约定而非技术强制要求，但遵循该约定可以和通常小写的Shell局部变量区分开。

```bash
TZ=Asia/Tokyo date  # 输出东京的当前时间
echo $TZ  # 这里输出为空，因为TZ只针对子命令生效
```

除此之外，我们可以用内置命令`export`修改当前Shell的环境变量，这样所有后续启动的子进程都会继承该变量：
```bash
export DEBUG=1
# 所有后续启动的程序的环境变量中都会有DEBUG=1
bash -c 'echo $DEBUG'
# 输出1
```

要删除变量可以使用内置命令`unset`，例如`unset DEBUG`。

> 环境变量是另一个Shell约定规范。它可以隐式修改很多程序的行为，无需显式传参。例如Shell会设置`$HOME`环境变量，值为当前用户的主目录路径，程序可以直接读取这个变量获取该信息，不需要用户显式传入`--home /home/alice`。另一个常见例子是`$TZ`，很多程序会根据这个变量指定的时区来格式化日期时间。

---
### 返回码（Return Codes）

正如我们之前看到的，Shell程序的主要输出通过标准输出/标准错误流，以及文件系统的变更来传递。

默认情况下，Shell脚本会返回0作为退出码。约定中0代表运行正常，非0代表遇到了错误。要返回非0的退出码，需要使用Shell内置命令`exit NUM`。我们可以通过特殊变量`$?`获取上一条命令的返回码。

Shell提供了布尔运算符`&&`（与）和`||`（或）。和普通编程语言中的布尔运算符不同，Shell中的这两个运算符是基于程序的返回码来运算的，且都是[短路运算符](https://en.wikipedia.org/wiki/Short-circuit_evaluation)。也就是说，我们可以用它们来根据前一条命令的成功或失败（成功对应返回码为0）来条件执行后续命令，举几个例子：
```bash
# 只有grep成功（匹配到内容）时才会执行echo
grep -q "pattern" file.txt && echo "Pattern found"

# 只有grep失败（未匹配到内容）时才会执行echo
grep -q "pattern" file.txt || echo "Pattern not found"

# true是始终返回成功的Shell程序
true && echo "This will always print"

# false是始终返回失败的Shell程序
false || echo "This will always print"
```

同样的规则也适用于`if`和`while`语句，它们都基于返回码来做判断：
```bash
# if会判断条件命令的返回码（0代表真，非0代表假）
if grep -q "pattern" file.txt; then
    echo "Found"
fi

# 只要命令返回0，while循环就会继续执行
while read line; do
    echo "$line"
done < file.txt
```

---
### 信号（Signals）
有些情况下你需要中断正在运行的程序，比如某个命令执行时间过长。最简单的中断方式是按下`Ctrl-C`，通常命令就会停止。但这背后的原理是什么？为什么有时候`Ctrl-C`无法终止进程？
```bash
$ sleep 100
^C
$
```
> 注意，这里的`^C`是终端中输入`Ctrl-C`时的显示形式。

这背后的运行流程如下：
1.  用户按下`Ctrl-C`
2.  Shell识别到这个特殊组合键
3.  Shell进程向`sleep`进程发送`SIGINT`信号
4.  信号中断了`sleep`进程的执行

信号是一种特殊的进程间通信机制。进程收到信号后会暂停当前执行，处理信号，并可能根据信号传递的信息改变执行流程，因此信号也属于**软件中断**。

在刚才的例子中，输入`Ctrl-C`会让Shell向进程发送`SIGINT`（中断信号）。下面是一个简单的Python程序示例，它会捕获并忽略`SIGINT`信号，因此按下`Ctrl-C`无法停止它。要终止这个程序，我们可以按下`Ctrl-\`发送`SIGQUIT`（退出信号）。
```python
#!/usr/bin/env python
import signal, time

def handler(signum, time):
    print("\nI got a SIGINT, but I am not stopping")

signal.signal(signal.SIGINT, handler)
i = 0
while True:
    time.sleep(.1)
    print("\r{}".format(i), end="")
    i += 1
```

如果我们向这个程序发送两次`SIGINT`，再发送`SIGQUIT`，效果如下。注意`^`是终端中输入`Ctrl`键时的显示前缀：
```bash
$ python sigint.py
24^C
I got a SIGINT, but I am not stopping
26^C
I got a SIGINT, but I am not stopping
30^\[1]    39913 quit       python sigint.py
```

虽然`SIGINT`和`SIGQUIT`通常都和终端发起的请求相关，还有一个更通用的信号`SIGTERM`，用于请求进程优雅退出。要发送这个信号，可以使用[`kill`](https://www.man7.org/linux/man-pages/man1/kill.1.html)命令，语法为`kill -TERM <PID>`。

信号除了终止进程之外还有其他作用。信号除了终止进程之外还有其他作用。例如`SIGSTOP`（暂停信号）会暂停进程。在终端中按下`Ctrl-Z`会让Shell发送`SIGTSTP`信号，这是Terminal Stop（终端停止）的缩写，也就是终端版本的`SIGSTOP`。

我们可以随后分别使用[`fg`](https://www.man7.org/linux/man-pages/man1/fg.1p.html)（前台）或[`bg`](https://man7.org/linux/man-pages/man1/bg.1p.html)（后台）命令，让被暂停的任务在前台或后台继续运行。

[`jobs`](https://www.man7.org/linux/man-pages/man1/jobs.1p.html)命令会列出当前终端会话关联的未完成任务。你可以通过进程ID（Process ID, PID）引用这些任务（可以用[`pgrep`](https://www.man7.org/linux/man-pages/man1/pgrep.1.html)查找PID）。更直观的方式是使用百分号加任务编号（由`jobs`命令输出）来引用进程。要引用最近一个后台任务，可以使用特殊参数`$!`。

还有一点需要注意，命令后加`&`后缀会让命令在后台运行，你可以继续使用终端提示符，不过后台程序仍然会占用Shell的标准输出，可能会造成干扰（这种情况可以使用Shell重定向解决）。同理，要把已经在运行的程序放到后台，可以按下`Ctrl-Z`后执行`bg`命令。

> 注意后台进程仍然是当前终端的子进程，如果你关闭终端，后台进程也会终止（关闭终端会发送`SIGHUP`（挂起信号））。要避免这种情况，可以用[`nohup`](https://www.man7.org/linux/man-pages/man1/nohup.1.html)运行程序（它会忽略`SIGHUP`信号），如果进程已经启动，可以使用`disown`命令。或者你也可以使用终端复用器（Terminal Multiplexer），我们会在下一节介绍。

以下是展示上述概念的示例会话：
```
$ sleep 1000
^Z
[1]  + 18653 suspended  sleep 1000

$ nohup sleep 2000 &
[2] 18745
appending output to nohup.out

$ jobs
[1]  + suspended  sleep 1000
[2]  - running    nohup sleep 2000

$ kill -SIGHUP %1
[1]  + 18653 hangup     sleep 1000

$ kill -SIGHUP %2   # nohup会屏蔽SIGHUP信号

$ jobs
[2]  + running    nohup sleep 2000

$ kill %2
[2]  + 18745 terminated  nohup sleep 2000
```

`SIGKILL`（强制终止信号）是一个特殊信号，它无法被进程捕获，会立即强制终止进程。但它可能带来不良副作用，比如留下孤儿子进程。

你可以在[这里](https://en.wikipedia.org/wiki/Signal_(IPC))、执行[`man signal`](https://www.man7.org/linux/man-pages/man7/signal.7.html)或者`kill -l`命令，了解更多关于这些信号和其他信号的信息。

在Shell脚本中，你可以使用内置命令`trap`在收到信号时执行指定命令，这对清理操作非常有用：
```bash
#!/usr/bin/env bash
cleanup() {
    echo "Cleaning up temporary files..."
    rm -f /tmp/mytemp.*
}
trap cleanup EXIT  # 脚本退出时执行清理
trap cleanup SIGINT SIGTERM  # 收到Ctrl-C或kill信号时也执行清理
```

---
## 远程机器
程序员日常工作中使用远程服务器的场景越来越普遍。这类场景最常用的工具是SSH（Secure Shell，安全外壳协议），它可以帮你连接到远程服务器，提供我们已经熟悉的Shell界面。我们可以用类似以下的命令连接服务器：
```bash
ssh alice@server.mit.edu
```
这里我们尝试以用户`alice`的身份登录`server.mit.edu`服务器。

SSH一个经常被忽略的特性是可以非交互式运行命令。SSH会正确处理命令的标准输入传递和标准输出接收，因此我们可以把它和其他命令结合使用：
```bash
# ls在远程服务器运行，wc在本地运行
ssh alice@server ls | wc -l

# ls和wc都在远程服务器运行
ssh alice@server 'ls | wc -l'
```

> 建议安装[Mosh](https://mosh.org/)作为SSH的替代工具，它可以处理网络断开、设备休眠/唤醒、网络切换以及高延迟网络等场景。

要让SSH允许我们在远程服务器运行命令，我们需要证明自己有合法权限。我们可以通过密码或SSH密钥完成验证。基于密钥的认证使用公钥加密技术，向服务器证明客户端持有私密的私钥（Private Key），且不会泄露私钥内容。密钥认证既更方便也更安全，因此是更推荐的方式。注意私钥（通常是`~/.ssh/id_rsa`，更新的算法用`~/.ssh/id_ed25519`）本质上就是你的密码，要妥善保管，绝对不要泄露私钥内容。

你可以运行[`ssh-keygen`](https://www.man7.org/linux/man-pages/man1/ssh-keygen.1.html)生成密钥对：
```bash
ssh-keygen -a 100 -t ed25519 -f ~/.ssh/id_ed25519
```

如果你曾经配置过用SSH密钥推送代码到GitHub，那你大概率已经按照[这篇指南](https://help.github.com/articles/connecting-to-github-with-ssh/)的步骤操作过，已经有合法的密钥对了。要检查你的密钥是否设置了密码并验证密码，可以运行`ssh-keygen -y -f /path/to/key`。

服务端的SSH会读取`.ssh/authorized_keys`文件，判断哪些客户端允许登录。要把公钥复制到服务端，可以使用：
```bash
cat .ssh/id_ed25519.pub | ssh alice@remote 'cat >> ~/.ssh/authorized_keys'

# 更简单的方式（如果安装了ssh-copy-id）
ssh-copy-id -i .ssh/id_ed25519 alice@remote
```

除了运行命令，SSH建立的连接还可以用来在本地和服务器之间安全传输文件。[`scp`](https://www.man7.org/linux/man-pages/man1/scp.1.html)是最传统的工具，语法为`scp 本地文件路径 远程主机:远程文件路径`。[`rsync`](https://www.man7.org/linux/man-pages/man1/rsync.1.html)是scp的改进版，它会检测本地和远端的相同文件，避免重复传输。它还支持更细粒度的软链接、权限控制，并且提供`--partial`等特性可以从中断的传输中恢复。rsync的语法和scp类似。

SSH客户端的配置文件位于`~/.ssh/config`，你可以在里面声明主机别名并设置默认参数。不仅SSH会读取这个配置文件，scp、rsync、mosh等其他工具也会读取：
```
Host vm
    User alice
    HostName 172.16.174.141
    Port 2222
    IdentityFile ~/.ssh/id_ed25519

# 配置也支持通配符
Host *.mit.edu
    User alice
```

---
## 终端复用器
使用命令行界面时，你经常会需要同时运行多个任务。比如你可能想同时打开编辑器和运行程序。虽然打开多个终端窗口也能实现，但使用终端复用器（Terminal Multiplexer）是更灵活的解决方案。

像[`tmux`](https://www.man7.org/linux/man-pages/man1/tmux.1.html)这类终端复用器，允许你用窗格（Pane）和标签页（Tab）来复用终端窗口，从而高效地和多个Shell会话交互。此外，终端复用器支持断开当前终端会话，之后再重新连接。这个特性在操作远程机器时非常方便，无需再使用nohup之类的技巧。

目前最流行的终端复用器是[`tmux`](https://www.man7.org/linux/man-pages/man1/tmux.1.html)。tmux支持高度自定义，通过配套的快捷键，你可以创建多个标签页和窗格，并快速在它们之间切换。

tmux的快捷键都遵循`<C-b> x`的格式，意思是：（1）按下`Ctrl+b`，（2）松开`Ctrl+b`，（3）再按下`x`。tmux的对象分为以下层级：
*   **会话（Session）** - 会话是独立的工作区，包含一个或多个窗口
    *   `tmux` 启动新会话
    *   `tmux new -s 名称` 以指定名称启动新会话
    *   `tmux ls` 列出当前所有会话
    *   在tmux内按下`<C-b> d` 断开当前会话
    *   `tmux a` 连接最近的会话，可加`-t`参数指定要连接的会话
*   **窗口（Window）** - 相当于编辑器或浏览器的标签页，是同一会话中视觉上独立的部分
    *   `<C-b> c` 创建新窗口，要关闭窗口可以直接终止其中的Shell，按下`<C-d>`
    *   `<C-b> N` 跳转到第N个窗口，窗口是按数字编号的
    *   `<C-b> p` 跳转到上一个窗口
    *   `<C-b> n` 跳转到下一个窗口
    *   `<C-b> ,` 重命名当前窗口
    *   `<C-b> w` 列出当前所有窗口
*   **窗格（Pane）** - 和Vim的分屏类似，窗格让你可以在同一个视觉界面中运行多个Shell
    *   `<C-b> "` 水平拆分当前窗格
    *   `<C-b> %` 垂直拆分当前窗格
    *   `<C-b> <方向键>` 跳转到指定方向的窗格
    *   `<C-b> z` 切换当前窗格的最大化/还原状态
    *   `<C-b> [` 启动回滚浏览，之后可以按`<space>`开始选择内容，按`<enter>`复制选中内容
    *   `<C-b> <space>` 循环切换窗格布局

> 要了解更多tmux的用法，可以阅读[这篇](https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/)快速教程，以及[这篇](https://linuxcommand.org/lc3_adv_termmux.php)更详细的讲解。

掌握了tmux和SSH之后，你会希望自己的环境在任何机器上都能保持熟悉的使用体验，这就需要用到Shell定制。

---
## 定制Shell
大量命令行程序都通过纯文本文件配置，这类文件叫做**点文件（Dotfile）**——因为文件名以`.`开头，比如`~/.vimrc`，所以默认在`ls`的目录列表中是隐藏的。
> 点文件是另一个Shell约定规范。开头的点就是为了在列目录时“隐藏”这些文件，没错，这又是一个约定。

Shell就是这类通过点文件配置的程序的典型例子。Shell启动时会读取多个文件来加载配置。根据Shell类型、以及你启动的是登录会话还是交互式会话，整个加载流程可能相当复杂。[这篇文章](https://blog.flowblok.id.au/2013-02/shell-startup-scripts.html)是介绍该主题的优秀资源。

对于bash来说，大多数系统下编辑`.bashrc`或`.bash_profile`就可以配置生效。其他支持点文件配置的工具包括：
*   `bash` - `~/.bashrc`、`~/.bash_profile`
*   `git` - `~/.gitconfig`
*   `vim` - `~/.vimrc`和`~/.vim`目录
*   `ssh` - `~/.ssh/config`
*   `tmux` - `~/.tmux.conf`

一个常见的配置修改是为Shell添加新的程序查找路径，安装软件时经常会用到这个操作：
```bash
export PATH="$PATH:要追加的路径"
```
这里我们告诉Shell，将`$PATH`变量的值设为它当前的值加上新路径，并且让所有子进程继承这个新的`PATH`值。这样子进程就能找到`要追加的路径`下的程序了。

定制Shell通常意味着安装新的命令行工具，包管理器（Package Manager）可以简化这个过程。包管理器会处理软件的下载、安装和更新。不同操作系统有不同的包管理器：macOS用[Homebrew](https://brew.sh/)，Ubuntu/Debian用`apt`，Fedora用`dnf`，Arch用`pacman`。我们会在代码交付的章节详细介绍包管理器。

以下是在macOS上用Homebrew安装两个实用工具的示例：
```bash
# ripgrep：速度更快、默认配置更合理的grep替代工具
brew install ripgrep

# fd：速度更快、更易用的find替代工具
brew install fd
```
安装完成后，你就可以用`rg`代替`grep`，用`fd`代替`find`。

> **关于`curl | bash`的安全警告**：你经常会看到类似`curl -fsSL https://example.com/install.sh | bash`的安装指令。这种模式会下载脚本并立即执行，虽然方便但存在风险：你运行的是没有经过检查的代码。更安全的做法是先下载脚本，审核后再执行：
> ```bash
> curl -fsSL https://example.com/install.sh -o install.sh
> less install.sh  # 检查脚本内容
> bash install.sh
> ```
> 有些安装指令使用稍微安全一点的变体：`/bin/bash -c "$(curl -fsSL https://url)"`，这种方式至少确保是用bash来解释脚本，而不是你当前使用的其他Shell。

当你尝试运行未安装的命令时，Shell会提示`command not found`。[command-not-found.com](https://command-not-found.com/)是一个非常实用的网站，你可以搜索任意命令，找到它在不同包管理器和发行版中的安装方式。

另一个实用工具是[`tldr`](https://tldr.sh/)，它提供简化的、以示例为核心的手册页。你不用阅读冗长的文档，就可以快速查看命令的常见用法：
```bash
$ tldr fd
  find的替代工具
  目标是比find更快、更易用

  递归查找当前目录下匹配模式的文件：
      fd "pattern"

  查找以"foo"开头的文件：
      fd "^foo"

  查找指定扩展名的文件：
      fd --extension txt
```

有时候你不需要安装新程序，只是想给带指定参数的现有命令创建快捷方式，这时候就可以用到别名（Alias）。

我们可以使用Shell内置命令`alias`创建自定义命令别名。Shell别名是另一个命令的简写形式，Shell在计算表达式前会自动将别名替换为对应的完整命令。例如bash中的别名语法如下：
```bash
alias 别名="要别名的命令 参数1 参数2"
```
> 注意等号`=`周围不能有空格，因为[`alias`](https://www.man7.org/linux/man-pages/man1/alias.1p.html)是一个接收单个参数的Shell命令。

别名有很多实用的特性：
```bash
# 为常用参数创建简写
alias ll="ls -lh"

# 为高频命令减少输入
alias gs="git status"
alias gc="git commit"

# 避免输入错误
alias sl=ls

# 覆盖原有命令，使用更合理的默认参数
alias mv="mv -i"           # -i会在覆盖文件前提示
alias mkdir="mkdir -p"     # -p会自动创建不存在的父目录
alias df="df -h"           # -h会输出人类可读的格式

# 别名可以组合使用
alias la="ls -A"
alias lla="la -l"

# 要忽略别名，可以在命令前加\
\ls
# 或者用unalias完全删除别名
unalias la

# 要查看别名的定义，直接执行alias加别名
alias ll
# 会输出ll='ls -lh'
```

别名有局限性：它不能在命令中间接收参数。如果需要更复杂的行为，应该使用Shell函数。

大多数Shell支持`Ctrl-R`进行反向历史搜索：按下`Ctrl-R`后开始输入，就可以搜索之前执行过的命令。之前我们介绍过模糊查找器`fzf`，配置好fzf的Shell集成后，`Ctrl-R`会变成交互式的全历史模糊搜索，远比默认功能强大。

应该如何管理你的点文件？你应该把它们放在独立的文件夹中，用版本控制管理，并且通过脚本**软链接（Symlink）**到系统对应的位置。这种做法有以下好处：
*   **安装便捷**：如果你登录新机器，只需要一分钟就能应用所有定制配置。
*   **可移植**：你使用的工具在任何地方的行为都一致。
*   **同步方便**：你可以在任何地方更新点文件，保持所有设备的配置同步。
*   **变更追踪**：你的编程生涯大概率会一直维护点文件，对于长期项目来说，版本历史是非常有用的。

点文件里应该放什么？你可以通过阅读在线文档或[手册页（Man Page）](https://en.wikipedia.org/wiki/Man_page)了解工具的配置项。另一个好方法是搜索特定程序的相关博客，作者会分享他们常用的定制方案。了解配置的还有一种方式是参考其他人的点文件：GitHub上有大量[点文件仓库](https://github.com/search?o=desc&q=dotfiles&s=stars&type=Repositories)，最受欢迎的可以看[这里](https://github.com/mathiasbynens/dotfiles)（不过我们不建议盲目复制配置）。[这个网站](https://dotfiles.github.io/)也是相关主题的优质资源。

本课程的所有讲师都在GitHub上公开了他们的点文件：[Anish](https://github.com/anishathalye/dotfiles)、[Jon](https://github.com/jonhoo/configs)、[Jose](https://github.com/jjgo/dotfiles)。

**框架和插件**也可以优化你的Shell体验。一些流行的通用框架有[prezto](https://github.com/sorin-ionescu/prezto)和[oh-my-zsh](https://ohmyz.sh/)，还有专注特定功能的小型插件：

*   [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting) - 输入时对合法/非法命令进行语法高亮
*   [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions) - 输入时根据历史记录自动补全命令建议
*   [zsh-completions](https://github.com/zsh-users/zsh-completions) - 额外的补全定义
*   [zsh-history-substring-search](https://github.com/zsh-users/zsh-history-substring-search) - 类似fish shell的历史子串搜索
*   [powerlevel10k](https://github.com/romkatv/powerlevel10k) - 快速、可定制的提示符主题

像[fish](https://fishshell.com/)这类Shell默认就集成了很多这类特性。
> 你不需要用oh-my-zsh这类大型框架来获得这些功能，单独安装单个插件通常速度更快，可控性也更高。大型框架会明显拖慢Shell的启动速度，因此建议只安装你实际会用到的功能。

---
## Shell中的AI
有很多方式可以将AI工具集成到Shell中，以下是不同集成层级的几个示例：

**命令生成**：像[`simonw/llm`](https://github.com/simonw/llm)这类工具，可以根据自然语言描述生成Shell命令：
```bash
$ llm cmd "查找最近一周修改过的所有Python文件"
find . -name "*.py" -mtime -7
```

**管道集成**：大语言模型（Large Language Model, LLM）可以集成到Shell管道中处理和转换数据。当你需要从不一致的格式中提取信息，而正则表达式实现起来很麻烦时，它们格外有用：
```bash
$ cat users.txt
Contact: john.doe@example.com
User 'alice_smith' logged in at 3pm
Posted by: @bob_jones on Twitter
Author: Jane Doe (jdoe)
Message from mike_wilson yesterday
Submitted by user: sarah.connor
$ INSTRUCTIONS="从每一行中仅提取用户名，每行一个，不要其他内容"
$ llm "$INSTRUCTIONS" < users.txt
john.doe
alice_smith
bob_jones
jdoe
mike_wilson
sarah.connor
```
注意这里我们使用带引号的`"$INSTRUCTIONS"`，因为变量包含空格，同时用`< users.txt`将文件内容重定向到标准输入。

**AI Shell**：像[Claude Code](https://docs.anthropic.com/en/docs/claude-code)这类工具作为元Shell运行，接收自然语言命令，将其转换为Shell操作、文件编辑以及更复杂的多步骤任务。

---
## 终端模拟器
除了定制Shell之外，花时间挑选合适的**终端模拟器（Terminal Emulator）**并调整其设置也很有价值。终端模拟器是提供文本界面、运行Shell的GUI程序，市面上有很多可选的终端模拟器。

由于你可能会在终端中花费数百甚至数千小时，研究它的设置是值得的。你可能想要调整的终端设置包括：
*   字体选择
*   配色方案
*   快捷键
*   标签/窗格支持
*   回滚配置
*   性能（一些新的终端如[Alacritty](https://github.com/alacritty/alacritty)或[Ghostty](https://ghostty.org/)支持GPU加速）

---
## 练习
### 参数与通配符
1.  你可能见过`cmd --flag -- --notaflag`这类命令。`--`是一个特殊参数，告诉程序停止解析选项，`--`之后的所有内容都会被当作位置参数。这个特性有什么用处？尝试运行`touch -- -myfile`创建文件，然后不用`--`参数删除它，看看会发生什么。
2.  阅读[`ls`的手册页](https://www.man7.org/linux/man-pages/man1/ls.1.html)，编写满足以下要求的`ls`命令：
    *   列出所有文件，包括隐藏文件
    *   以人类可读的格式显示文件大小（例如显示454M而不是454279954）
    *   文件按修改时间从新到旧排序
    *   输出带颜色
    示例输出如下：
    ```
     -rw-r--r--   1 user group 1.1M Jan 14 09:53 baz
     drwxr-xr-x   5 user group  160 Jan 14 09:53 .
     -rw-r--r--   1 user group  514 Jan 14 06:42 bar
     -rw-r--r--   1 user group 106M Jan 13 12:12 foo
     drwx------+ 47 user group 1.5K Jan 12 18:08 ..
    ```
3.  进程替换`<(command)`可以让你把命令的输出当作文件使用。使用`diff`和进程替换比较`printenv`和`export`命令的输出，解释两者为什么不同？（提示：试试`diff <(printenv | sort) <(export | sort)`）

### 环境变量
1.  编写bash函数`marco`和`polo`，实现以下功能：执行`marco`时保存当前工作目录，之后无论你在哪个目录下执行`polo`，都能切换回执行`marco`时所在的目录。调试时可以把代码写在`marco.sh`文件中，通过执行`source marco.sh`将定义加载到当前Shell。

### 返回码
1.  假设你有一个很少出错的命令，为了调试需要捕获它的输出，但等待出错的运行可能非常耗时。编写一个bash脚本，重复运行以下脚本直到它失败，将它的标准输出和标准错误流捕获到文件中，最后打印所有内容。如果还能统计到失败时总共运行了多少次，可以额外加分。
    ```bash
     #!/usr/bin/env bash
     n=$(( RANDOM % 100 ))
     if [[ n -eq 42 ]]; then
        echo "Something went wrong"
        >&2 echo "The error was using magic numbers"
        exit 1
     fi
     echo "Everything went according to plan"
    ```

### 信号与作业控制
1.  在终端中启动`sleep 10000`任务，按下`Ctrl-Z`将其放到后台暂停，再用`bg`让它在后台继续运行。现在用[`pgrep`](https://www.man7.org/linux/man-pages/man1/pgrep.1.html)找到它的进程ID（Process ID, PID），不用手动输入PID，用[`pkill`](https://man7.org/linux/man-pages/man1/pgrep.1.html)杀死这个进程。（提示：使用`-af`参数）
2.  假设你想等另一个进程完成后再启动某个进程，应该怎么实现？在本练习中，我们的前置进程是`sleep 60 &`。实现这个需求的一种方式是使用[`wait`](https://www.man7.org/linux/man-pages/man1/wait.1p.html)命令。尝试启动sleep命令，让`ls`命令等到后台进程结束后再执行。
    不过如果是在不同的bash会话中，这个策略会失效，因为`wait`只对当前Shell的子进程生效。我们的讲义中没有提到的一个特性是：`kill`命令执行成功时返回0，否则返回非0。`kill -0`不会发送信号，但如果进程不存在则会返回非0的退出码。编写一个名为`pidwait`的bash函数，接收一个PID作为参数，等待该进程结束。应该使用`sleep`避免不必要的CPU占用。

### 文件与权限
1.  （进阶题）编写命令或脚本，递归查找目录下最近修改的文件。更通用的，你能按修改时间新旧顺序列出所有文件吗？

### 终端复用器
1.  学习这篇[tmux教程](https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/)，然后按照[这些步骤](https://www.hamvocke.com/blog/a-guide-to-customizing-your-tmux-conf/)学习如何做基础的tmux定制。

### 别名与点文件
1.  创建别名`dc`，当你输错`cd`时会自动解析为`cd`。
2.  运行`history | awk '{$1="";print substr($0,2)}' | sort | uniq -c | sort -n | tail -n 10`获取你最常用的10个命令，考虑为它们编写更短的别名。注意：这个命令适用于Bash，如果你用ZSH，把`history`换成`history 1`。
3.  为你的点文件创建一个文件夹，配置版本控制。
4.  为至少一个程序添加配置，比如你的Shell，可以先从简单的定制开始，例如设置`$PS1`自定义Shell提示符。
5.  配置一套可以在新机器上快速（无需手动操作）安装点文件的方法，可以是一个为每个文件执行`ln -s`的简单Shell脚本，也可以使用[专门的工具](https://dotfiles.github.io/utilities/)。
6.  在全新的虚拟机上测试你的安装脚本。
7.  将你当前所有的工具配置迁移到点文件仓库中。
8.  把你的点文件发布到GitHub上。

### 远程机器（SSH）
以下练习需要安装Linux虚拟机（也可以用已有的）。如果你不熟悉虚拟机，可以参考[这篇教程](https://hibbard.eu/install-ubuntu-virtual-box/)学习如何安装。
1.  进入`~/.ssh/`目录，检查是否已经有SSH密钥对。如果没有，用`ssh-keygen -a 100 -t ed25519`生成。建议给密钥设置密码并使用`ssh-agent`，更多信息参考[这里](https://www.ssh.com/ssh/agent)。
2.  编辑`.ssh/config`，添加如下配置：
    ```
     Host vm
         User 你的用户名
         HostName 虚拟机IP
         IdentityFile ~/.ssh/id_ed25519
         LocalForward 9999 localhost:8888
    ```
3.  用`ssh-copy-id vm`把你的SSH公钥复制到服务器。
4.  在虚拟机中执行`python -m http.server 8888`启动Web服务器，在本地浏览器访问`http://localhost:9999`，确认可以访问虚拟机的Web服务。
5.  执行`sudo vim /etc/ssh/sshd_config`编辑SSH服务端配置，修改`PasswordAuthentication`的值禁用密码认证，修改`PermitRootLogin`的值禁用root登录。执行`sudo service sshd restart`重启SSH服务，尝试重新SSH登录。
6.  （挑战题）在虚拟机中安装[`mosh`](https://mosh.org/)并建立连接，然后断开服务器/虚拟机的网络适配器，mosh能正常恢复连接吗？
7.  （挑战题）了解SSH的`-N`和`-f`参数的作用，写出实现后台端口转发的命令。

---
* * *
[编辑本页面](https://github.com/missing-semester/missing-semester/blob/master/_2026/command-line-environment.md)。
本内容采用[CC BY-NC-SA](https://creativecommons.org/licenses/by-nc-sa/4.0/)协议授权。

本文转自 [https://missing.csail.mit.edu/2026/command-line-environment/](https://missing.csail.mit.edu/2026/command-line-environment/)，如有侵权，请联系删除。

---
【核心术语对照表】
| 英文原文                     | 标准译法      | 概念说明                                                     |
| ---------------------------- | ------------- | ------------------------------------------------------------ |
| Shell                        | 壳程序/Shell  | 类Unix系统中提供命令行界面、用于和操作系统交互的程序，常见的有bash、zsh等，业内通常直接使用原名 |
| Command Line Interface (CLI) | 命令行界面    | 基于文本的用户交互界面，用户通过输入文本命令操作系统         |
| Flag                         | 选项          | 命令行参数中以`-`或`--`开头的部分，用于调整命令的运行行为    |
| Globbing/Glob                | 通配符展开    | Shell在执行命令前，将`*`、`?`、`{}`等特殊模式展开为匹配的文件路径或参数列表的操作 |
| Stream                       | 流            | 程序输入输出的字节序列，Shell中默认分为标准输入、标准输出、标准错误三类 |
| stdin (standard input)       | 标准输入      | 程序默认的输入流，默认关联到终端键盘输入                     |
| stdout (standard output)     | 标准输出      | 程序默认的正常输出流，默认关联到终端显示器                   |
| stderr (standard error)      | 标准错误      | 程序默认的错误输出流，默认关联到终端显示器，和标准输出相互独立 |
| Environment Variable         | 环境变量      | 操作系统或Shell中全局生效的键值对变量，会被当前Shell启动的所有子进程继承 |
| Command Substitution         | 命令替换      | Shell中将命令的输出替换为文本值的语法，通常用`$(命令)`实现   |
| Process Substitution         | 进程替换      | Shell中将命令的输出临时保存为文件、并将语法部分替换为临时文件路径的特性，语法为`<(命令)` |
| Return Code/Exit Code        | 返回码/退出码 | 程序运行结束后返回的整数值，约定0代表运行成功，非0代表运行出错 |
| Short-circuit Evaluation     | 短路求值      | 布尔运算中，当第一个操作数已经可以确定整个表达式的结果时，不再计算第二个操作数的特性 |
| Signal                       | 信号          | 类Unix系统中进程间通信的一种机制，用于向进程发送异步通知，例如中断、终止、暂停等指令 |
| SSH (Secure Shell)           | 安全外壳协议  | 用于在不安全网络中安全登录远程主机、传输数据的加密协议       |
| Terminal Multiplexer         | 终端复用器    | 允许在一个终端窗口中同时管理多个Shell会话、支持会话断开重连的工具，常见的有tmux、screen |
| Dotfile                      | 点文件        | 文件名以`.`开头的配置文件，默认在目录列表中隐藏，用于配置各类命令行工具 |
| Alias                        | 别名          | Shell中为长命令设置的简写，执行时会自动替换为完整命令        |
| Terminal Emulator            | 终端模拟器    | 提供图形界面、模拟传统终端功能的程序，用于运行Shell和命令行程序 |
| Man Page                     | 手册页        | 类Unix系统中内置的命令和工具官方文档，可通过`man`命令查看    |

---
