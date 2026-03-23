# week_0_note_Chinese
---

> 这里是CS50x 2026版本 🎉 如果你是2025年或更早之前开始学习本课程，想知道之前的学习进度如何衔接2026版课程，请查看[常见问题解答](https://cs50.harvard.edu/faqs/#if-i-started-cs50x-before-this-calendar-year-can-i-resume)。如需了解[认证证书、专业证书、学分转换与课程认证](https://cs50.harvard.edu/#how-to-take-this-course)相关信息，可点击对应链接查看。

# [这里是CS50](https://cs50.harvard.edu/)
[许可协议](https://cs50.harvard.edu/license/)

## 第0讲
*   [欢迎！](#welcome)
*   [Visual Studio Code 与 chat.py](#visual-studio-code-and-chatpy)
*   [计算机科学与问题解决](#computer-science-and-problem-solving)
*   [ASCII 编码](#ascii)
*   [Unicode 编码](#unicode)
*   [RGB 色彩模型](#rgb)
*   [算法](#algorithms)
*   [伪代码](#pseudocode)
*   [后续课程安排](#whats-ahead)
*   [Scratch 可视化编程](#scratch)
*   [Hello World 示例](#hello-world)
*   [打招呼程序](#hello-you)
*   [喵叫、循环与抽象](#meow-loops-and-abstraction)
*   [条件判断](#conditionals)
*   [Oscartime 小游戏](#oscartime)
*   [Ivy的硬核小游戏](#ivys-hardest-game)
*   [总结](#summing-up)

---

<h2 id="welcome">欢迎！</h2>

人工智能（AI）正在为计算机科学乃至整个世界带来前所未有的突破与发展机遇。
虽然AI技术进步巨大，有时甚至能消除拖慢流程的人力瓶颈，但**理解代码、编写代码、组织代码逻辑的能力，会让你通过编程成为规则的制定者、技术的掌舵人、拥有创造力的开发者**。
因此，请不要认为AI的出现会让学习编程基础变得没有必要；恰恰相反，掌握基础原理再加上AI的赋能，会为你和你所服务的对象带来全新的可能性。

<h2 id="visual-studio-code-and-chatpy">Visual Studio Code 与 chat.py</h2>

VS Code是一款**集成开发环境（IDE）**，你可以在其中编写代码。
为了让你对后续学习内容有个直观感受，我们可以编写一个名为`chat.py`的聊天机器人程序。
在已经配置好OpenAI开发库的系统中，你可以编写如下代码：

```python
from openai import OpenAI

client = OpenAI()

response = client.responses.create(
    input="用一句话说明什么是CS50？",
    model="gpt-5"
)

print(response.output_text)
```
可以看到，我们首先导入了OpenAI的库以使用其提供的能力，接着创建了一个聊天客户端，然后将名为`input`的问题传给客户端请求回答，最后打印得到的`response`（响应）。

我们可以优化这段代码，让用户可以输入自定义问题：
```python
from openai import OpenAI

client = OpenAI()

prompt = input("请输入问题：")

response = client.responses.create(
    input=prompt,
    model="gpt-5"
)

print(response.output_text)
```
这里我们创建了`prompt`变量，用来接收用户输入的问题。

我们还可以进一步优化，增加`system_prompt`（系统提示词），为聊天机器人提供额外的上下文和行为规则：
```python
from openai import OpenAI

client = OpenAI()

user_prompt = input("请输入问题：")
system_prompt = "用一句话回答问题，并且模仿猫的语气。"

response = client.responses.create(
    input=user_prompt,
    instructions=system_prompt,
    model="gpt-5"
)

print(response.output_text)
```
这里的`system_prompt`用来给模型提供额外的上下文和指令。

仅仅用10行代码，你就能构建出功能非常强大的程序！
我们还打造了专属的「橡皮鸭」调试助手——[CS50小鸭子](https://cs50.ai/)，你在课程学习过程中遇到问题都可以向它求助。
请注意遵守课程的[学术诚信政策](https://cs50.harvard.edu/syllabus/#academic-honesty)：**除了CS50小鸭子外，禁止使用任何其他AI工具完成作业**。

<h2 id="computer-science-and-problem-solving">计算机科学与问题解决</h2>

本质上，计算机编程就是接收输入、生成输出，以此解决问题的过程。输入和输出之间的「黑盒」，就是本课程的核心学习内容。
```
flowchart LR
    in["输入"] --> BOX[" ??? 黑盒 ??? "]
    BOX --> out["输出"]
```

举个例子，如果我们需要为班级点名，可以用**一进制（也叫base-1）**系统，掰着手指头一个个数。
而现代计算机使用**二进制（也叫base-2）**计数。我们熟悉的术语「比特（bit）」就来自「二进制位（binary digit）」的缩写，1个比特就是一个0或1：代表电路的关或开。
计算机只懂0和1：0代表「关」，1代表「开」。计算机内部是由数百万甚至数十亿个不断切换开关状态的晶体管组成的。
你可以把单个晶体管想象成一个灯泡：单个灯泡只能表示0（灭）或1（亮），最多计数到1。
但如果有3个灯泡，就能表示更多状态了！
你的iPhone、电脑等设备内部有数百万个这种「灯泡」一样的晶体管，支撑着我们每天习以为常的各种设备功能。
我们可以用如下位权来表示二进制每个位置的数值：
```
4 2 1
```
用3个灯泡的话，全部熄灭就代表0：
```
4 2 1
0 0 0
```
同理，以下状态代表1：
```
4 2 1
0 0 1
```
按照这个逻辑，以下状态代表2：
```
4 2 1
0 1 0
```
以此类推，以下状态代表3：
```
4 2 1
0 1 1
```
4的表示方式是：
```
4 2 1
1 0 0
```
仅用3个灯泡，我们最多可以计数到7：
```
4 2 1
1 1 1
```
计算机使用二进制计数，位权本质是2的幂次：
```
2^2  2^1  2^0
4    2    1
```
因此可以说，要表示最大为7的数，需要3个比特（4位、2位、1位）。
同理，要表示8的话，就需要4个比特：
```
8 4 2 1
1 0 0 0
```
计算机通常用8个比特（也叫**字节（byte）**）来表示一个数字。比如`00000101`是二进制的5，`11111111`是二进制的255。0的8比特表示如下：

| 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |

<h2 id="ascii">ASCII 编码</h2>

正如数字可以用0和1的模式表示，字母也可以用0和1表示！
由于表示数字和字母的01序列会有重叠，因此人们制定了**ASCII**标准，将特定字母映射到特定数字。
比如字母`A`被映射到数字65，二进制的`01000001`就代表65，可视化如下：

| 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | 1 | 0 | 0 | 0 | 0 | 0 | 1 |

如果你收到一条短信，底层的二进制序列可能代表数字72、73、33，通过ASCII映射后，对应的文本就是：
```
H   I   !
72  73  33
```
多亏了ASCII这样的统一标准，我们才能对数值的含义达成共识！
完整的ASCII编码映射表如下：

<table><tbody><tr><td><strong>0</strong></td><td>NUL</td><td><strong>16</strong></td><td>DLE</td><td><strong>32</strong></td><td>SP</td><td><strong>48</strong></td><td>0</td><td><strong>64</strong></td><td>@</td><td><strong>80</strong></td><td>P</td><td><strong>96</strong></td><td>`</td><td><strong>112</strong></td><td>p</td></tr><tr><td><strong>1</strong></td><td>SOH</td><td><strong>17</strong></td><td>DC1</td><td><strong>33</strong></td><td>!</td><td><strong>49</strong></td><td>1</td><td><strong>65</strong></td><td>A</td><td><strong>81</strong></td><td>Q</td><td><strong>97</strong></td><td>a</td><td><strong>113</strong></td><td>q</td></tr><tr><td><strong>2</strong></td><td>STX</td><td><strong>18</strong></td><td>DC2</td><td><strong>34</strong></td><td>”</td><td><strong>50</strong></td><td>2</td><td><strong>66</strong></td><td>B</td><td><strong>82</strong></td><td>R</td><td><strong>98</strong></td><td>b</td><td><strong>114</strong></td><td>r</td></tr><tr><td><strong>3</strong></td><td>ETX</td><td><strong>19</strong></td><td>DC3</td><td><strong>35</strong></td><td>#</td><td><strong>51</strong></td><td>3</td><td><strong>67</strong></td><td>C</td><td><strong>83</strong></td><td>S</td><td><strong>99</strong></td><td>c</td><td><strong>115</strong></td><td>s</td></tr><tr><td><strong>4</strong></td><td>EOT</td><td><strong>20</strong></td><td>DC4</td><td><strong>36</strong></td><td>$</td><td><strong>52</strong></td><td>4</td><td><strong>68</strong></td><td>D</td><td><strong>84</strong></td><td>T</td><td><strong>100</strong></td><td>d</td><td><strong>116</strong></td><td>t</td></tr><tr><td><strong>5</strong></td><td>ENQ</td><td><strong>21</strong></td><td>NAK</td><td><strong>37</strong></td><td>%</td><td><strong>53</strong></td><td>5</td><td><strong>69</strong></td><td>E</td><td><strong>85</strong></td><td>U</td><td><strong>101</strong></td><td>e</td><td><strong>117</strong></td><td>u</td></tr><tr><td><strong>6</strong></td><td>ACK</td><td><strong>22</strong></td><td>SYN</td><td><strong>38</strong></td><td>&amp;</td><td><strong>54</strong></td><td>6</td><td><strong>70</strong></td><td>F</td><td><strong>86</strong></td><td>V</td><td><strong>102</strong></td><td>f</td><td><strong>118</strong></td><td>v</td></tr><tr><td><strong>7</strong></td><td>BEL</td><td><strong>23</strong></td><td>ETB</td><td><strong>39</strong></td><td>’</td><td><strong>55</strong></td><td>7</td><td><strong>71</strong></td><td>G</td><td><strong>87</strong></td><td>W</td><td><strong>103</strong></td><td>g</td><td><strong>119</strong></td><td>w</td></tr><tr><td><strong>8</strong></td><td>BS</td><td><strong>24</strong></td><td>CAN</td><td><strong>40</strong></td><td>(</td><td><strong>56</strong></td><td>8</td><td><strong>72</strong></td><td>H</td><td><strong>88</strong></td><td>X</td><td><strong>104</strong></td><td>h</td><td><strong>120</strong></td><td>x</td></tr><tr><td><strong>9</strong></td><td>HT</td><td><strong>25</strong></td><td>EM</td><td><strong>41</strong></td><td>)</td><td><strong>57</strong></td><td>9</td><td><strong>73</strong></td><td>I</td><td><strong>89</strong></td><td>Y</td><td><strong>105</strong></td><td>i</td><td><strong>121</strong></td><td>y</td></tr><tr><td><strong>10</strong></td><td>LF</td><td><strong>26</strong></td><td>SUB</td><td><strong>42</strong></td><td>*</td><td><strong>58</strong></td><td>:</td><td><strong>74</strong></td><td>J</td><td><strong>90</strong></td><td>Z</td><td><strong>106</strong></td><td>j</td><td><strong>122</strong></td><td>z</td></tr><tr><td><strong>11</strong></td><td>VT</td><td><strong>27</strong></td><td>ESC</td><td><strong>43</strong></td><td>+</td><td><strong>59</strong></td><td>;</td><td><strong>75</strong></td><td>K</td><td><strong>91</strong></td><td>[</td><td><strong>107</strong></td><td>k</td><td><strong>123</strong></td><td>{</td></tr><tr><td><strong>12</strong></td><td>FF</td><td><strong>28</strong></td><td>FS</td><td><strong>44</strong></td><td>,</td><td><strong>60</strong></td><td>&lt;</td><td><strong>76</strong></td><td>L</td><td><strong>92</strong></td><td>\</td><td><strong>108</strong></td><td>l</td><td><strong>124</strong></td><td>|</td></tr><tr><td><strong>13</strong></td><td>CR</td><td><strong>29</strong></td><td>GS</td><td><strong>45</strong></td><td>-</td><td><strong>61</strong></td><td>=</td><td><strong>77</strong></td><td>M</td><td><strong>93</strong></td><td>]</td><td><strong>109</strong></td><td>m</td><td><strong>125</strong></td><td>}</td></tr><tr><td><strong>14</strong></td><td>SO</td><td><strong>30</strong></td><td>RS</td><td><strong>46</strong></td><td>.</td><td><strong>62</strong></td><td>&gt;</td><td><strong>78</strong></td><td>N</td><td><strong>94</strong></td><td>^</td><td><strong>110</strong></td><td>n</td><td><strong>126</strong></td><td>~</td></tr><tr><td><strong>15</strong></td><td>SI</td><td><strong>31</strong></td><td>US</td><td><strong>47</strong></td><td>/</td><td><strong>63</strong></td><td>?</td><td><strong>79</strong></td><td>O</td><td><strong>95</strong></td><td>_</td><td><strong>111</strong></td><td>o</td><td><strong>127</strong></td><td>DEL</td></tr></tbody></table>



如果你想了解更多相关内容，可以查看[ASCII的维基百科页面](https://en.wikipedia.org/wiki/ASCII)。
如果每个字符固定用8位的1字节存储，最多只能编码256个不同的字符，ASCII仅用到了其中128个（编号0-127）。

<h2 id="unicode">Unicode 编码</h2>

随着时间推移，人们用文本交流的场景越来越丰富。
由于二进制位数不足，无法表示人类使用的所有字符，因此人们制定了**Unicode**标准，扩展了计算机可以传输和识别的比特位数。Unicode不仅包含特殊字符，还覆盖了表情符号（emoji）。
你平时大概率每天都会用到表情符号，这些你应该很熟悉：
😀 😃 😄 😁 😆 😅 😂 🙂 🙃 😉 😊 😇 😍 😘 😗 😙 😚 😋 😛 😜 😝 🤑 🤓 😎 🤗 😏 😶 😐 😑 😒 🙄 😬 😕 ☹️ 😟 😮 😯 😲 😳 😦 😧 😨
虽然Unicode对01序列的编码是统一的，但不同设备厂商对同一个emoji的渲染显示可能略有差异。
Unicode标准还在不断更新，加入更多字符和emoji。
如果你想了解更多，可以查看[Unicode的维基百科页面](https://en.wikipedia.org/wiki/Unicode)，也可以了解更多[emoji相关知识](https://en.wikipedia.org/wiki/Emoji)。

<h2 id="rgb">RGB 色彩模型</h2>

0和1还可以用来表示颜色。
红（Red）、绿（Green）、蓝（Blue）三种颜色的数值组合构成了`RGB`色彩模型。
```
72 73 33
```
我们之前用来表示文本`HI!`的数字72、73、33，如果被图像解析器读取，会被识别为浅黄的色调：红色分量是72，绿色分量是73，蓝色分量是33。
表示红、绿、蓝三种颜色分量的三个字节，构成了数字图像中每个**像素（pixel，即图像的最小光点）**的颜色。图像本质上就是大量RGB数值的集合。
0和1不仅能表示图像，还能表示视频、音乐！
视频就是大量连续图像的集合，就像翻页动画一样。
音乐也可以通过不同的字节组合来表示。

<h2 id="algorithms">算法</h2>

解决问题是计算机科学和编程的核心。**算法**就是解决某个问题的一步步明确指令。
想象一个基础问题：要在电话本里找到某一个人的名字。
你会怎么做？
第一种方法：从第一页开始，一页一页往后翻，直到翻到最后一页。
第二种方法：一次翻两页，加快查找速度。
第三种（也是更优的）方法：直接翻到电话本的中间，判断「我要找的名字是在左边还是右边？」，然后重复这个过程，每次把需要查找的范围缩小一半。
以上每一种方法都可以称为算法。我们可以用**大O表示法（big-O notation）**来直观表示不同算法的速度：
> （图表说明：纵轴为解决时间，横轴为问题规模，三条曲线分别对应`n`、`n/2`、`log₂n`的增长趋势）

可以看到，第一种算法的大O复杂度是`n`：如果电话本里有100个名字，最多需要找100次才能找到目标。第二种一次翻两页的算法，大O复杂度是`n/2`，查找速度是第一种的两倍。最后一种算法的大O复杂度是`log₂n`：即使问题规模翻倍，也只需要多执行一步就能解决问题。
程序员的工作就是把人类能读懂的文字指令，翻译成**代码**来解决问题。

<h2 id="pseudocode">伪代码</h2>

伪代码是人类可读的指令，通常用来描述算法的执行步骤。
编写**伪代码**的能力，不管是对本课程的学习，还是对未来的编程工作都至关重要。
比如上面提到的第三种查找电话本的算法，我们可以写出如下伪代码：
```
1  拿起电话本
2  翻到电话本中间页
3  查看当前页的内容
4  如果要找的人在这一页
5      打电话给这个人
6  否则如果要找的人在本子的前半部分
7      翻到左半部分的中间页
8      回到第3步
9  否则如果要找的人在本子的后半部分
10     翻到右半部分的中间页
11     回到第3步
12 否则
13     退出
```
伪代码是非常重要的技能，至少有两个原因：第一，在写正式代码之前先写伪代码，可以帮你提前梳理清楚问题的逻辑；第二，伪代码可以分享给其他开发者，让他们更容易理解你的编码思路和代码的工作原理。
注意我们的伪代码里有几个特殊的结构：第一，有些行以「拿起」「翻到」「查看」这类动词开头，后面我们会把这类结构叫做**函数**。
第二，有些行包含`if`（如果）或者`else if`（否则如果）这类语句，这些叫做**条件判断**。
第三，有些语句的结果可以被判定为「真」或「假」，比如「要找的人在本子的前半部分」，这类语句叫做**布尔表达式**。
最后，有些语句比如「回到第3步」，我们称之为**循环**。
这些就是编程的基础组成模块。
我们在后面要讲的Scratch编程语言中，就会用到上面提到的所有编程基础模块。

<h2 id="whats-ahead">后续课程安排</h2>

本周你会学习Scratch——一种可视化编程语言。
之后的几周，你会学习C语言，代码看起来会是这样：
```c
#include <stdio.h>
int main(void)
{
  printf("hello, world\n");
}
```
学好C语言会为你之后学习Python等其他编程语言打下非常扎实的基础。
你会发现程序员会通过**抽象**来站在前人的肩膀上开发：我们不需要直接用0和1编程，编程语言的出现就是为了把极其复杂的二进制编程任务「抽象」掉，让我们可以使用越来越易用的编程语言开发，我们完全可以站在前人的成果上继续前进！

<h2 id="scratch">Scratch 可视化编程</h2>

**Scratch**是麻省理工学院（MIT）开发的可视化编程语言。
[Scratch官网](https://scratch.mit.edu/)用到的核心编程模块，和我们刚才在讲座里提到的基础模块完全一致。
Scratch是非常适合编程入门的工具，因为你可以用可视化的方式操作这些编程模块，不需要纠结花括号、分号、括号这类语法细节。
Scratch的集成开发环境（IDE）界面如下：
![Scratch界面](https://cs50.harvard.edu/cs50Week0Slide162.png "Scratch界面")
可以看到，左侧是编程可用的「模块面板」，你可以把模块拖到中间的编程区拼接成程序；编程区右侧是「舞台」，上面站着一只小猫，你的程序运行效果就会在舞台上呈现。
Scratch的坐标系如下：
![Scratch坐标系](https://cs50.harvard.edu/cs50Week0Slide167.png "Scratch坐标系")
舞台的中心点坐标是(0,0)，默认情况下小猫就在这个位置。

<h2 id="hello-world">Hello World 示例</h2>

首先，把「当绿色旗帜被点击」模块拖到编程区，再把「说」模块拖过来，拼接到前一个模块下面：
```
当绿色旗帜被点击
说 [hello, world]
```
点击舞台上的绿色旗帜，小猫就会说出「hello, world」。这个例子非常好地阐释了我们之前讨论的编程逻辑：
![Scratch黑盒示例](https://cs50.harvard.edu/cs50Week0Slide172.png "Scratch黑盒示例")
输入`hello, world`被传给「说」函数，函数运行的**副作用**就是小猫说出了`hello, world`。

<h2 id="hello-you">打招呼程序</h2>

我们可以让程序更有交互性，让小猫对特定的人打招呼，修改代码如下：
```
当绿色旗帜被点击
询问 [你叫什么名字？] 并等待
说 (拼接 [hello,] (回答))
```
点击绿色旗帜后，「询问」函数会运行，提示用户输入「你叫什么名字？」，然后把用户输入的名字存储在名为`answer`（回答）的**变量**里。接下来程序把`answer`传给「拼接」函数，把文本`hello,`和用户输入的名字这两个字符串组合起来。`answer`的值作为参数传给「拼接」函数，拼接后的结果再传给「说」函数，小猫就会说出「Hello, 你的名字」，你的程序现在就有交互性了。本课程的学习中，你会不断地给算法输入内容、得到输出，用上面的程序来可视化就是：
![Scratch算法示例](https://cs50.harvard.edu/cs50Week0Slide169.png "hello和answer被传给拼接函数，得到hello david")
输入`hello,`和`answer`被传给「拼接」函数，返回`hello, David`，这个返回值再被传给「说」函数，产生小猫说话的副作用。我们还可以再修改一下程序：

```
当绿色旗帜被点击
询问 [你叫什么名字？] 并等待
朗读 (拼接 [hello,] (回答))
```
点击绿色旗帜后，程序会把和`hello`拼接后的变量传给「朗读」函数，实现语音播报效果。

<h2 id="meow-loops-and-abstraction">喵叫、循环与抽象</h2>

除了编写伪代码，**抽象**也是计算机编程中非常核心的技能和概念。
抽象就是把复杂的大问题拆解成越来越小的小问题的过程。
比如你要给朋友们办一场大型晚宴，「做一整顿饭」这个任务听起来会非常让人头大！但如果你把做饭的任务拆成一个一个小任务（小问题），做出一顿美味大餐的目标就不会看起来那么难了。
在编程甚至Scratch里，我们都能看到抽象的实际应用。在编程区写如下代码：
```
当绿色旗帜被点击
播放声音 (喵叫 v) 直到结束
等待 (1) 秒
播放声音 (喵叫 v) 直到结束
等待 (1) 秒
播放声音 (喵叫 v) 直到结束
```
你会发现你在重复做完全一样的事情。如果你发现自己在反复写相同的代码，大概率说明你可以用更优雅的方式编程——把重复的代码抽象出来。
你可以把代码改成这样：
```
当绿色旗帜被点击
重复 (3) 次
  播放声音 (喵叫 v) 直到结束
  等待 (1) 秒
```
这个循环的效果和之前的程序完全一样，但我们把重复的逻辑抽象成了「重复」模块，简化了问题。
我们还可以更进一步，用「定义」模块创建你自己的模块（也就是你自己的函数）！写如下代码：
```
定义 喵叫
  播放声音 (喵叫 v) 直到结束
  等待 (1) 秒

当绿色旗帜被点击
重复 (3) 次
  喵叫
```
我们定义了一个叫做「喵叫」的模块，这个函数的功能是播放喵叫的声音，然后等待1秒。在绿色旗帜被点击后，「喵叫」函数会被重复执行3次。
我们甚至可以给函数加一个输入参数`n`，让它可以自定义重复次数：
```
定义 喵叫 n 次
  重复 (n) 次
    播放声音 [喵叫 v] 直到结束
    等待 (1) 秒
```
这里的`n`是从「喵叫 n 次」的定义里传入的，通过「定义」模块把参数传递给喵叫函数。
总的来说，你可以看到这个逐步优化的过程让代码的设计越来越好，而且我们创建了自己的算法来解决问题。这两项技能会贯穿你整个课程的学习过程。

<h2 id="conditionals">条件判断</h2>

**条件判断**是编程的核心组成模块，程序会检查特定条件是否被满足，如果满足就执行对应的操作。
为了演示条件判断，写如下代码：
```
当绿色旗帜被点击
永远重复
  如果 <碰到 (鼠标指针 v)?> 那么
    播放声音 (喵叫 v) 直到结束
```
这里用了「永远重复」模块，让`if`判断不断触发，持续检查小猫是否碰到了鼠标指针。
我们还可以修改程序，加入视频检测功能：
```
当视频运动 > (10)
播放声音 (喵叫 v) 直到结束
```
记住，编程往往是一个反复试错的过程。如果你觉得受挫了，不妨花点时间把当前的问题梳理清楚：你现在正在解决的具体问题是什么？哪些部分是正常工作的？哪些部分出了问题？

<h2 id="oscartime">Oscartime 小游戏</h2>

**Oscartime**是David自己写的Scratch程序——不过里面的背景音乐可能是他的「心理阴影」，因为他开发这个游戏的时候听了几百个小时。你可以花几分钟玩一下[Oscartime游戏](https://scratch.mit.edu/projects/277537196)。我们自己来搭建Oscartime，首先添加路灯角色：
![Oscartime界面](https://cs50.harvard.edu/cs50Week0Scratch10.png "Oscartime界面")

然后写如下代码：

```
当绿色旗帜被点击
切换造型为 (oscar1 v)
永远重复
  如果 <碰到 (鼠标指针 v)?> 那么
    切换造型为 (oscar2 v)
  否则
    切换造型为 (oscar1 v)
```
把鼠标移到Oscar身上的时候，他的造型就会变化。你可以[查看这些代码块的完整项目](https://scratch.mit.edu/studios/50827060/)了解更多细节。
接下来修改代码，实现下落的垃圾角色：

```
当绿色旗帜被点击
设置拖拽模式 [可拖拽 v]
移动到 x: (从 (0) 到 (240) 随机数) y: (180)
永远重复
  将y增加 (-1)
结束
```
垃圾的y轴初始位置永远是180，x轴位置是随机的，只要垃圾还在地面上方，就会每次向下移动1像素。你可以[查看这些代码块的完整项目](https://scratch.mit.edu/projects/726782167)了解更多。
接下来修改代码，实现拖拽垃圾的功能：
```
当绿色旗帜被点击
永远重复
  如果 <碰到 [Oscar v] ?> 那么
    移动到 x: (从 (0) 到 (240) 随机数) y: (180)
  结束
结束
```
你可以[查看这些代码块的完整项目](https://scratch.mit.edu/projects/726780064)了解更多。
接下来我们可以实现计分变量：
```
当绿色旗帜被点击
设置拖拽模式 [可拖拽 v]
移动到顶部
永远重复
  将y增加 (-1)
结束
```
```
当绿色旗帜被点击
永远重复
  如果 <碰到 [Oscar v] ?> 那么
    移动到顶部
  结束
结束
```
```
定义 移动到顶部
  移动到 x: (从 (0) 到 (240) 随机数) y: (180)
```
你可以[查看这些代码块的完整项目](https://scratch.mit.edu/projects/726779570)了解更多。
可以去体验完整版的[Oscartime游戏](https://scratch.mit.edu/projects/277537196)。

<h2 id="ivys-hardest-game">Ivy的硬核小游戏</h2>

看完Oscartime，我们来看Ivy的硬核小游戏，现在我们可以学习怎么在程序里实现移动功能。
这个程序有三个核心部分。
第一部分，写如下代码：

```
当绿色旗帜被点击
移动到 x: (0) y: (0)
永远重复
  监听键盘
  检测墙壁
```
点击绿色旗帜后，角色会移动到舞台中心(0,0)的位置，然后永远重复执行监听键盘和检测墙壁的逻辑。
第二部分，添加这段代码：
```
定义 监听键盘
如果 <按键 (上方向键 v) 按下?> 那么
  将y增加(1)
结束
如果 <按键 (下方向键 v) 按下?> 那么
  将y增加 (-1)
结束
如果 <按键 (右方向键 v) 按下?> 那么
  将x增加 (1)
结束
如果 <按键 (左方向键 v) 按下?> 那么
  将x增加 (-1)
结束
```
可以看到我们创建了一个自定义的「监听键盘」脚本，按下键盘上的四个方向键时，就会控制角色在屏幕上移动。

最后，添加第三部分代码块：
```
定义 检测墙壁
如果 <碰到 (左侧墙壁 v) ?> 那么
  将x增加 (1)
结束
如果 <碰到 (右侧墙壁 v) ?> 那么
  将x增加 (-1)
结束
```
我们还做了一个自定义的「检测墙壁」脚本：当角色碰到墙壁时，会把它移回安全位置，避免角色移出屏幕。
你可以[查看这些代码块的完整项目](https://scratch.mit.edu/projects/577647301/)了解更多细节。

Scratch支持同时在屏幕上放置多个角色。我们再添加一个耶鲁大学的角色，写入如下代码：
```
当绿色旗帜被点击
移动到 x: (0) y: (0)
指向方向 (90)
永远重复
  如果 <<碰到 (左侧墙壁 v)?> 或 <碰到 (右侧墙壁 v)?>> 那么
    右转 (180) 度
  结束
  移动 (1) 步
结束
```
你会看到耶鲁的角色会来回移动，挡住哈佛角色的路：它撞到墙就会掉头，一直往返移动。你可以[查看这些代码块的完整项目](https://scratch.mit.edu/projects/577647503/)了解更多细节。

你甚至可以让一个角色跟随另一个角色。我们再添加麻省理工（MIT）的logo作为新角色，写入如下代码：
```
当绿色旗帜被点击
移动到 (随机位置 v)
永远重复
  朝向 (哈佛 v)
  移动 (1) 步
```
你会看到MIT的logo会一直跟着哈佛的角色移动。你可以[查看这些代码块的完整项目](https://scratch.mit.edu/projects/577647729/)了解更多细节。
你可以去体验完整版的[Ivy的硬核小游戏](https://scratch.mit.edu/projects/565742837)。

<h2 id="summing-up">总结</h2>
本节课你了解了本课程在计算机科学和编程领域的定位，学到了以下内容：
*   虽然AI可以帮助消除人力瓶颈，但学习计算机科学基础和编程的核心组成模块，能让你更好地利用不断涌现的新技术。
*   本课程中使用AI的正确与错误方式（仅允许使用CS50小鸭子，禁止其他AI工具完成作业）。
*   解决问题是计算机科学家工作的核心。
*   计算机是如何表示和理解数字、文本、图像、音乐和视频的。
*   编程的基础技能——编写伪代码。
*   抽象能力会在你后续的课程学习中发挥怎样的作用。
*   编程的基础组成模块：函数、条件判断、循环和变量。
*   如何在Scratch中创建项目。

以上就是CS50第0讲的全部内容！欢迎加入课程！我们下次课见！

---
本文转自 [https://cs50.harvard.edu/x/notes/0/](https://cs50.harvard.edu/x/notes/0/)，如有侵权，请联系删除。