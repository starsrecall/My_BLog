# Shell 脚本编程教程
*   [首页](https://www.shellscript.sh/#home)
*   [设计理念](https://www.shellscript.sh/philosophy.html)
*   [第一个脚本](https://www.shellscript.sh/first.html)
*   [变量（上）](https://www.shellscript.sh/variables1.html)
*   [通配符](https://www.shellscript.sh/wildcards.html)
*   [转义字符](https://www.shellscript.sh/escape.html)
*   [循环](https://www.shellscript.sh/loops.html)
*   [条件测试](https://www.shellscript.sh/test.html)
*   [分支选择](https://www.shellscript.sh/case.html)
*   [变量（中）](https://www.shellscript.sh/variables2.html)
*   [变量（下）](https://www.shellscript.sh/variables3.html)
*   [外部程序](https://www.shellscript.sh/external.html)
*   [函数](https://www.shellscript.sh/functions.html)
*   [实用技巧](https://www.shellscript.sh/hints.html)
*   [快速参考](https://www.shellscript.sh/quickref.html)
*   [交互式Shell](https://www.shellscript.sh/interactive.html)
*   [练习](https://www.shellscript.sh/exercises.html)
*   [技巧与示例](https://www.shellscript.sh/examples/)
*   [《Shell脚本编程：高级实战》](https://amzn.to/3J1JYSx)

----

### 本教程的目的
本教程旨在帮助读者理解**Shell脚本（Shell Script，也叫Shell scripting）**编程的基础概念，同时介绍Bourne Shell（伯恩Shell）环境下简单却强大的编程能力。因此本教程既可用作一对一/小组教学、配套练习的基础材料，也可作为后续开发工作的参考手册。

### 获取本教程的最新版本
你当前阅读的是4.5b版本，最后更新于2023年6月6日。
本教程的最新版本永久发布于：[https://www.shellscript.sh](https://www.shellscript.sh/)，请务必从该地址获取最新版本。（如果你是在其他地址阅读本文，大概率是官方站点的镜像副本，内容可能已经过时）

### sh的发展简史
史蒂夫·伯恩（Steve Bourne）编写了最初的Bourne Shell（伯恩Shell），首次出现在贝尔实验室发布的第7版Unix系统中。此后衍生出了众多Shell变种（比如csh、ksh等），其中不少已经被淘汰。
本教程仅讲解兼容Bourne Shell的语法作为基础，覆盖Shell脚本编程的所有核心知识点。教程的[技巧与示例](https://www.shellscript.sh/examples/)部分会补充额外实例，专门讲解Bash Shell相比标准Bourne Shell提供的更多实用扩展功能。

### 适用人群
本教程假设读者具备以下前置经验：
*   **会使用**Unix/Linux的**交互式**Shell
*   具备基础编程知识——了解变量、函数的概念即可
*   了解部分Unix/Linux命令，能够熟练使用常用命令（比如`ls`、`cp`、`echo`等）
*   会使用ruby、perl、python、C、Pascal甚至BASIC等任意编程语言，能读懂Shell脚本但不清楚其运行原理的开发者

你可以阅读[本教程收到的用户反馈](https://www.shellscript.sh/feedback.html)，进一步判断它是否适合你。

### 本教程的排版约定
代码段和脚本输出会用等宽字体显示。命令行输入的内容前会加美元符号（`$`）作为前缀。如果你的Shell提示符和示例不同，可以执行以下命令修改提示符：
```
PS1="$ " ; export PS1
```
修改后你的交互界面就会和示例（比如下面的`./my-script.sh`）一致。脚本输出（比如下面的"Hello World"）会直接顶行显示。
```
$ echo '#!/bin/sh' > my-script.sh
$ echo 'echo Hello World' >> my-script.sh
$ chmod 755 my-script.sh
$ ./my-script.sh
Hello World
$
```
完整的脚本会以如下格式展示，如果有对应的纯文本版本会附带链接，比如：[my-script.sh](https://www.shellscript.sh/eg/my-script.sh.txt)
```shell
#!/bin/sh
# 这是注释！
echo Hello World        # 这也是注释！
```
> 注意：要让文件可执行，你必须设置**可执行（eXecutable）**权限位；而Shell脚本还需要同时开启**可读（Readable）**权限位。因此你通常需要修改脚本的权限才能运行它。如果你的脚本名为"myscript.sh"，可以这样修改权限：
```bash
$ chmod u+rx myscript.sh
$ ./myscript.sh
```

[仅需5美元即可购买本教程的PDF版本](https://gum.co/shellscript)
* * *
[下一章：设计理念](https://www.shellscript.sh/philosophy.html)
* * *

### 我的纸质书与电子书
我撰写的Shell脚本编程相关书籍有纸质版和电子版两种形式：本[教程](http://amzn.to/2mPj2tK)是Shell脚本的通用入门材料，更厚的《[Shell脚本编程：Linux、Bash高级实战大全](http://amzn.to/2mPhTlK)》则详细覆盖了Bash的所有特性。
<table class="booklist"><tbody><tr><td><a target="_blank" href="http://amzn.to/2mPj2tK"><center><img src="https://www.shellscript.sh/amzn/tutorial.jpg" width="125px" height="200px" alt="Shell Scripting Tutorial"></center><br>《Shell脚本编程教程》</a>就是本站教程的纸质/电子版，共88页，方便随身携带阅读，纸质版也可以放在桌边作为常备参考手册。<br><br>也可以在Gumroad购买PDF版本：<a class="gumroad-button" href="https://gum.co/shellscript" target="_blank">点击购买PDF版</a></td><td><a target="_blank" href="http://amzn.to/2mPhTlK"><center><img src="https://www.shellscript.sh/amzn/shellscripting.jpg" width="159px" height="200px" alt="Shell Scripting: Expert Recipes for Linux, Bash and more"></center><br>《Shell脚本编程：Linux、Bash高级实战大全》</a>是我撰写的564页Shell编程专著，前半部分详细讲解Shell的所有特性，后半部分按主题整理了真实生产环境的Shell脚本，并对每个脚本做了详细解析。</td></tr></tbody></table>

Copyright © 2000 - 2026 Steve Parker
本文转自 [https://www.shellscript.sh/](https://www.shellscript.sh/)，如有侵权，请联系删除。

---
【核心术语对照表】
| 英文原文 | 标准译法 | 概念说明 |
| -------- | -------- | -------- |
| Shell Script / Shell Scripting | Shell脚本 / Shell脚本编程 | 基于Shell命令编写的脚本程序，以及编写这类程序的过程，常用于系统自动化操作 |
| Bourne Shell | 伯恩Shell | 由Steve Bourne开发的初代标准Unix Shell，是后续几乎所有Shell的兼容基础，符合POSIX标准 |
| Bash | Bash Shell | 目前最流行的Shell实现，是绝大多数Linux发行版的默认Shell，兼容Bourne Shell并扩展了大量实用功能 |
| Executable Bit | 可执行位 | Unix/Linux文件权限的标识位之一，标记该文件是否可以被系统作为程序运行 |
| Readable Bit | 可读位 | Unix/Linux文件权限的标识位之一，标记该文件是否可以被读取内容 |
| chmod | 权限修改命令 | Unix/Linux系统中用于修改文件权限的命令 |
| PS1 | 提示符变量 | 交互式Shell的环境变量，定义命令行提示符的显示格式 |
| POSIX | 可移植操作系统接口 | 类Unix系统的通用标准，规定了Shell、系统调用等统一接口，保证程序在不同Unix系统间可移植 |


---
【背景补充说明】
本教程是全球范围内知名度极高的免费Shell脚本入门教程，由资深Unix开发者Steve Parker从2000年开始维护至今，内容严格遵循POSIX标准，同时兼顾行业最常用的Bash扩展特性，内容由浅入深、示例丰富，适合从零基础到有一定经验的开发者学习。教程的所有示例都可直接运行，配套练习覆盖了日常Shell脚本开发的绝大多数场景。

