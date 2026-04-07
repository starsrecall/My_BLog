[课程首页](https://missing.csail.mit.edu/) 
[课程列表](https://missing.csail.mit.edu/2026/) [往届内容](https://missing.csail.mit.edu/past/) [关于课程](https://missing.csail.mit.edu/about/)

# 开发环境与工具（Development Environment and Tools）

**开发环境（Development Environment）**是用于软件开发的工具集合，核心是文本编辑功能，配套能力包括语法高亮、类型检查、代码格式化、自动补全等。**集成开发环境（Integrated Development Environment, IDE）**（比如[VS Code](https://code.visualstudio.com/)）将所有这些功能整合到单一应用中。基于终端的开发工作流则会组合使用各类工具，比如[tmux](https://github.com/tmux/tmux)（终端复用器）、[Vim](https://www.vim.org/)（文本编辑器）、[Zsh](https://www.zsh.org/)（Shell/命令行解释器），以及语言专属的命令行工具，比如[Ruff](https://docs.astral.sh/ruff/)（Python静态代码检查与格式化工具）和[Mypy](https://mypy-lang.org/)（Python类型检查工具）。

IDE和基于终端的工作流各有优劣：比如图形化IDE更容易上手，当前主流IDE的开箱即用AI集成能力（比如AI自动补全）也更完善；另一方面，终端工作流更轻量，在没有**图形用户界面（Graphical User Interface, GUI）**或无法安装软件的环境下，可能是你唯一的选择。我们建议你对两类工作流都建立基础认知，并至少熟练掌握其中一种。如果你还没有偏好的IDE，推荐从[VS Code](https://code.visualstudio.com/)入门。

本章节将覆盖以下内容：
*   [文本编辑与Vim](about:blank#text-editing-and-vim)
*   [代码智能与语言服务器](about:blank#code-intelligence-and-language-servers)
*   [AI辅助开发](about:blank#ai-powered-development)
*   [扩展与其他IDE功能](about:blank#extensions-and-other-ide-functionality)

---

## 文本编辑与Vim
编程时你大部分时间都在代码中跳转、阅读代码片段、修改代码，而非连续写大段代码或从头到尾通读文件。[Vim](https://www.vim.org/)是一款针对这类任务特性优化的文本编辑器。

**Vim的设计哲学**：Vim的核心理念非常优雅——它的操作界面本身就是一门为文本导航和编辑设计的编程语言。按键采用助记命名，对应不同命令，且命令之间可组合使用。Vim不推荐使用鼠标，因为操作太慢；甚至不推荐用方向键，因为手部移动距离过大。最终你会得到一款和思考速度同频、仿佛脑机接口一样的编辑器。

**其他软件中的Vim支持**：你不需要直接使用Vim本身就能受益于它的核心设计思想。几乎所有涉及文本编辑的工具都支持「Vim模式」，要么是内置功能，要么可通过插件实现。比如VS Code有[VSCodeVim](https://marketplace.visualstudio.com/items?itemName=vscodevim.vim)插件，Zsh有Vim模拟的[内置支持](https://zsh.sourceforge.io/Guide/zshguide04.html)，甚至Claude Code也有Vim编辑模式的[内置支持](https://code.claude.com/docs/en/interactive-mode#vim-editor-mode)。几乎所有你用到的文本编辑工具，都或多或少支持Vim模式。

### 模态编辑（Modal Editing）
Vim是一款**模态编辑器**：不同类型的任务对应不同的操作模式，按键在不同模式下有不同含义。
*   **普通模式（Normal Mode）**：用于文件内跳转和编辑操作
*   **插入模式（Insert Mode）**：用于输入文本
*   **替换模式（Replace Mode）**：用于替换文本
*   **可视模式（Visual Mode）**（普通、行、块三种）：用于选中文本块
*   **命令行模式（Command-line Mode）**：用于运行命令

按键在不同模式下含义完全不同：比如字母`x`在插入模式下只会输入字符`x`，但在普通模式下会删除光标所在的字符，在可视模式下会删除选中的内容。

默认配置下，Vim会在左下角显示当前模式，初始默认模式是普通模式，你大部分时间都会在普通模式和插入模式之间切换。
按`<ESC>`（退出键）可以从任意模式切回普通模式；从普通模式出发，按`i`进入插入模式，按`R`进入替换模式，按`v`进入可视模式，按`V`进入行可视模式，按`<C-v>`（Ctrl-V，有时也写作`^V`）进入块可视模式，按`:`进入命令行模式。

使用Vim时你会频繁按`<ESC>`键，建议考虑将大写锁定键（Caps Lock）映射为ESC（[macOS设置指南](https://vim.fandom.com/wiki/Map_caps_lock_to_escape_in_macOS)），或者用简单的按键组合替代`<ESC>`（[替代映射方案](https://vim.fandom.com/wiki/Avoid_the_escape_key#Mappings)）。

### 基础：插入文本
在普通模式下按`i`即可进入插入模式，此时Vim和其他普通文本编辑器的行为完全一致，直到你按`<ESC>`返回普通模式。掌握上述基础操作你就可以开始用Vim编辑文件了——不过如果你一直停留在插入模式编辑，效率会非常低。

### Vim的操作界面是一门编程语言
Vim的操作界面本身就是一门编程语言：采用助记命名的按键对应命令，且命令之间可组合使用。一旦命令操作形成肌肉记忆，就能实现极高的移动和编辑效率，就像学会键盘盲打后输入速度会大幅提升一样。

#### 移动操作
你大部分时间应该处于普通模式，用移动命令在文件内跳转。Vim中的移动操作也叫「名词」，因为它们指代不同的文本块。
*   基础移动：`hjkl`（左、下、上、右）
*   单词跳转：`w`（下一个单词开头）、`b`（上一个单词开头）、`e`（下一个单词结尾）
*   行内跳转：`0`（行首）、`^`（行内第一个非空白字符）、`$`（行尾）
*   屏幕内跳转：`H`（屏幕顶部）、`M`（屏幕中间）、`L`（屏幕底部）
*   滚动：`Ctrl-u`（向上滚动半屏）、`Ctrl-d`（向下滚动半屏）
*   文件内跳转：`gg`（文件开头）、`G`（文件结尾）
*   跳转到指定行：`:{行号}<CR>` 或 `{行号}G`（跳转到第{行号}行）
    * `<CR>`指代回车键（Enter键）
*   配对符号跳转：`%`（跳转到当前括号/大括号的配对符号）
*   行内查找：`f{字符}`、`t{字符}`、`F{字符}`、`T{字符}`
    *  分别表示在当前行向前/向后查找{字符}，跳转到字符位置/字符前一个位置
    *  用`,`/`;`在匹配结果之间切换
*   全局搜索：`/{正则表达式}`，用`n`/`N`在匹配结果之间切换

#### 选择操作
可视模式分为三类：
*   普通可视：`v`
*   行可视：`V`
*   块可视：`Ctrl-v`
进入可视模式后可配合移动命令选中目标文本。

#### 编辑操作
以前用鼠标完成的所有操作，现在都可以用键盘通过「编辑命令+移动命令」的组合实现，这正是Vim界面像编程语言的核心体现。Vim的编辑命令也叫「动词」，因为动词可以作用于名词（移动操作指代的文本块）。
*   `i` 进入插入模式
    *  但如果要操作/删除文本，有比退格键更高效的方式
*   `o` / `O` 在当前行下方/上方插入新行
*   `d{动作}` 删除{动作}覆盖的内容
    *  例如`dw`是删除单词，`d$`是删除到行尾，`d0`是删除到行首
*   `c{动作}` 修改{动作}覆盖的内容
    *  例如`cw`是修改单词
    *  效果等价于执行`d{动作}`后自动进入插入模式
*   `x` 删除光标所在字符（等价于`dl`）
*   `s` 替换光标所在字符（等价于`cl`）
*   可视模式+操作：选中文本后按`d`删除，按`c`修改
*   `u` 撤销操作，`<C-r>` 重做操作
*   `y` 复制/拉取（yank，部分命令如`d`也会自动复制内容）
*   `p` 粘贴内容
*   更多实用操作：比如`~`翻转字符大小写，`J`合并当前行与下一行

#### 计数
你可以将计数与名词、动词组合，让操作重复执行指定次数：
*   `3w` 向前跳转3个单词
*   `5j` 向下移动5行
*   `7dw` 删除7个单词

#### 修饰符
你可以用修饰符改变名词的含义，常用修饰符包括`i`（表示「内部」）和`a`（表示「周围/包含边界」）：
*   `ci(` 修改当前括号对内部的内容
*   `ci[` 修改当前方括号对内部的内容
*   `da'` 删除单引号包裹的字符串，包括两侧的单引号

### 综合实操示例
以下是一段存在问题的[Fizz Buzz](https://en.wikipedia.org/wiki/Fizz_buzz)实现代码：
```python
def fizz_buzz(limit):
    for i in range(limit):
        if i % 3 == 0:
            print("fizz", end="")
        if i % 5 == 0:
            print("fizz", end="")
        if i % 3 and i % 5:
            print(i, end="")
        print()

def main():
    fizz_buzz(20)
```
我们可以用以下Vim命令序列修复问题，所有操作从普通模式开始：
1.  **问题1：main函数没有被调用**
    *   按`G`跳转到文件末尾
    *   按`o`在当前行下方打开新行
    *   输入`if __name__ == "__main__": main()`
        *   如果编辑器有Python语言支持，插入模式下可能会自动处理缩进
    *   按`<ESC>`返回普通模式
2.  **问题2：遍历从0开始而非从1开始**
    *   按`/`输入`range`后按`<CR>`搜索`range`字符串
    *   按`ww`向前跳转2个单词（也可以用`2w`，但实际使用中跳转次数较少时，重复按键比用计数更常见）
    *   按`i`进入插入模式，输入`1,`
    *   按`<ESC>`返回普通模式
    *   按`e`跳转到下一个单词的结尾
    *   按`a`进入追加模式，输入`+ 1`
    *   按`<ESC>`返回普通模式
3.  **问题3：5的倍数仍然输出“fizz”**
    *   按`:6<CR>`跳转到第6行
    *   按`ci"`修改双引号内部的内容，输入`"buzz"`
    *   按`<ESC>`返回普通模式

### 学习Vim的建议
学习Vim的最佳方式是先掌握基础用法（即本节介绍的内容），之后在所有支持Vim模式的软件中开启该功能，在实操中熟练。要刻意避免使用鼠标或方向键；部分编辑器中你甚至可以解绑方向键，强迫自己养成高效操作的习惯。

#### 额外学习资源
*   本课程往期版本的[Vim专题讲座](https://missing.csail.mit.edu/2020/editors/)，对Vim的讲解更深入
*   `vimtutor`是Vim自带的交互式教程，安装Vim后可直接在Shell中运行`vimtutor`学习
*   [Vim Adventures](https://vim-adventures.com/)：通过游戏学习Vim操作
*   [Vim Tips Wiki](https://vim.fandom.com/wiki/Vim_Tips_Wiki)：Vim技巧维基
*   [Vim Advent Calendar](https://vimways.org/2019/)：包含各类Vim实用技巧
*   [VimGolf](https://www.vimgolf.com/)：Vim版「代码高尔夫」，用最少的按键完成指定编辑任务
*   [Vi/Vim Stack Exchange](https://vi.stackexchange.com/)：Vim相关问答社区
*   [Vim Screencasts](http://vimcasts.org/)：Vim操作视频教程
*   [《Practical Vim》](https://pragprog.com/titles/dnvim2/)：Vim经典入门书籍

---

## 代码智能与语言服务器
IDE通常会提供编程语言专属支持，这类支持需要基于代码语义理解实现，通过IDE扩展连接到实现了**语言服务器协议（Language Server Protocol, LSP）**的语言服务器即可获得相关能力。比如VS Code的[Python扩展](https://marketplace.visualstudio.com/items?itemName=ms-python.python)依赖[Pylance](https://marketplace.visualstudio.com/items?itemName=ms-python.vscode-pylance)，VS Code的[Go扩展](https://marketplace.visualstudio.com/items?itemName=golang.go)依赖官方推出的[gopls](https://go.dev/gopls/)。为你常用的编程语言安装对应扩展和语言服务器后，就可以在IDE中启用各类语言专属功能，比如：
*   **代码补全**：更智能的自动补全与建议，比如输入`object.`后会提示该对象的属性和方法
*   **行内文档**：鼠标悬停或自动补全时显示对应代码的文档说明
*   **跳转到定义**：从代码的使用位置跳转到其定义位置，比如从`object.field`的引用处跳转到该字段的定义处
*   **查找引用**：跳转到定义的反向操作，查找某个字段、类型等被引用的所有位置
*   **导入管理**：自动整理导入语句、删除未使用的导入、标记缺失的导入
*   **代码质量检查**：这类工具也可以单独使用，但语言服务器通常也会集成相关能力：代码格式化可自动缩进、统一代码风格；类型检查器和静态检查工具可在你输入时实时发现代码错误。我们会在[代码质量](https://missing.csail.mit.edu/2026/code-quality/)章节更深入地讲解这类功能。

### 配置语言服务器
部分语言只需要安装对应扩展和语言服务器即可直接使用；另一些语言需要向IDE指定开发环境信息，才能最大化发挥语言服务器的能力。比如在VS Code中指定你的Python环境路径，就能让语言服务器识别到已安装的依赖包。我们会在[代码打包与分发](https://missing.csail.mit.edu/2026/shipping-code/)章节更深入地讲解环境相关内容。
不同语言的语言服务器可能支持自定义配置，比如在VS Code的Python支持中，你可以为没有使用Python可选类型注解的项目关闭静态类型检查。

---

## AI辅助开发
自2021年中基于OpenAI [Codex模型](https://openai.com/index/openai-codex/)的[GitHub Copilot](https://github.com/features/copilot/ai-code-editor)推出以来，**大语言模型（Large Language Model, LLM）**已经被广泛应用于软件工程领域。当前AI辅助开发主要有三种形态：自动补全、内嵌聊天、编码代理。

### 自动补全
AI自动补全的形态和IDE中传统自动补全类似，会在你输入时在光标位置给出补全建议，是一种「开箱即用」的被动功能。除此之外，你还可以通过代码注释引导AI补全的结果（即提示词工程）。

比如我们要写一个脚本下载本讲内容并提取所有链接，可以先写：
```python
import requests

def download_contents(url: str) -> str:
```
AI模型会自动补全函数体：
```python
    response = requests.get(url)
    return response.text
```
我们可以通过注释进一步引导补全结果。比如如果要写一个提取所有Markdown链接的函数，但函数名不够清晰：
```python
def extract(contents: str) -> list[str]:
```
模型可能会补全出不相关的结果：
```python
    lines = contents.splitlines()
    return [line for line in lines if line.strip()]
```
这时我们可以通过代码注释引导补全方向：
```python
def extract(content: str) -> list[str]:
    # 从内容中提取所有Markdown链接
```
这次模型会给出更符合预期的结果：
```python
    import re
    pattern = r'\[.*?\]\((.*?)\)'
    return re.findall(pattern, content)
```
这个例子也体现了AI编码工具的一个缺点：它只能在光标位置给出补全建议。上述示例中更规范的做法是将`import re`放在模块顶部，而非函数内部。
上面的例子用了一个命名不够清晰的函数来演示如何用注释引导代码补全；实际开发中你应该编写更具描述性的函数名，比如`extract_links`，同时最好编写函数文档字符串——基于这些信息，模型同样能生成和上面示例类似的补全结果。

为了演示完整流程，我们可以补全整个脚本：
```python
print(extract(download_contents("https://raw.githubusercontent.com/missing-semester/missing-semester/refs/heads/master/_2026/development-environment.md")))
```

### 内嵌聊天（Inline Chat）
内嵌聊天功能允许你选中某一行或某块代码，直接向AI模型发起请求，让它给出修改建议。这种交互模式下，模型可以直接修改现有代码，和仅能在光标后补全内容的自动补全功能有本质区别。

继续上面的示例，假设我们不想使用第三方`requests`库，就可以选中对应的三行代码，唤起内嵌聊天，输入类似这样的指令：
```
use built-in libraries instead
```
模型会给出如下修改建议：
```python
from urllib.request import urlopen

def download_contents(url: str) -> str:
    with urlopen(url) as response:
        return response.read().decode('utf-8')
```

### 编码代理（Coding Agents）
编码代理的相关内容会在[智能编码代理](https://missing.csail.mit.edu/2026/agentic-coding/)章节中深入讲解。

### 推荐工具
当前比较流行的AI辅助开发IDE包括安装了[GitHub Copilot](https://github.com/features/copilot/ai-code-editor)扩展的[VS Code](https://code.visualstudio.com/)，以及[Cursor](https://cursor.com/)。GitHub Copilot目前对学生、教师、热门开源项目维护者[免费开放](https://github.com/education/students)。这个领域发展非常快，主流产品的功能差异不大。

---

## 扩展与其他IDE功能
IDE本身已经是非常强大的工具，**扩展（Extension）**能进一步放大它的能力。我们无法在一讲中覆盖所有功能，这里只介绍几个热门扩展的方向，鼓励你自行探索：网上有很多热门IDE扩展清单，比如Vim插件可以参考[Vim Awesome](https://vimawesome.com/)，VS Code扩展可以查看[按安装量排序的官方市场列表](https://marketplace.visualstudio.com/search?target=VSCode&category=All%20categories&sortBy=Installs)。

*   **开发容器（Development Containers）**：主流IDE均已支持（比如[VS Code的开发容器支持](https://code.visualstudio.com/docs/devcontainers/containers)），它允许你在容器中运行所有开发工具，对提升开发环境的可移植性、隔离性非常有帮助。我们会在[代码打包与分发](https://missing.csail.mit.edu/2026/shipping-code/)章节更深入地讲解容器相关内容。
*   **远程开发**：通过SSH在远程机器上进行开发（比如用VS Code的[Remote SSH插件](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh)）。如果你需要在云端的高性能GPU机器上开发、运行代码，这个功能会非常实用。
*   **协同编辑**：类似Google Docs的多人实时编辑同一文件的能力（比如用VS Code的[Live Share插件](https://marketplace.visualstudio.com/items?itemName=MS-vsliveshare.vsliveshare)）。

---

## 练习题
1.  在你所有支持Vim模式的软件（编辑器、Shell等）中开启Vim模式，接下来一个月的所有文本编辑操作都用Vim模式完成。如果觉得某个操作效率很低，或是你觉得「肯定有更好的方法」，就去搜索一下——大概率确实存在更高效的方案。
2.  完成一道[VimGolf](https://www.vimgolf.com/)的挑战题。
3.  为你正在开发的项目配置IDE扩展和语言服务器，确保所有预期功能（比如跳转到第三方依赖库的定义）都能正常工作。如果没有合适的项目，可以用GitHub上的开源项目练习（比如[这个Go语言项目](https://github.com/spf13/cobra)）。
4.  浏览IDE扩展列表，安装一个你觉得有用的扩展。

---

[编辑此页面](https://github.com/missing-semester/missing-semester/blob/master/_2026/development-environment.md)
本内容采用[CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0)协议授权。

本文转自 [https://missing.csail.mit.edu/2026/development-environment/](https://missing.csail.mit.edu/2026/development-environment/)，如有侵权，请联系删除。

---

【核心术语对照表】
| 英文原文                                 | 标准译法       | 概念说明                                                     |
| ---------------------------------------- | -------------- | ------------------------------------------------------------ |
| Development Environment                  | 开发环境       | 用于软件开发的工具集合，通常包含编辑器、编译/运行环境、调试工具、代码检查工具等组件。 |
| Integrated Development Environment (IDE) | 集成开发环境   | 将代码编辑、编译、调试、版本控制等开发功能整合到单一应用中的软件，常见如VS Code、IntelliJ IDEA等。 |
| Graphical User Interface (GUI)           | 图形用户界面   | 采用图形方式显示的计算机操作界面，与基于文本的命令行界面相对。 |
| Modal Editing                            | 模态编辑       | Vim的核心设计，不同操作模式下按键有不同含义，可大幅提升编辑效率，避免大量快捷键组合。 |
| Normal Mode                              | 普通模式       | Vim的默认模式，用于代码跳转、编辑操作，是Vim高效操作的核心模式。 |
| Insert Mode                              | 插入模式       | Vim的文本输入模式，行为和普通文本编辑器一致。                |
| Visual Mode                              | 可视模式       | Vim的文本选择模式，分为普通、行、块三类，可精准选中不同范围的文本进行批量操作。 |
| Command-line Mode                        | 命令行模式     | Vim的命令执行模式，用于运行保存、退出、查找替换、跳转等命令。 |
| Language Server Protocol (LSP)           | 语言服务器协议 | 微软推出的通用语言服务协议，统一了IDE和语言分析工具的通信标准，让不同IDE可以复用同一套语言智能服务。 |
| Large Language Model (LLM)               | 大语言模型     | 具备大规模参数、通用语言理解生成能力的AI模型，是当前AI辅助开发的技术基础。 |
| Prompt Engineering                       | 提示词工程     | 通过设计输入文本引导大语言模型输出符合预期结果的技术，是AI辅助开发中的常用技巧。 |
| Development Containers                   | 开发容器       | 将完整开发环境打包在容器中的技术，可实现开发环境的一键复用、跨设备一致性。 |
| SSH                                      | 安全外壳协议   | 一种加密的网络传输协议，常用于远程登录服务器、安全传输文件，是远程开发的核心基础。 |

【背景补充说明】
本文出自麻省理工学院（MIT）知名公开课程《计算机科学教育中缺失的一学期》（Missing Semester of CS Education），课程聚焦大学计算机专业常规教学中很少涉及但实用性极强的工程工具、开发效率技巧，是全球开发者公认的必备入门学习资源。
1. Vim诞生于1991年，是Vi编辑器的增强版本，至今仍是类Unix系统默认预装的文本编辑器，其模态编辑设计被几乎所有主流编辑器、IDE、甚至笔记软件、浏览器支持，是提升文本编辑效率的通用技能。
2. 语言服务器协议（LSP）由微软2016年推出，目前已成为行业标准，彻底解决了之前不同IDE需要为每门语言单独开发智能服务的重复劳动问题，现在几乎所有主流编程语言都有官方或社区维护的LSP实现。
3. GitHub Copilot的学生免费申请需要通过GitHub Student Developer Pack认证，除Copilot外还可以免费获得大量开发工具、云服务资源，是学生开发者的重要福利。
4. 文中提到的将Caps Lock映射为ESC是Vim用户的通用优化技巧，Windows/Linux系统也可以通过系统设置、AutoHotkey等工具实现该映射，可大幅降低Vim操作的手部移动成本。