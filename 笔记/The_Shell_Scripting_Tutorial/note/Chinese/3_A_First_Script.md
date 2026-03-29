# 第一个脚本

作为我们的第一个Shell脚本（Shell script），我们会编写一个输出“Hello World”的程序。和你读过的所有其他教程不同，我们会深挖这个Hello World程序背后的核心知识点，绝不止步于简单的输出效果:-)
创建一个文件（[first.sh](https://www.shellscript.sh/eg/first.sh.txt)），内容如下：
```bash
#!/bin/sh
# This is a comment!
echo Hello World        # This is a comment, too!
```
第一行告知Unix系统，该文件需要由`/bin/sh`执行。这几乎是所有Unix系统中Bourne Shell（Bourne shell）的标准路径。如果你使用的是GNU/Linux系统，`/bin/sh`通常是指向bash（近年部分发行版默认改为dash）的符号链接（symbolic link）。
第二行以特殊符号`#`开头：这表示该行是注释（comment），会被Shell完全忽略。
唯一的例外是文件**首行**以`#!`开头的情况——也就是我们示例中的第一行。这是Unix系统的特殊指令，即使你当前使用的交互Shell是csh、ksh或其他任意Shell，也会调用Bourne Shell来解释执行后续的脚本内容。
同理，Perl脚本可以以`#!/usr/bin/perl`开头，告知交互Shell后续的程序需要由Perl执行。在Bourne Shell编程中，我们会统一使用`#!/bin/sh`作为首行。
第三行执行`echo`命令，传入两个参数（parameter/argument）：第一个是`Hello`，第二个是`World`。
注意`echo`会自动在多个参数之间添加单个空格。
`#`符号在这里仍然表示注释，`#`及其之后的所有内容都会被Shell忽略。
现在执行`chmod 755 first.sh`命令，将该文本文件设置为可执行（executable），然后运行`./first.sh`。
你的终端输出应该如下所示：
```bash
$ chmod 755 first.sh
$ ./first.sh
Hello World
$
```
这个结果大概率在你的意料之中。你甚至可以直接在终端运行以下命令得到同样的效果：
```bash
$ echo Hello World
Hello World
$
```
现在我们做几个小改动。
首先要注意：`echo`只会在参数之间添加**一个**空格。如果在“Hello”和“World”之间加多个空格，你认为输出会是什么？如果换成制表符（TAB）呢？
和所有Shell编程的学习方法一样，动手试试就知道了。
输出完全没变！因为我们调用`echo`程序时只传了两个参数，它对参数之间的空白间隔毫不关心，就像`cp`命令也不会在意参数之间的空格数量一样。现在我们再修改代码：
```bash
#!/bin/sh
# This is a comment!
echo "Hello      World"       # This is a comment, too!
```
这次输出就符合预期了，如果你有其他编程语言的使用经验，大概率也能猜到这个结果。但要理解更复杂的命令和Shell脚本，核心是要搞懂**为什么会有这个差异**：
这次调用`echo`时只传入了**一个**参数——也就是字符串`Hello      World`，所以`echo`会原样输出这个字符串。
这里的核心知识点是：Shell会先完成参数解析（parse），再将处理后的参数传递给被调用的程序。在这个例子中，Shell会去掉引号，但会把引号包裹的内容作为单个参数传递。
最后我们再看一个示例，输入以下脚本内容，尝试在运行前预测输出结果：
[first2.sh](https://www.shellscript.sh/eg/first2.sh.txt)
```bash
#!/bin/sh
# This is a comment!
echo "Hello      World"       # This is a comment, too!
echo "Hello World"
echo "Hello * World"
echo Hello * World
echo Hello      World
echo "Hello" World
echo Hello "     " World
echo "Hello "*" World"
echo `hello` world
echo 'hello' world
```
所有结果都符合你的预期吗？如果没有也不用担心！这些都是本教程后续会覆盖的知识点……当然，后续我们还会使用比`echo`更强大的命令！

---



Copyright © 2000 - 2026 Steve Parker
本文转自 [https://www.shellscript.sh/first.html](https://www.shellscript.sh/first.html)，如有侵权，请联系删除。

---



【核心术语对照表】
| 英文原文 | 标准译法 | 概念说明 |
|----------|----------|----------|
| Shell script | Shell脚本 | 为Unix/Linux命令行解释器（Shell）编写的脚本程序，可批量执行命令或完成系统自动化任务，是运维、开发领域的常用工具。 |
| Bourne shell | Bourne Shell | 由Steve Bourne开发的Unix原生标准Shell，是后续bash、dash等Shell变体的基础，标准路径为`/bin/sh`，也译作“伯恩Shell”。 |
| symbolic link | 符号链接 | Unix/Linux系统中的特殊文件类型，指向另一个文件或目录，类似Windows的快捷方式，访问时会跳转至目标路径。 |
| comment | 注释 | 代码中用于说明功能的文本内容，不会被解释器执行，Shell脚本中以`#`开头的内容（首行`#!`除外）均为注释。 |
| executable | 可执行文件 | 具备执行权限、可被系统直接加载运行的文件，Unix/Linux系统中可通过`chmod`命令添加执行权限。 |
| chmod | 变更权限命令 | Unix/Linux系统中用于修改文件/目录权限的标准命令，`755`是常见的脚本权限配置，表示所有者可读可写可执行，其他用户可读可执行。 |
| parameter/argument | 参数 | 调用命令或程序时传入的附加信息，用于指定程序的操作对象或运行行为。 |
| parse | 解析 | Shell的核心处理步骤，指将用户输入的命令行内容拆解为命令名、参数、运算符等语义单元的过程。 |
| echo | echo命令 | Unix/Linux系统中最基础的输出命令，用于将传入的内容打印到标准输出（通常为终端屏幕）。 |

