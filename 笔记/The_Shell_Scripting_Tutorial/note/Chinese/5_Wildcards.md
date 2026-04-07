# 通配符

如果你之前用过Unix系统，应该对通配符并不陌生。
但它在Shell脚本（Shell script）中的实用场景却不一定是一目了然的。本章节的核心目的是引导你主动思考通配符在Shell脚本中的行为逻辑，预判不同语法的实际作用——相关内容会在后续尤其是[循环](https://www.shellscript.sh/loops.html)章节中用到。
首先思考一下：如何把`/tmp/a`目录下的所有文件复制到`/tmp/b`？如果要复制所有`.txt`文件呢？所有`.html`文件呢？
你大概率会写出以下命令：
```bash
$ cp /tmp/a/* /tmp/b/
$ cp /tmp/a/*.txt /tmp/b/
$ cp /tmp/a/*.html /tmp/b/
```
现在如果不用`ls /tmp/a/`命令，你要怎么列出`/tmp/a/`下的文件？用`echo /tmp/a/*`行不行？这种方式和`ls`的输出有两个核心区别是什么？它有什么优势？又存在什么局限？
要如何把所有`.txt`文件重命名为`.bak`后缀？注意，
```bash
$ mv *.txt *.bak
```
并不能达到预期效果——你可以思考一下，这条命令在被传递给`mv`程序之前，Shell会先对通配符做怎样的展开。如果不好理解，可以把`mv`换成`echo`执行试试。我们后续会深入讲解这个问题，因为它涉及到几个尚未介绍的概念。

---
版权所有 © 2000 - 2026 Steve Parker
本文转自 [https://www.shellscript.sh/wildcards.html](https://www.shellscript.sh/wildcards.html)，如有侵权，请联系删除。

【核心术语对照表】
| 英文原文 | 标准译法 | 概念说明 |
|----------|----------|----------|
| Wildcards | 通配符 | Shell中用于匹配文件名、路径的特殊字符，可快速筛选符合规则的一批文件，常见符号包括`*`、`?`、`[]`等。 |
| Unix | Unix操作系统 | 1969年贝尔实验室研发的多用户、多任务操作系统，是Linux、macOS等类Unix系统的技术原型。 |
| Shell script | Shell脚本 | 类Unix系统下的解释型脚本程序，可批量执行系统命令，广泛用于运维自动化场景。 |
| Loops | 循环 | 编程语言中控制代码块重复执行的控制结构，Shell中常见有`for`、`while`、`until`三类循环。 |
| ls | 列表命令 | 类Unix系统核心命令，用于列出目录下的文件、子目录的属性信息。 |
| echo | 输出命令 | 类Unix系统基础命令，用于将指定内容输出到标准输出流。 |
| cp | 复制命令 | 类Unix系统核心命令，用于复制文件或目录。 |
| mv | 移动/重命名命令 | 类Unix系统核心命令，可用于移动文件位置或修改文件名称。 |

【难句解析】
1. 原文：It is not necessarily obvious how they are useful in shell scripts though. This section is really just to get the old grey cells thinking how things look when you're in a shell script - predicting what the effect of using different syntaxes are.
   译文：但它在Shell脚本中的实用场景却不一定是一目了然的。本章节的核心目的是引导你主动思考通配符在Shell脚本中的行为逻辑，预判不同语法的实际作用。
   解析：原句前半部分为形式主语句型，真正主语是`how`引导的从句，翻译时调整为符合中文表达的语序，避免生硬的机翻腔；`old grey cells`是英语俚语，指代大脑思考能力，结合教学语境意译为“引导你主动思考”，避免直译的生硬感；破折号后的补充内容整合为递进逻辑，符合中文技术文本的表达习惯。
2. 原文：Note that `$ mv *.txt *.bak` will not have the desired effect; think about how this gets expanded by the shell before it is passed to `mv`.
   译文：注意，`$ mv *.txt *.bak` 并不能达到预期效果——你可以思考一下，这条命令在被传递给`mv`程序之前，Shell会先对通配符做怎样的展开。
   解析：原句由分号连接两个关联分句，前半句为提示说明，后半句为引导思考的内容，翻译时将分号调整为破折号，强化逻辑关联，符合中文技术教程的提示风格；`gets expanded by the shell`为被动语态，翻译时调整为主动语态，更符合中文技术文本的表达习惯，避免被动句的生硬感。
3. 原文：We will look into this further later on, as it uses a few concepts not yet covered.
   译文：我们后续会深入讲解这个问题，因为它涉及到几个尚未介绍的概念。
   解析：原句为原因状语从句，后半句`concepts not yet covered`是后置定语修饰`concepts`，翻译时调整为前置定语“尚未介绍的概念”，符合中文的定语语序习惯；`look into this further`结合教学语境意译为“深入讲解这个问题”，避免直译“进一步调查”的语义偏差。
4. 原文：Wildcards are really nothing new if you have used Unix at all before.
   译文：如果你之前用过Unix系统，应该对通配符（Wildcards）并不陌生。
   解析：原句为条件状语从句，主句`are really nothing new`是口语化表达，翻译时调整为符合中文学习者认知的“并不陌生”，避免直译“真的不是新东西”的随意感；同时按照规则在核心术语首次出现时标注英文原文，方便学习者对应记忆。

【背景补充说明】
通配符是Shell编程的基础语法，广泛用于文件批量操作、日志筛选、运维自动化等场景，掌握通配符的Shell展开逻辑是避免脚本常见Bug的核心前提。

