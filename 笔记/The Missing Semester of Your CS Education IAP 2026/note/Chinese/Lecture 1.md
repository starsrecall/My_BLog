[./missing-semester](https://missing.csail.mit.edu/) 

[lectures](https://missing.csail.mit.edu/2026/) [past](https://missing.csail.mit.edu/past/) [about](https://missing.csail.mit.edu/about/)

# 课程概览与 Shell

## 我们是谁？

本课程由 [Anish](https://anish.io/)、[Jon](https://thesquareplanet.com/) 和 [Jose](http://josejg.com/) 共同讲授。我们都是前 MIT 学生，在还是学生时就开始了这个 MIT IAP 课程。您可以通过 [missing-semester@mit.edu](mailto:missing-semester@mit.edu) 联系我们。

我们不是有偿授课，也没有通过任何方式将此课程商业化。我们将所有[课程材料](https://missing.csail.mit.edu/)和[讲座录像](https://www.youtube.com/@MissingSemester)免费在线发布。如果您想支持我们的工作，最好的方式就是向他人介绍这个课程。如果您是公司、大学或其他组织，并希望向更大规模的群体传授这些内容，请通过电子邮件向我们发送体验报告/推荐信，让我们知道 :)

## 动机

作为计算机科学家，我们都知道计算机最擅长帮助我们完成重复性的工作。然而，我们常常忘记这一点不仅适用于我们希望程序执行的计算，也同样适用于我们自己对计算机的使用。在进行任何与计算机相关的工作时，我们指尖下拥有大量工具，能让我们更高效地解决问题。但许多人只利用了这些工具的一小部分；我们仅靠死记硬背一些“魔法咒语”来勉强应付，当遇到困难时，便盲目地从网上复制粘贴命令。

[这门课程旨在解决这个问题 [1]](https://missing.csail.mit.edu/about/)。

我们想教您如何最大限度地利用您已知的工具，向您展示可以添加到工具箱中的新工具，并希望激发您自己探索（甚至构建）更多工具的热情。我们认为这是大多数计算机科学课程中缺失的一个学期 [1][6]。

## 课程结构

这个不计学分的课程包含九场时长1小时的讲座，每场讲座围绕一个[特定的主题](https://missing.csail.mit.edu/2026/)展开。讲座内容很大程度上是独立的，但随着学期的推进，我们会假定您已经熟悉了早期讲座的内容。我们在网上提供了讲义，但课堂中可能包含讲义中没有的内容（例如演示）。与往年一样，我们将录制讲座并将录像发布在[网上](https://www.youtube.com/@MissingSemester)。

我们试图在短短几场1小时的讲座中覆盖大量内容，因此讲座信息密度相当高。为了让您有时间按照自己的节奏熟悉内容，每场讲座都包含一系列练习，以引导您掌握讲座的关键点。我们不会安排专门的答疑时间，但我们鼓励您在 [OSSU Discord](https://ossu.dev/#community) 的 `#missing-semester-forum` 频道提问，或通过电子邮件联系我们：missing-semester@mit.edu。

由于时间有限，我们无法像一门全面课程那样详细地覆盖所有工具。在可能的情况下，我们会尝试为您指明进一步深入了解某个工具或主题的资源，但如果某个内容特别吸引您，请不要犹豫，随时联系我们寻求指导！

最后，如果您对课程有任何反馈，请通过电子邮件发送至 [missing-semester@mit.edu](mailto:missing-semester@mit.edu)。

# 主题 1：Shell

## Shell 是什么？

如今的计算机有各种各样的接口来接受命令：花哨的图形用户界面、语音界面、AR/VR，以及最近的 LLM。这些接口对于 80% 的用例来说都很棒，但它们通常从根本上限制了您能做的事情——您无法按下不存在的按钮，也无法发出未经编程的语音命令。为了充分利用计算机提供的工具，我们必须回归传统，使用文本界面：Shell [6]。

几乎所有您能接触到的平台都有某种形式的 shell，其中许多平台还提供了多种 shell 供您选择。虽然它们在细节上可能有所不同，但核心功能大致相同：它们允许您运行程序，提供输入，并以半结构化的方式检查输出。

要打开一个 shell _提示符_（您可以在其中输入命令），首先需要一个 _终端_，这是 shell 的视觉界面。您的设备很可能预装了一个，或者您可以相当容易地安装一个：

*   **Linux:** 按 `Ctrl + Alt + T`（适用于大多数发行版）。或者在应用程序菜单中搜索“Terminal”。
*   **Windows:** 按 `Win + R`，输入 `cmd` 或 `powershell`，然后按 Enter。或者，在开始菜单中搜索“Terminal”或“Command Prompt”。
*   **macOS:** 按 `Cmd + Space` 打开 Spotlight，输入“Terminal”，然后按 Enter。或者在 应用程序 → 实用工具 → 终端 中找到它。

在 Linux 和 macOS 上，这通常会打开 Bourne Again SHell，简称“bash”。这是使用最广泛的 shell 之一，其语法与您将在许多其他 shell 中看到的类似。在 Windows 上，根据您运行的命令，您会遇到“batch”或“powershell” shell。这些是 Windows 特有的，不是本课程的重点，尽管它们对我们讲授的大部分内容都有类似的功能。您需要使用 [适用于 Linux 的 Windows 子系统](https://docs.microsoft.com/en-us/windows/wsl/) 或 Linux 虚拟机。

还存在其他 shell，通常比 bash 有更多的人体工程学改进（fish 和 zsh 是最常见的）。虽然这些非常流行（所有讲师都使用其中之一），但它们远不如 bash 普及，并且基于许多相同的概念，因此本次讲座不会重点讨论它们。

## 为什么您应该关心它？

Shell 不仅（通常）比“点击操作”快得多，它还提供了您在任何单一图形程序中不易找到的表达能力。正如我们将看到的，shell 使您能够以创造性的方式 _组合_ 程序，从而自动化几乎任何任务 [6]。

熟悉 shell 对于导航开源软件世界（其安装说明通常需要 shell）、为您的软件项目构建持续集成（如 [代码质量讲座](https://missing.csail.mit.edu/2026/code-quality/) 中所述）以及在其它程序失败时调试错误也非常有用。

## 在 Shell 中导航

当您启动终端时，您会看到一个 _提示符_，通常看起来像这样：

```bash
missing:~$
```

这是 shell 的主要文本界面。它告诉您，您在机器 `missing` 上，您的“当前工作目录”，即您当前所在的位置是 `~`（“home”的缩写）。`$` 表示您不是 root 用户（稍后详述）。在此提示符下，您可以键入一个 _命令_，该命令将由 shell 解释。最基本的命令是执行一个程序：

```bash
missing:~$ date
Fri 10 Jan 2020 11:49:31 AM EST
missing:~$
```

这里，我们执行了 `date` 程序，它（也许不出所料）打印出当前的日期和时间。然后 shell 要求我们输入另一个命令来执行。我们也可以带 _参数_ 执行命令：

```bash
missing:~$ echo hello
hello
```

在这种情况下，我们告诉 shell 执行程序 `echo`，参数为 `hello`。`echo` 程序只是打印出它的参数。shell 通过按空白字符分割命令来解析它，然后运行第一个单词指示的程序，并将每个后续单词作为程序可以访问的参数提供。如果您想提供一个包含空格或其他特殊字符的参数（例如，名为“My Photos”的目录），您可以使用 `'` 或 `"` 引用该参数（`"My Photos"`），或者仅使用 `\` 转义相关字符（`My\ Photos`）[6]。

也许您入门时最重要的命令是 `man`，“manual”的缩写。`man` 程序允许您查找系统上任何命令的更多信息。例如，如果运行 `man date`，它会解释 `date` 是什么，以及您可以传递的所有各种参数以改变其行为。通常也可以通过向大多数命令传递 `--help` 参数来获取简短的帮助版本。

> 考虑在 `man` 之外安装并使用 [`tldr`](https://tldr.sh/)，因为它直接在终端中向您展示常见用法示例。LLM 通常也非常擅长解释命令的工作原理以及如何调用它们来实现您的目标。

在 `man` 之后，下一个最重要的命令是 `cd`，即“change directory”。这个命令实际上是内置在 shell 中的，而不是一个单独的程序（即 `which cd` 会说“no cd found”）。您向其传递一个路径，该路径就成为您的当前工作目录。您还会在 shell 提示符中看到工作目录的反映：

    missing:~$ cd /bin
    missing:/bin$ cd /
    missing:/$ cd ~
    missing:~$

> 注意，shell 带有自动补全功能，因此您通常可以通过按 `<TAB>` 更快地补全路径！

如果没有指定其他内容，许多命令会对当前工作目录进行操作。如果您不确定自己在哪里，可以运行 `pwd` 或打印 `$PWD` 环境变量（使用 `echo $PWD`），两者都会产生当前工作目录。

当前工作目录还便于我们使用 _相对_ 路径。到目前为止我们见过的所有路径都是 _绝对_ 路径——它们以 `/` 开头，并给出从文件系统根目录 (`/`) 导航到某个位置所需的完整目录集。在实践中，您更常使用相对路径；之所以称为相对，是因为它们相对于当前工作目录。在相对路径（任何 _不_ 以 `/` 开头的路径）中，第一个路径组件在当前工作目录中查找，后续组件照常遍历。例如：

    missing:~$ cd /
    missing:/$ cd bin
    missing:/bin$

每个目录中还存在两个“特殊”组件：`.` 和 `..`。`.` 是“此目录”，`..` 是“父目录”。所以：

    missing:~$ cd /
    missing:/$ cd bin/../bin/../bin/././../bin/..
    missing:/$

对于任何命令参数，您通常可以互换使用绝对路径和相对路径，只是在使用相对路径时要记住当前的当前工作目录是什么！

> 考虑安装并使用 [`zoxide`](https://github.com/ajeetdsouza/zoxide) 来加速您的 `cd` 操作——`z` 会记住您经常访问的路径，让您用更少的键入访问它们。

## Shell 中有哪些可用工具？

但是 shell 如何知道如何找到像 `date` 或 `echo` 这样的程序呢？如果 shell 被要求执行一个命令，它会查询一个名为 `$PATH` 的 _环境变量_，该变量列出了当给定一个命令时，shell 应搜索程序的目录：

    missing:~$ echo $PATH
    /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
    missing:~$ which echo
    /bin/echo
    missing:~$ /bin/echo $PATH
    /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

当我们运行 `echo` 命令时，shell 看到它应该执行程序 `echo`，然后在 `$PATH` 中由 `:` 分隔的目录列表中搜索具有该名称的文件。当它找到该文件时，就会运行它（假设该文件是 _可执行的_；稍后详述）。我们可以使用 `which` 程序找出为给定程序名称执行的是哪个文件。我们也可以通过给出要执行文件的 _路径_ 来完全绕过 `$PATH`。

这也为我们如何确定在 shell 中能够执行 _所有_ 程序提供了线索：列出 `$PATH` 上所有目录的内容。我们可以通过将给定目录路径传递给 `ls` 程序来实现这一点，该程序会列出文件：

    missing:~$ ls /bin

> 考虑安装并使用 [`eza`](https://eza.rocks/) 以获得更人性化的 `ls`。

在大多数计算机上，这将打印出 _大量_ 程序，但这里我们只关注其中最重要的一些。首先，一些简单的：

*   `cat file`，打印 `file` 的内容。
*   `sort file`，按排序顺序打印 `file` 的行。
*   `uniq file`，消除 `file` 中连续的重复行。
*   `head file` 和 `tail file`，分别打印 `file` 的前几行和后几行。

> 考虑安装并使用 [`bat`](https://github.com/sharkdp/bat) 替代 `cat`，以获得语法高亮和滚动功能。

还有 `grep pattern file`，它在 `file` 中查找匹配 `pattern` 的行。这个命令值得稍加关注，因为它 _非常_ 有用，并且具有比人们预期更广泛的功能。`pattern` 实际上是一个 _正则表达式_，可以表达非常复杂的模式——我们将在代码质量讲座中 [介绍这些](https://missing.csail.mit.edu/2026/code-quality/#regular-expressions)。您也可以指定一个目录而不是文件（或者对于 `.` 省略），并传递 `-r` 以递归搜索目录中的所有文件。

> 考虑安装并使用 [`ripgrep`](https://github.com/BurntSushi/ripgrep) 替代 `grep`，以获得更快、更人性化（但可移植性较差）的替代方案。`ripgrep` 默认也会递归搜索当前工作目录！

还有一些界面稍复杂的非常有用的工具。首先是 `sed`，它是一个编程式文件编辑器。它有自己的编程语言，用于对文件进行自动编辑，但最常见的用法是：

    missing:~$ sed -i 's/pattern/replacement/g' file

这会将 `file` 中所有 `pattern` 的实例替换为 `replacement`。`-i` 表示我们希望替换在原地发生（相对于保持 `file` 不变并打印替换后的内容）。`s/` 是在 sed 编程语言中表达我们想要进行替换的方式。`/` 将模式与替换内容分隔开。而末尾的 `/g` 表示我们希望替换每行上的 _所有_ 匹配项，而不仅仅是第一个。与 `grep` 一样，此处的 `pattern` 是正则表达式，这为您提供了强大的表达能力。正则表达式替换还允许 `replacement` 引用匹配模式的某些部分；我们稍后会看到一个例子。

接下来，我们有 `find`，它让您（递归地）查找符合某些条件的文件。例如：

    missing:~$ find ~/Downloads -type f -name "*.zip" -mtime +30

查找下载目录中超过 30 天的 ZIP 文件。

    missing:~$ find ~ -type f -size +100M -exec ls -lh {} \;

查找您主目录中大于 100M 的文件并列出它们。注意 `-exec` 接受一个以独立 `;` 结尾的 _命令_（我们需要像转义空格一样转义它），其中 `{}` 被 `find` 替换为每个匹配的文件路径。

    missing:~$ find . -name "*.py" -exec grep -l "TODO" {} \;

查找包含 TODO 项的 `.py` 文件。

`find` 的语法可能有点吓人，但希望这能让您感受到它有多有用！

> 考虑安装并使用 [`fd`](https://github.com/sharkdp/fd) 替代 `find`，以获得更人性化（但可移植性较差！）的体验。

接下来是 `awk`，它和 `sed` 一样，有自己的编程语言。`sed` 用于编辑文件，而 `awk` 用于解析文件。`awk` 最常见的用法是针对具有规则语法（如 CSV 文件）的数据文件，您只想提取每条记录（即行）的某些部分：

    missing:~$ awk '{print $2}' file

打印 `file` 每一行的第二个以空白字符分隔的列。如果添加 `-F,`，它将打印每一行的第二个以逗号分隔的列。`awk` 能做更多事情——过滤行、计算聚合等——练习中会有一小部分体验。

将这些工具组合在一起，我们可以做类似这样花哨的事情：

    missing:~$ ssh myserver 'journalctl -u sshd -b-1 | grep "Disconnected from"' \
      | sed -E 's/.*Disconnected from .* user (.*) [^ ]+ port.*/\1/' \
      | sort | uniq -c \
      | sort -nk1,1 | tail -n10 \  | awk '{print $2}' | paste -sd,
    postgres,mysql,oracle,dell,ubuntu,inspur,test,admin,user,root

这从远程服务器获取 SSH 日志（我们将在下一讲中更多讨论 `ssh`），搜索断开连接的消息，从每个这样的消息中提取用户名，并打印前 10 个用户名，以逗号分隔。全部在一个命令中！我们将把剖析每个步骤留作练习。

## Shell 语言 (bash)

前面的例子引入了一个新概念：管道 (`|`)。这使您可以将一个程序的输出与另一个程序的输入串联起来。这之所以有效，是因为如果没有给定 `file` 参数，大多数命令行程序将在其“标准输入”（通常是您的击键输入处）上进行操作。`|` 将 `|` 前面程序的“标准输出”（通常打印到您的终端的内容）作为 `|` 后面程序的标准输入。这使您能够 _组合_ shell 程序，这也是 shell 成为一个高生产力工作环境的部分原因！

事实上，大多数 shell 实现了一个完整的编程语言（如 bash），就像 Python 或 Ruby 一样。它具有变量、条件、循环和函数。当您在 shell 中运行命令时，您实际上是在编写一小段由 shell 解释的代码。我们今天不会教您所有的 bash，但有一些部分您会发现特别有用：

首先，重定向：`>file` 让您将程序的标准输出写入 `file`，而不是您的终端。这使得事后分析更容易。`>>file` 将追加到 `file` 而不是覆盖它。还有 `<file`，它告诉 shell 从 `file` 而不是从您的键盘读取，作为程序的标准输入。

> 现在是提一下 `tee` 程序的好时机。`tee` 会将标准输入打印到标准输出（就像 `cat` 一样！），但 _同时_ 也会将其写入文件。因此 `verbose cmd | tee verbose.log | grep CRITICAL` 将完整的详细日志保存到文件，同时保持您的终端清洁！

接下来，条件语句：`if command1; then command2; command3; fi` 将执行 `command1`，如果它没有导致错误，则将运行 `command2` 和 `command3`。如果您愿意，也可以有一个 `else` 分支。最常用作 `command1` 的命令是 `test` 命令，通常简写为 `[`，它让您评估诸如“文件是否存在”（`test -f file` / `[ -f file ]`）或“字符串是否等于另一个”（`[ "$var" = "string" ]`）之类的条件。在 bash 中，还有 `[[ ]]`，它是 `test` 的“更安全”的内置版本，在引用方面有更少的奇怪行为。

Bash 还有两种形式的循环，`while` 和 `for`。`while command1; do command2; command3; done` 的功能与等效的 `if` 命令类似，不同之处在于只要 `command1` 不报错，它就会一遍又一遍地重新执行整个过程。`for varname in a b c d; do command; done` 执行 `command` 四次，每次将 `$varname` 设置为 `a`、`b`、`c` 和 `d` 中的一个。除了明确列出项目外，您通常会使用“命令替换”，例如：

    for i in $(seq 1 10); do

这会执行命令 `seq 1 10`（打印从 1 到 10 的数字），然后用该命令的输出替换整个 `$()`，从而得到一个 10 次迭代的 for 循环。在较旧的代码中，您有时会看到文字反引号（如 ``for i in `seq 1 10`; do``）而不是 `$()`，但您应强烈偏好 `$()` 形式，因为它可以嵌套。

虽然您 _可以_ 直接在提示符中编写长的 shell 脚本，但您通常更希望将它们写入 `.sh` 文件中。例如，这是一个脚本，它将循环运行一个程序直到失败，仅打印失败运行的输出，同时在后台对您的 CPU 施加压力（例如，用于重现不稳定的测试）：

    #!/bin/bash
    set -euo pipefail
    
    # 在后台启动 CPU 压力测试
    stress --cpu 8 &
    STRESS_PID=$!
    
    # 设置日志文件
    LOGFILE="test_runs_$(date +%s).log"
    echo "Logging to $LOGFILE"
    
    # 运行测试直到一个失败
    RUN=1
    while cargo test my_test > "$LOGFILE" 2>&1; do
        echo "Run $RUN passed"
        ((RUN++))
    done
    
    # 清理并报告
    kill $STRESS_PID
    echo "Test failed on run $RUN"
    echo "Last 20 lines of output:"
    tail -n 20 "$LOGFILE"
    echo "Full log: $LOGFILE"

这里面有很多新东西，我建议您花些时间深入研究，因为它们对于构建有用的 shell 调用非常有用，比如后台作业 (`&`) 来并发运行程序、更复杂的 [shell 重定向](https://www.gnu.org/software/bash/manual/html_node/Redirections.html) 和 [算术扩展](https://www.gnu.org/software/bash/manual/html_node/Arithmetic-Expansion.html)。

不过，值得花点时间看看程序的前两行。第一行是“shebang”——您也会在其他文件（不仅仅是 shell 脚本）的顶部看到这个。当执行一个以神奇咒语 `#!/path` 开头的文件时，shell 将启动 `/path` 处的程序，并将文件内容作为输入传递给它。对于 shell 脚本，这意味着将 shell 脚本的内容传递给 `/bin/bash`，但您也可以编写 shebang 行为 `/usr/bin/python` 的 Python 脚本！

第二行是使 bash 更“严格”的一种方式，可以减轻编写 shell 脚本时的一些陷阱。`set` 可以接受很多参数，但简而言之：`-e` 使得如果任何命令失败，脚本提前退出；`-u` 使得使用未定义变量会导致脚本崩溃，而不仅仅是使用空字符串；`-o pipefail` 使得如果 `|` 序列中的程序失败，整个 shell 脚本也会提前退出。

> Shell 编程是一个深入的话题，就像任何编程语言一样，但要警告：bash 有异常多的陷阱，以至于有 [多个](https://tldp.org/LDP/abs/html/gotchas.html) 专门用于 [列出它们](https://mywiki.wooledge.org/BashPitfalls) 的网站。我强烈建议在编写时充分利用 [shellcheck](https://www.shellcheck.net/)。LLM 也非常擅长编写和调试 shell 脚本，以及当它们对 bash 来说变得过于笨重（100+ 行）时，将它们翻译成“真正的”编程语言（如 Python）。

## 下一步

至此，您对 shell 的了解已经足以完成基本任务。您应该能够四处导航以查找感兴趣的文件，并使用大多数程序的基本功能。在下一讲中，我们将讨论如何使用 shell 和许多现成的命令行程序来执行和自动化更复杂的任务。

## 练习

本课程的所有讲座都附带一系列练习。有些给您一个特定的任务，而另一些则是开放式的，比如“尝试使用 X 和 Y 程序”。我们强烈鼓励您尝试它们。

我们没有为练习编写答案。如果您在任何特定问题上遇到困难，可以在 [Discord](https://ossu.dev/#community) 的 `#missing-semester-forum` 频道发帖，或给我们发送电子邮件，描述您到目前为止尝试过什么，我们将尽力帮助您。这些练习也可能非常适合作为与 LLM 对话的初始提示，您可以在其中交互式地深入探讨该主题。这些练习的真正价值在于发现答案的过程，而不是答案本身。我们鼓励您在工作时跟随切线并问“为什么”，而不是仅仅寻找最短的解决方案路径。

1.  对于本课程，您需要使用像 Bash 或 ZSH 这样的 Unix shell。如果您使用的是 Linux 或 macOS，则无需进行任何特殊操作。如果您使用的是 Windows，则需要确保没有运行 cmd.exe 或 PowerShell；您可以使用 [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/) 或 Linux 虚拟机来使用 Unix 风格的命令行工具。为确保您正在运行适当的 shell，可以尝试命令 `echo $SHELL`。如果它显示类似 `/bin/bash` 或 `/usr/bin/zsh` 的内容，则表示您正在运行正确的程序。

2.  `ls` 的 `-l` 标志有什么作用？运行 `ls -l /` 并检查输出。每一行的前 10 个字符代表什么？（提示：`man ls`）

3.  在命令 `find ~/Downloads -type f -name "*.zip" -mtime +30` 中，`*.zip` 是一个“通配符”。什么是通配符？创建一个包含一些文件的测试目录，并尝试使用像 `ls *.txt`、`ls file?.txt` 和 `ls {a,b,c}.txt` 这样的模式。请参阅 Bash 手册中的 [模式匹配](https://www.gnu.org/software/bash/manual/html_node/Pattern-Matching.html)。

4.  `'单引号'`、`"双引号"` 和 `$'ANSI引号'` 有什么区别？编写一个命令，回显一个包含字面量 `$`、`!` 和换行符的字符串。请参阅 [引用](https://www.gnu.org/software/bash/manual/html_node/Quoting.html)。

5.  shell 有三个标准流：stdin (0)、stdout (1) 和 stderr (2)。运行 `ls /nonexistent /tmp` 并将 stdout 重定向到一个文件，将 stderr 重定向到另一个文件。如何将两者重定向到同一个文件？请参阅 [重定向](https://www.gnu.org/software/bash/manual/html_node/Redirections.html)。

6.  `$?` 保存上一个命令的退出状态（0 = 成功）。`&&` 仅在前一个命令成功时运行下一个命令；`||` 仅在前一个命令失败时运行它。编写一个单行命令，仅当 `/tmp/mydir` 不存在时才创建它。请参阅 [退出状态](https://www.gnu.org/software/bash/manual/html_node/Exit-Status.html)。

7.  为什么 `cd` 必须内置于 shell 本身，而不是一个独立的程序？（提示：考虑子进程在其父进程中可以影响什么，不可以影响什么。）

8.  编写一个脚本，它将文件名作为参数 (`$1`)，并使用 `test -f` 或 `[ -f ... ]` 检查该文件是否存在。根据文件是否存在，它应打印不同的消息。请参阅 [Bash 条件表达式](https://www.gnu.org/software/bash/manual/html_node/Bash-Conditional-Expressions.html)。

9.  将上一个练习的脚本保存到一个文件（例如，`check.sh`）。尝试使用 `./check.sh somefile` 运行它。会发生什么？现在运行 `chmod +x check.sh` 并重试。为什么需要此步骤？（提示：在 `chmod` 之前和之后查看 `ls -l check.sh`。）

10.  如果将 `-x` 添加到脚本中的 `set` 标志会发生什么？尝试一个简单的脚本并观察输出。请参阅 [set 内建命令](https://www.gnu.org/software/bash/manual/html_node/The-Set-Builtin.html)。

11.  编写一个命令，将文件复制到一个备份，并在文件名中包含今天的日期（例如，`notes.txt` → `notes_2026-01-12.txt`）。（提示：`$(date +%Y-%m-%d)`）。请参阅 [命令替换](https://www.gnu.org/software/bash/manual/html_node/Command-Substitution.html)。

12.  修改讲座中的不稳定测试脚本，使其接受测试命令作为参数，而不是硬编码 `cargo test my_test`。（提示：`$1` 或 `$@`）。请参阅 [特殊参数](https://www.gnu.org/software/bash/manual/html_node/Special-Parameters.html)。

13.  使用管道查找您主目录中 5 个最常见的文件扩展名。（提示：结合使用 `find`、`grep` 或 `sed` 或 `awk`、`sort`、`uniq -c` 和 `head`。）

14.  `xargs` 将 stdin 中的行转换为命令参数。将 `find` 和 `xargs` 一起使用（而不是 `find -exec`）来查找目录中的所有 `.sh` 文件，并使用 `wc -l` 计算每个文件的行数。奖励：使其能够处理带空格的文件名。（提示：`-print0` 和 `-0`）。请参阅 `man xargs`。

15.  使用 `curl` 获取课程网站 (`https://missing.csail.mit.edu/`) 的 HTML，并将其通过管道传递给 `grep` 来计算列出的讲座数量。（提示：查找每个讲座出现一次的模式；使用 `curl -s` 来静默进度输出。）

16.  [`jq`](https://jqlang.github.io/jq/) 是处理 JSON 数据的强大工具。使用 `curl` 从 `https://microsoftedge.github.io/Demos/json-dummy-data/64KB.json` 获取示例数据，并使用 `jq` 仅提取版本大于 6 的人员的姓名。（提示：先通过管道传递给 `jq .` 查看结构；然后尝试 `jq '.[] | select(...) | .name'`）

17.  `awk` 可以基于列值过滤行并操作输出。例如，`awk '$3 ~ /pattern/ {$4=""; print}'` 仅打印第三列匹配 `pattern` 的行，同时省略第四列。编写一个 `awk` 命令，仅打印第二列大于 100 的行，并交换第一列和第三列。测试：`printf 'a 50 x\nb 150 y\nc 200 z\n'`

18.  剖析讲座中的 SSH 日志管道：每个步骤做什么？然后构建类似的东西，从 `~/.bash_history`（或 `~/.zsh_history`）中找到您最常用的 shell 命令。

* * *

> **说明**：此中文翻译基于 MIT "The Missing Semester" 课程的中文社区翻译版本，并结合了 2026 年课程内容进行综合整理 [1][6]。

[6]: