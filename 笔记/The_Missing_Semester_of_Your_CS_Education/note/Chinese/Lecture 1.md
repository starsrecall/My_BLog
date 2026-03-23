[课程主页](https://missing.csail.mit.edu/) 
[2026 讲座列表](https://missing.csail.mit.edu/2026/) [往期课程](https://missing.csail.mit.edu/past/) [关于课程](https://missing.csail.mit.edu/about/)

# 课程概览与 Shell

## 我们是谁？

本课程由 [Anish](https://anish.io/)、[Jon](https://thesquareplanet.com/) 和 [Jose](http://josejg.com/) 共同讲授。我们都是前 MIT 学生，在学生时期就发起了这门 MIT IAP 短期课程。你可以通过 [missing-semester@mit.edu](mailto:missing-semester@mit.edu) 联系我们。

我们授课没有薪酬，也没有以任何形式将课程商业化。所有[课程材料](https://missing.csail.mit.edu/)和[讲座录像](https://www.youtube.com/@MissingSemester)都免费在线公开。如果你想支持我们的工作，最好的方式就是向更多人介绍这门课。如果你是公司、大学或其他组织，希望面向更大规模的群体讲授这些内容，请通过邮件给我们发送体验报告/反馈，我们很乐意听到你的分享 :)

## 课程动机
作为计算机从业者，我们都知道计算机最擅长处理重复性工作。但我们常常忽略：这一点不仅适用于我们写程序要完成的计算，也适用于我们使用计算机的过程。在处理任何和计算机相关的工作时，我们手边有大量工具可以帮助我们更高效地解决问题，但很多人只用到了这些工具的一小部分——只会死记硬背一些“魔法命令”勉强应付，遇到问题就盲目从网上复制粘贴命令。

[这门课程就是为了解决这个问题](https://missing.csail.mit.edu/about/)。

我们会教你如何最大化利用已经掌握的工具，介绍可以加入工具箱的新工具，也希望激发你自主探索（甚至构建）更多工具的热情。我们认为，这是绝大多数计算机科学培养体系中缺失的一个学期。

## 课程结构
这门不计学分的课程包含9场1小时的讲座，每场围绕一个[特定主题](https://missing.csail.mit.edu/2026/)展开。讲座内容基本相互独立，但随着课程推进，我们会默认你已经熟悉前期讲座的内容。我们会在线发布讲义，但课堂上的演示等内容可能不会包含在讲义中。和往年一样，我们会录制所有讲座并上传到[YouTube频道](https://www.youtube.com/@MissingSemester)。

我们希望在有限的几场1小时讲座中覆盖尽可能多的内容，因此讲座信息密度较高。为了让你可以按照自己的节奏熟悉内容，每场讲座都配有一系列练习，引导你掌握核心知识点。我们不会设置专门的答疑时间，但你可以在 [OSSU Discord](https://ossu.dev/#community) 的 `#missing-semester-forum` 频道提问，也可以发邮件到 [missing-semester@mit.edu](mailto:missing-semester@mit.edu) 联系我们。

由于时间有限，我们无法像系统课程那样详细讲解所有工具。我们会尽可能提供深入学习的资源，如果你对某个内容特别感兴趣，随时可以联系我们要学习指引！

最后，如果你对课程有任何反馈，也欢迎发邮件到 [missing-semester@mit.edu](mailto:missing-semester@mit.edu)。

---

# 主题 1：Shell

## Shell 是什么？
如今的计算机有各种各样的命令输入接口：花哨的图形界面、语音界面、AR/VR，还有最近兴起的大语言模型。这些接口对80%的场景都足够好用，但从根本上限制了你能做的操作——你没法按下一个不存在的按钮，也没法发出没有被预设的语音指令。要充分利用计算机提供的能力，我们需要回归传统的文本界面：Shell。

几乎所有平台都有某种形式的Shell，很多还提供了多种可选。虽然细节有差异，但核心功能基本一致：允许你运行程序、给程序输入、半结构化地查看程序输出。

要打开可以输入命令的 **Shell 提示符**，你首先需要一个**终端**——也就是Shell的可视化界面。你的设备大概率已经预装了终端，也可以很容易地安装：
*   **Linux:** 按 `Ctrl + Alt + T`（绝大多数发行版适用），或者在应用菜单中搜索“Terminal”。
*   **Windows:** 按 `Win + R`，输入 `cmd` 或 `powershell` 后回车，或者在开始菜单搜索“Terminal”/“命令提示符”。
*   **macOS:** 按 `Cmd + Space` 打开 Spotlight，输入“Terminal”后回车，或者在「应用程序 → 实用工具 → 终端」中找到它。

在Linux和macOS上，打开终端默认会启动Bourne Again SHell，简称`bash`。它是应用最广泛的Shell之一，语法和其他多数Shell相似。在Windows上你会进入batch或powershell，它们是Windows特有的，不是本课程的重点，虽然也能实现我们要讲的大部分功能，但更推荐你安装[适用于Linux的Windows子系统（WSL）](https://docs.microsoft.com/en-us/windows/wsl/) 或Linux虚拟机来使用Unix风格的命令行工具。

还有很多其他Shell，通常比bash有更多易用性改进（最常见的是fish和zsh），也非常流行（所有讲师都在使用），但普及度远不如bash，且核心概念和bash一致，因此本次讲座不会重点介绍。

## 为什么要学习Shell？
Shell不仅通常比“点击操作”快得多，还能提供任何单一图形程序都很难实现的表达能力。我们接下来会看到，Shell允许你以创造性的方式**组合**程序，几乎可以自动化任何任务。

熟悉Shell也能帮你更好地使用开源软件（很多开源软件的安装说明都需要用到Shell）、为项目构建持续集成流程（正如[代码质量讲座](https://missing.csail.mit.edu/2026/code-quality/)中介绍的），还能在其他程序出错时高效调试问题。

## 在Shell中导航
启动终端后，你会看到类似这样的提示符：
```bash
missing:~$
```
这是Shell的主要交互界面：它告诉你当前在名为`missing`的机器上，当前工作目录（你当前所在的位置）是`~`（“家目录”的缩写），`$`表示你当前不是root用户（后续会详细说明）。你可以在提示符后输入命令，由Shell解释执行。最基础的命令是执行程序：
```bash
missing:~$ date
Fri 10 Jan 2020 11:49:31 AM EST
missing:~$
```
这里我们执行了`date`程序，它会打印当前日期和时间，之后Shell会等待你输入下一个命令。我们也可以给命令传递参数：
```bash
missing:~$ echo hello
hello
```
这个例子中，我们让Shell执行`echo`程序，参数是`hello`，`echo`的功能就是打印传入的参数。Shell会按空格拆分命令，运行第一个单词对应的程序，后续的单词都作为参数传给程序。如果你想传递包含空格或特殊字符的参数（比如名为“My Photos”的目录），可以用`'`或`"`包裹参数（`"My Photos"`），或者用`\`转义特殊字符（`My\ Photos`）。

入门阶段最重要的命令可能是`man`，是“manual（手册）”的缩写。`man`程序可以查询系统上任意命令的详细说明，比如运行`man date`会解释`date`的功能，以及所有可用参数的作用。绝大多数命令也支持传入`--help`参数来获取简短的帮助信息。
> 除了`man`，也推荐安装使用[`tldr`](https://tldr.sh/)，它会直接在终端中展示命令的常见用法示例。大语言模型也很擅长解释命令的功能，以及教你如何用命令实现目标。

在`man`之后，第二个需要掌握的命令是`cd`，也就是“change directory（切换目录）”。这个命令是Shell内置的，不是独立程序（也就是说`which cd`会提示找不到cd命令）。你给它传递一个路径，当前工作目录就会切换到对应路径，工作目录的变化也会反映在Shell提示符中：
```
missing:~$ cd /bin
missing:/bin$ cd /
missing:/$ cd ~
missing:~$
```
> 注意Shell支持自动补全，你可以按`<TAB>`键快速补全路径！

如果没有指定其他路径，很多命令都会默认操作当前工作目录。如果不确定自己当前在哪里，可以运行`pwd`命令，或者打印`$PWD`环境变量（用`echo $PWD`），两者都会输出当前工作目录的绝对路径。

有了当前工作目录，我们就可以使用**相对路径**了。之前我们用到的都是**绝对路径**：以`/`开头，给出从文件系统根目录`/`到目标位置的完整路径。实际使用中更常用相对路径：它相对于当前工作目录解析，任何不以`/`开头的路径都是相对路径，第一个路径组件会在当前工作目录下查找，后续组件按正常规则遍历。例如：
```
missing:~$ cd /
missing:/$ cd bin
missing:/bin$
```

每个目录下都有两个特殊的路径组件：`.`和`..`。`.`表示“当前目录”，`..`表示“父目录”，所以你可以写出这样的路径：
```
missing:~$ cd /
missing:/$ cd bin/../bin/../bin/././../bin/..
missing:/$
```
绝对路径和相对路径在绝大多数命令参数中可以互换使用，使用相对路径时只要注意当前工作目录即可！
> 推荐安装使用[`zoxide`](https://github.com/ajeetdsouza/zoxide)来加速目录切换：`z`命令会记住你常访问的路径，只需要少量输入就能快速跳转。

## Shell中有哪些常用工具？
Shell是怎么找到`date`、`echo`这类程序的？当Shell要执行一个命令时，会查询名为`$PATH`的**环境变量**，它列出了Shell搜索程序的目录列表：
```
missing:~$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
missing:~$ which echo
/bin/echo
missing:~$ /bin/echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```
当我们运行`echo`命令时，Shell会在`$PATH`的`:`分隔的目录列表中，按顺序查找名为`echo`的可执行文件，找到后就运行它。我们可以用`which`命令查看某个程序对应的实际文件路径，也可以直接给出文件的完整路径，绕过`$PATH`搜索直接执行。

因此，要知道Shell中可以执行的所有程序，只要列出`$PATH`中所有目录的内容即可。可以用`ls`命令列出目录下的文件：
```
missing:~$ ls /bin
```
> 推荐安装使用[`eza`](https://eza.rocks/)，它是更人性化的`ls`替代工具。

大多数系统的`/bin`下会有非常多程序，这里只介绍最重要的几个：
*   `cat file`：打印`file`的内容。
*   `sort file`：按排序顺序打印`file`的每一行。
*   `uniq file`：去除`file`中连续的重复行。
*   `head file` 和 `tail file`：分别打印`file`的前几行和后几行。
> 推荐安装使用[`bat`](https://github.com/sharkdp/bat)替代`cat`，支持语法高亮和滚动查看。

接下来是`grep pattern file`，它会在`file`中查找匹配`pattern`的行。这个命令非常有用，功能比大多数人想象的更强大：`pattern`其实是**正则表达式**，可以表达非常复杂的匹配规则，我们会在代码质量讲座中[详细介绍正则表达式](https://missing.csail.mit.edu/2026/code-quality/#regular-expressions)。你也可以传入目录（或者不传参数默认是当前目录`.`），加上`-r`参数递归搜索目录下的所有文件。
> 推荐安装使用[`ripgrep`](https://github.com/BurntSushi/ripgrep)替代`grep`，速度更快、更易用（但可移植性稍差），默认就会递归搜索当前工作目录！

还有一些功能强大但接口稍复杂的工具，首先是`sed`，它是可编程的文件编辑器，有自己的编程语言用来自动修改文件，最常见的用法是：
```
missing:~$ sed -i 's/pattern/replacement/g' file
```
这个命令会把`file`中所有`pattern`替换成`replacement`。`-i`表示直接修改原文件（默认是不修改原文件，只打印替换后的内容）；`s/`是sed的替换语法标识；`/`用来分隔匹配模式和替换内容；末尾的`/g`表示替换每行中所有匹配项，而不是只替换第一个。和`grep`一样，这里的`pattern`是正则表达式，替换内容还可以引用匹配到的部分，后面我们会看到示例。

接下来是`find`，可以（递归）查找符合特定条件的文件，例如：
```
missing:~$ find ~/Downloads -type f -name "*.zip" -mtime +30
```
查找下载目录中30天前修改的ZIP文件。
```
missing:~$ find ~ -type f -size +100M -exec ls -lh {} \;
```
查找家目录中大于100M的文件并列出详细信息。注意`-exec`参数接收一个以独立`;`结尾的命令（需要用反斜杠转义`;`），其中`{}`会被替换为每个匹配到的文件路径。
```
missing:~$ find . -name "*.py" -exec grep -l "TODO" {} \;
```
查找所有包含TODO的`.py`文件。
`find`的语法看起来复杂，但非常实用，熟悉后能极大提升文件操作效率。
> 推荐安装使用[`fd`](https://github.com/sharkdp/fd)替代`find`，更易用（但可移植性稍差）。

然后是`awk`，和`sed`一样有自己的编程语言。`sed`侧重编辑文件，`awk`则侧重解析文本文件，最常用于处理CSV这类有规则格式的文件，提取每行的特定部分：
```
missing:~$ awk '{print $2}' file
```
打印`file`中每行的第二个空格分隔的列；如果加上`-F,`参数，就会打印每行的第二个逗号分隔的列。`awk`还能做更多事情：过滤行、计算聚合值等，练习中会有相关体验。

把这些工具组合起来，就能实现非常灵活的功能，比如：
```bash
missing:~$ ssh myserver 'journalctl -u sshd -b-1 | grep "Disconnected from"' \
  | sed -E 's/.*Disconnected from .* user (.*) [^ ]+ port.*/\1/' \
  | sort | uniq -c \
  | sort -nk1,1 | tail -n10 \
  | awk '{print $2}' | paste -sd,
postgres,mysql,oracle,dell,ubuntu,inspur,test,admin,user,root
```
这个命令从远程服务器获取SSH日志（下一节会详细讲`ssh`），搜索断开连接的日志，提取用户名，统计出现次数最多的前10个用户名，最后用逗号分隔输出，全部只需要一行命令！我们会把拆解每个步骤的任务留作练习。

## Shell语言（Bash）
上面的例子引入了一个重要概念：管道`|`，它可以把一个程序的输出作为另一个程序的输入。这是因为绝大多数命令行程序如果没有指定文件参数，就会从“标准输入”（默认是你的键盘输入）读取数据，`|`会把前一个程序的“标准输出”（默认打印到终端的内容），作为后一个程序的标准输入。这种组合能力是Shell生产力高的核心原因之一！

实际上，大多数Shell（比如Bash）本身就是一门完整的编程语言，和Python、Ruby类似，支持变量、条件、循环、函数。你在Shell中运行命令，本质就是在写一段Shell解释执行的代码。我们不会今天就讲完所有Bash语法，只介绍最常用的部分：

首先是**重定向**：
- `> file`：把程序的标准输出写入`file`，而不是打印到终端，方便后续分析。
- `>> file`：把标准输出追加到`file`末尾，而不是覆盖原有内容。
- `< file`：把`file`的内容作为程序的标准输入，而不是从键盘读取。
> 这里介绍一个很有用的命令`tee`：它会把标准输入同时打印到标准输出（和`cat`一样），并且写入文件。比如`verbose cmd | tee verbose.log | grep CRITICAL`，既把完整日志保存到文件，又只在终端显示关键错误信息，非常实用。

接下来是**条件语句**：
`if command1; then command2; command3; fi` 会先执行`command1`，如果它没有报错，就执行`command2`和`command3`，也可以加`else`分支。最常用的`command1`是`test`命令，通常简写为`[`，用来判断条件，比如“文件是否存在”（`test -f file` / `[ -f file ]`）、“两个字符串是否相等”（`[ "$var" = "string" ]`）。Bash中还有`[[ ]]`，它是`test`的“更安全”的内置版本，在引号处理上的异常行为更少。

Bash支持两种循环：`while`和`for`。
- `while command1; do command2; command3; done` 的逻辑和`if`类似，区别是只要`command1`执行不报错，就会反复执行`command2`和`command3`。
- `for varname in a b c d; do command; done` 会把`$varname`依次赋值为`a`、`b`、`c`、`d`，总共执行4次`command`。

除了显式列出元素，你还可以用**命令替换**，比如：
```bash
for i in $(seq 1 10); do
```
这里会先执行`seq 1 10`命令（打印1到10的所有整数，包含首尾），然后把命令的输出替换掉整个`$()`部分，得到一个10次迭代的for循环。在旧代码中你可能会看到反引号的写法（``for i in `seq 1 10`; do``），但更推荐使用`$()`，因为它支持嵌套。

虽然你可以直接在提示符中写很长的Shell命令，但通常更建议把代码写到`.sh`脚本文件中。比如下面这个脚本，会循环运行某个程序直到它失败，只打印失败运行的输出，同时在后台加压CPU（适合复现不稳定的测试）：
```bash
#!/bin/bash
set -euo pipefail

# 在后台启动CPU压力测试
stress --cpu 8 &
STRESS_PID=$!

# 设置日志文件
LOGFILE="test_runs_$(date +%s).log"
echo "日志将保存到 $LOGFILE"

# 循环运行测试直到失败
RUN=1
while cargo test my_test > "$LOGFILE" 2>&1; do
    echo "第 $RUN 次运行通过"
    ((RUN++))
done

# 清理并报告结果
kill $STRESS_PID
echo "测试在第 $RUN 次运行时失败"
echo "最后20行输出："
tail -n 20 "$LOGFILE"
echo "完整日志路径：$LOGFILE"
```
这里包含了很多实用的Shell特性，建议你花时间深入了解：比如后台任务`&`可以让程序并发运行、更复杂的[Shell重定向](https://www.gnu.org/software/bash/manual/html_node/Redirections.html)、[算术扩展](https://www.gnu.org/software/bash/manual/html_node/Arithmetic-Expansion.html)等，这些在编写复杂Shell命令时非常有用。

这里特别解释脚本的前两行：
第一行是**shebang**，不止Shell脚本，很多可执行脚本的顶部都有这行。当一个文件以`#!/path`开头时，Shell会启动`/path`对应的程序，把文件内容作为输入传给它。对于Shell脚本来说，就是把脚本内容传给`/bin/bash`执行，你也可以给Python脚本写`#!/usr/bin/python`的shebang，这样直接执行脚本文件就能用Python运行。
第二行是让Bash更“严格”的配置，可以避免很多Shell脚本的常见陷阱：`set`支持很多参数，简单来说：
- `-e`：只要有任意命令执行失败，脚本就提前退出
- `-u`：使用未定义的变量时直接报错退出，而不是默认替换为空字符串
- `-o pipefail`：如果管道`|`链中的任意命令失败，整个管道的返回值就为失败，避免错误被忽略

> Shell编程和其他编程语言一样是很深的话题，但要注意：Bash有异常多的陷阱，甚至有[多个](https://tldp.org/LDP/abs/html/gotchas.html)网站专门[整理这些坑](https://mywiki.wooledge.org/BashPitfalls)。编写Shell脚本时强烈建议使用[shellcheck](https://www.shellcheck.net/)做静态检查。大语言模型也很擅长编写和调试Shell脚本，当脚本复杂到超过100行时，也可以考虑把它翻译成Python这类“正规”编程语言。

## 下一步
到这里，你已经掌握了Shell的基础用法，足够完成常见任务：可以导航文件系统查找文件，使用大多数程序的基础功能。下一节讲座中，我们会介绍如何用Shell和现成的命令行工具执行、自动化更复杂的任务。

---

## 练习
本课程的所有讲座都配有一系列练习，有些是指定具体任务，有些是开放式的（比如“尝试使用X和Y程序”），我们非常鼓励你动手实践。

我们没有编写练习的标准答案。如果你在某个问题上卡住了，可以在 [Discord](https://ossu.dev/#community) 的 `#missing-semester-forum` 频道发帖，或者给我们发邮件描述你已经尝试过的步骤，我们会尽力提供帮助。这些练习也很适合作为和大语言模型对话的初始提示，你可以交互式地深入探索相关主题。练习的真正价值在于探索答案的过程，而不是答案本身。我们鼓励你在做练习的过程中顺着兴趣点拓展相关内容，多问“为什么”，而不是只找最快得到答案的路径。

1.  本课程需要使用Bash或ZSH这类Unix Shell。如果你用Linux或macOS，不需要额外配置；如果你用Windows，请确保不要使用cmd.exe或PowerShell，可以安装[适用于Linux的Windows子系统](https://docs.microsoft.com/en-us/windows/wsl/) 或Linux虚拟机来使用Unix风格的命令行工具。可以运行`echo $SHELL`确认当前Shell，如果输出类似`/bin/bash`或`/usr/bin/zsh`就说明环境正确。
2.  `ls`的`-l`参数有什么作用？运行`ls -l /`查看输出，每一行的前10个字符分别代表什么含义？（提示：`man ls`）
3.  在命令`find ~/Downloads -type f -name "*.zip" -mtime +30`中，`*.zip`是一个**通配模式（glob）**。什么是通配模式？创建一个测试目录，放入一些文件，尝试使用`ls *.txt`、`ls file?.txt`、`ls {a,b,c}.txt`这类模式，参考Bash手册中的[模式匹配](https://www.gnu.org/software/bash/manual/html_node/Pattern-Matching.html)部分。
4.  `'单引号'`、`"双引号"`和`$'ANSI引号'`有什么区别？编写一个命令，回显包含字面量`$`、`!`和换行符的字符串。参考[引号规则](https://www.gnu.org/software/bash/manual/html_node/Quoting.html)。
5.  Shell有三个标准流：标准输入stdin（0）、标准输出stdout（1）、标准错误stderr（2）。运行`ls /nonexistent /tmp`，把stdout重定向到一个文件，stderr重定向到另一个文件。如何把两个流重定向到同一个文件？参考[重定向](https://www.gnu.org/software/bash/manual/html_node/Redirections.html)。
6.  `$?`保存上一个命令的退出状态（0代表成功，非0代表失败）。`&&`只有前一个命令成功时才会运行后一个命令；`||`只有前一个命令失败时才会运行后一个命令。编写一行命令，仅当`/tmp/mydir`不存在时才创建它。参考[退出状态](https://www.gnu.org/software/bash/manual/html_node/Exit-Status.html)。
7.  为什么`cd`必须是Shell的内置命令，而不能是独立的程序？（提示：思考子进程能影响、不能影响父进程的哪些属性。）
8.  编写一个脚本，接收文件名作为参数（`$1`），用`test -f`或`[ -f ... ]`检查文件是否存在，根据是否存在打印不同的提示信息。参考[Bash条件表达式](https://www.gnu.org/software/bash/manual/html_node/Bash-Conditional-Expressions.html)。
9.  把上一题的脚本保存为`check.sh`，尝试用`./check.sh somefile`运行，会发生什么？运行`chmod +x check.sh`后再试一次，为什么需要这一步？（提示：分别在`chmod`前后运行`ls -l check.sh`查看权限变化。）
10. 给脚本的`set`参数加上`-x`会发生什么？写一个简单的脚本测试，观察输出。参考[set内置命令](https://www.gnu.org/software/bash/manual/html_node/The-Set-Builtin.html)。
11. 编写命令，把一个文件复制为带当天日期的备份（比如`notes.txt` → `notes_2026-01-12.txt`）。（提示：`$(date +%Y-%m-%d)`）参考[命令替换](https://www.gnu.org/software/bash/manual/html_node/Command-Substitution.html)。
12. 修改讲座中提到的不稳定测试脚本，让它接收测试命令作为参数，而不是硬编码`cargo test my_test`。（提示：`$1`或`$@`）参考[特殊参数](https://www.gnu.org/software/bash/manual/html_node/Special-Parameters.html)。
13. 用管道命令找出你的家目录中出现次数最多的5个文件扩展名。（提示：组合使用`find`、`grep`/`sed`/`awk`、`sort`、`uniq -c`和`head`。）
14. `xargs`可以把标准输入的行转换成命令参数。结合`find`和`xargs`（不要用`find -exec`），查找目录下所有`.sh`文件，用`wc -l`统计每个文件的行数。进阶：让命令支持带空格的文件名。（提示：`-print0`和`-0`）参考`man xargs`。
15. 用`curl`获取课程官网（`https://missing.csail.mit.edu/`）的HTML，通过管道传给`grep`，统计列出的讲座数量。（提示：找每个讲座都会出现一次的模式；用`curl -s`关闭进度输出。）
16. [`jq`](https://jqlang.github.io/jq/)是处理JSON数据的强大工具。用`curl`获取示例数据`https://microsoftedge.github.io/Demos/json-dummy-data/64KB.json`，用`jq`提取所有`version`字段大于6的用户的姓名。（提示：先通过管道传给`jq .`查看数据结构，再尝试`jq '.[] | select(...) | .name'`）
17. `awk`可以基于列值过滤行、调整输出格式。比如`awk '$3 ~ /pattern/ {$4=""; print}'`会仅打印第三列匹配`pattern`的行，并且省略第四列。编写`awk`命令，仅打印第二列大于100的行，并且交换第一列和第三列。测试数据：`printf 'a 50 x\nb 150 y\nc 200 z\n'`
18. 拆解讲座中的SSH日志管道命令：每一步分别做了什么？然后写一个类似的命令，从`~/.bash_history`（或`~/.zsh_history`）中找出你最常用的Shell命令。



本内容采用 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) 许可协议授权。