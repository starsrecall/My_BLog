This is CS50x 2026. 🎉 Curious how your 2025 work counts toward the 2026 course? See our [FAQs](https://cs50.harvard.edu/faqs/#if-i-started-cs50x-before-this-calendar-year-can-i-resume) if you started in 2025 or earlier. Interested in [a verified certificate, a professional certificate, or transfer credit and accreditation](https://cs50.harvard.edu/#how-to-take-this-course)?

[This is CS50](https://cs50.harvard.edu/)
=========================================

https://cs50.harvard.edu/license/)

Lecture 0
=========

*   [Welcome!](about:blank#welcome)
*   [Visual Studio Code and chat.py](about:blank#visual-studio-code-and-chatpy)
*   [Computer Science and Problem Solving](about:blank#computer-science-and-problem-solving)
*   [ASCII](about:blank#ascii)
*   [Unicode](about:blank#unicode)
*   [RGB](about:blank#rgb)
*   [Algorithms](about:blank#algorithms)
*   [Pseudocode](about:blank#pseudocode)
*   [What’s Ahead](about:blank#whats-ahead)
*   [Scratch](about:blank#scratch)
*   [Hello World](about:blank#hello-world)
*   [Hello, You](about:blank#hello-you)
*   [Meow, Loops, and Abstraction](about:blank#meow-loops-and-abstraction)
*   [Conditionals](about:blank#conditionals)
*   [Oscartime](about:blank#oscartime)
*   [Ivy’s Hardest Game](about:blank#ivys-hardest-game)
*   [Summing Up](about:blank#summing-up)

Welcome!
--------

*   Artificial intelligence (AI) is providing new advancements and excitement in computer science and the wide world!
*   While AI provides huge advancements, sometimes eliminating the human bottlenecks that can slow down processes, being able to understand, create, and organize code allows you to be a driver, a pilot, an empowered creator through programming.
*   Therefore, rather than thinking about AI as a way to remove the need to learn the fundamentals, consider how you knowing the fundamentals and being further empowered by AI will lead to whole new opportunities for you and those you serve.

Visual Studio Code and chat.py
------------------------------

*   VS Code is an _IDE_ or integrated development environment, where one can create code.
*   To give you taste of what is to come, we could program our own chatbot called `chat.py`.
*   On a system already configured for using OpenAI’s libraries, we could program as follows.
*   In the text editor, one could type in the following code:
    
    ```
    from openai import OpenAI
    
    client = OpenAI()
    
    response = client.responses.create(
        input="In one sentence, what is CS50?",
        model="gpt-5"
    )
    
    print(response.output_text)
    
    ```
    
    Notice how a library from OpenAI is imported to give us abilities from that library. A chat client is created. Then, a question, called `input` is passed to the chat `client` for an answer. The `response` is then printed.
    
*   We could improve upon this code by allowing the user to ask a question. Modify your code as follow:
    
    ```
    from openai import OpenAI
    
    client = OpenAI()
    
    prompt = input("Prompt: ")
    
    response = client.responses.create(
        input=prompt,
        model="gpt-5"
    )
    
    print(response.output_text)
    
    ```
    
    Notice that `prompt` is now created, allowing the user to ask a question.
    
*   Even more, this program can be improved by providing a `system_prompt` to provide some further context and instructions to the chatbot:
    
    ```
    from openai import OpenAI
    
    client = OpenAI()
    
    user_prompt = input("Prompt: ")
    system_prompt = "Limit your answer to one sentence. Pretend you're a cat."
    
    response = client.responses.create(
        input=user_prompt,
        instructions=system_prompt,
        model="gpt-5"
    )
    
    print(response.output_text)
    
    ```
    
    Notice how `system_prompt` is used to provide further context and instructions.
    
*   With programming, you have the ability in ten lines of text to build very powerful programs!
*   We have created our own _rubber duck_, the [CS50 Duck](https://cs50.ai/), to get help in your work in this course.
*   Keep in mind our [Academic Honesty Policy](https://cs50.harvard.edu/syllabus/#academic-honesty), which prohibits the use of any AI tool besides the [CS50 Duck](https://cs50.ai/).

Computer Science and Problem Solving
------------------------------------

*   Essentially, computer programming is about taking some input and creating some output - thus solving a problem. What happens in between the input and output, what we could call _a black box,_ is the focus of this course.
    
    ```
    flowchart LR
        in["input"] --> BOX[" ??? "]
        BOX --> out["output"]
    
    ```
    
*   For example, we may need to take attendance for a class. We could use a system called _unary_ (also called _base-1_) to count one finger at a time.
*   Computers today count using a system called _binary_ (also called _base-2_). It’s from the term _binary digit_ that we get a familiar term called _bit_. A _bit_ is a zero or one: on or off.
*   Computers only speak in terms of zeros and ones. Zeros represent _off._ Ones represent _on._ Computers are millions, and perhaps billions, of transistors that are being turned on and off.
*   If you imagine using a light bulb, a single bulb can only count from zero to one.
*   However, if you were to have three light bulbs, there are more options open to you!
*   Inside your devices, such as your iPhone or computer, there are millions of metaphorical light bulbs called _transistors_ that enable the activities conducted on these devices one may take for granted each day.
*   As a heuristic, we could imagine that the following values represent each possible place in our _binary digit_:
    
    ```
    4 2 1
    
    ```
    
*   Using three light bulbs, the following could represent zero:
    
    ```
    4 2 1
    0 0 0
    
    ```
    
*   Similarly, the following would represent one:
    
    ```
    4 2 1
    0 0 1
    
    ```
    
*   By this logic, we could propose that the following equals two:
    
    ```
    4 2 1
    0 1 0
    
    ```
    
*   Extending this logic further, the following represents three:
    
    ```
    4 2 1
    0 1 1
    
    ```
    
*   Four would appear as:
    
    ```
    4 2 1
    1 0 0
    
    ```
    
*   We could, in fact, using only three light bulbs count as high as seven!
    
    ```
    4 2 1
    1 1 1
    
    ```
    
*   Computers use base-2 to count. This can be pictured as follows:
    
    ```
    2^2  2^1  2^0
    4    2    1
    
    ```
    
*   Therefore, you could say that it would require three bits (the four’s place, the two’s place, and the one’s place) to represent a number as high as seven.
*   Similarly, to count a number as high as eight, values would be represented as follows:
    
    ```
    8 4 2 1
    1 0 0 0
    
    ```
    
*   Computers generally use eight bits (also known as a _byte_) to represent a number. For example, `00000101` is the number 5 in _binary_. `11111111` represents the number 255. You can imagine zero as follows:
    
    | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
    | --- | --- | --- | --- | --- | --- | --- | --- |
    | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
    

ASCII
-----

*   Just as numbers are binary patterns of ones and zeros, letters are represented using ones and zeros, too!
*   Since there is an overlap between the ones and zeros that represent numbers and letters, the _ASCII_ standard was created to map specific letters to specific numbers.
*   For example, the letter `A` was decided to map to the number 65. `01000001` represents the number 65 in binary. You can visualize this as follows:
    
    | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
    | --- | --- | --- | --- | --- | --- | --- | --- |
    | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 1 |
    
*   If you received a text message, the binary under that message might represent the numbers 72, 73, and 33. Mapping these out to ASCII, your message would look as follows:
    
    ```
    H   I   !
    72  73  33
    
    ```
    
*   Thank goodness for standards like ASCII that allow us to agree upon these values!
*   Here is an expanded map of ASCII values:
    
    <table><tbody><tr><td><strong>0</strong></td><td>NUL</td><td><strong>16</strong></td><td>DLE</td><td><strong>32</strong></td><td>SP</td><td><strong>48</strong></td><td>0</td><td><strong>64</strong></td><td>@</td><td><strong>80</strong></td><td>P</td><td><strong>96</strong></td><td>`</td><td><strong>112</strong></td><td>p</td></tr><tr><td><strong>1</strong></td><td>SOH</td><td><strong>17</strong></td><td>DC1</td><td><strong>33</strong></td><td>!</td><td><strong>49</strong></td><td>1</td><td><strong>65</strong></td><td>A</td><td><strong>81</strong></td><td>Q</td><td><strong>97</strong></td><td>a</td><td><strong>113</strong></td><td>q</td></tr><tr><td><strong>2</strong></td><td>STX</td><td><strong>18</strong></td><td>DC2</td><td><strong>34</strong></td><td>”</td><td><strong>50</strong></td><td>2</td><td><strong>66</strong></td><td>B</td><td><strong>82</strong></td><td>R</td><td><strong>98</strong></td><td>b</td><td><strong>114</strong></td><td>r</td></tr><tr><td><strong>3</strong></td><td>ETX</td><td><strong>19</strong></td><td>DC3</td><td><strong>35</strong></td><td>#</td><td><strong>51</strong></td><td>3</td><td><strong>67</strong></td><td>C</td><td><strong>83</strong></td><td>S</td><td><strong>99</strong></td><td>c</td><td><strong>115</strong></td><td>s</td></tr><tr><td><strong>4</strong></td><td>EOT</td><td><strong>20</strong></td><td>DC4</td><td><strong>36</strong></td><td>$</td><td><strong>52</strong></td><td>4</td><td><strong>68</strong></td><td>D</td><td><strong>84</strong></td><td>T</td><td><strong>100</strong></td><td>d</td><td><strong>116</strong></td><td>t</td></tr><tr><td><strong>5</strong></td><td>ENQ</td><td><strong>21</strong></td><td>NAK</td><td><strong>37</strong></td><td>%</td><td><strong>53</strong></td><td>5</td><td><strong>69</strong></td><td>E</td><td><strong>85</strong></td><td>U</td><td><strong>101</strong></td><td>e</td><td><strong>117</strong></td><td>u</td></tr><tr><td><strong>6</strong></td><td>ACK</td><td><strong>22</strong></td><td>SYN</td><td><strong>38</strong></td><td>&amp;</td><td><strong>54</strong></td><td>6</td><td><strong>70</strong></td><td>F</td><td><strong>86</strong></td><td>V</td><td><strong>102</strong></td><td>f</td><td><strong>118</strong></td><td>v</td></tr><tr><td><strong>7</strong></td><td>BEL</td><td><strong>23</strong></td><td>ETB</td><td><strong>39</strong></td><td>’</td><td><strong>55</strong></td><td>7</td><td><strong>71</strong></td><td>G</td><td><strong>87</strong></td><td>W</td><td><strong>103</strong></td><td>g</td><td><strong>119</strong></td><td>w</td></tr><tr><td><strong>8</strong></td><td>BS</td><td><strong>24</strong></td><td>CAN</td><td><strong>40</strong></td><td>(</td><td><strong>56</strong></td><td>8</td><td><strong>72</strong></td><td>H</td><td><strong>88</strong></td><td>X</td><td><strong>104</strong></td><td>h</td><td><strong>120</strong></td><td>x</td></tr><tr><td><strong>9</strong></td><td>HT</td><td><strong>25</strong></td><td>EM</td><td><strong>41</strong></td><td>)</td><td><strong>57</strong></td><td>9</td><td><strong>73</strong></td><td>I</td><td><strong>89</strong></td><td>Y</td><td><strong>105</strong></td><td>i</td><td><strong>121</strong></td><td>y</td></tr><tr><td><strong>10</strong></td><td>LF</td><td><strong>26</strong></td><td>SUB</td><td><strong>42</strong></td><td>*</td><td><strong>58</strong></td><td>:</td><td><strong>74</strong></td><td>J</td><td><strong>90</strong></td><td>Z</td><td><strong>106</strong></td><td>j</td><td><strong>122</strong></td><td>z</td></tr><tr><td><strong>11</strong></td><td>VT</td><td><strong>27</strong></td><td>ESC</td><td><strong>43</strong></td><td>+</td><td><strong>59</strong></td><td>;</td><td><strong>75</strong></td><td>K</td><td><strong>91</strong></td><td>[</td><td><strong>107</strong></td><td>k</td><td><strong>123</strong></td><td>{</td></tr><tr><td><strong>12</strong></td><td>FF</td><td><strong>28</strong></td><td>FS</td><td><strong>44</strong></td><td>,</td><td><strong>60</strong></td><td>&lt;</td><td><strong>76</strong></td><td>L</td><td><strong>92</strong></td><td>\</td><td><strong>108</strong></td><td>l</td><td><strong>124</strong></td><td>|</td></tr><tr><td><strong>13</strong></td><td>CR</td><td><strong>29</strong></td><td>GS</td><td><strong>45</strong></td><td>-</td><td><strong>61</strong></td><td>=</td><td><strong>77</strong></td><td>M</td><td><strong>93</strong></td><td>]</td><td><strong>109</strong></td><td>m</td><td><strong>125</strong></td><td>}</td></tr><tr><td><strong>14</strong></td><td>SO</td><td><strong>30</strong></td><td>RS</td><td><strong>46</strong></td><td>.</td><td><strong>62</strong></td><td>&gt;</td><td><strong>78</strong></td><td>N</td><td><strong>94</strong></td><td>^</td><td><strong>110</strong></td><td>n</td><td><strong>126</strong></td><td>~</td></tr><tr><td><strong>15</strong></td><td>SI</td><td><strong>31</strong></td><td>US</td><td><strong>47</strong></td><td>/</td><td><strong>63</strong></td><td>?</td><td><strong>79</strong></td><td>O</td><td><strong>95</strong></td><td>_</td><td><strong>111</strong></td><td>o</td><td><strong>127</strong></td><td>DEL</td></tr></tbody></table>
    
*   If you wish, you can learn more about [ASCII](https://en.wikipedia.org/wiki/ASCII).
*   If each character is stored in exactly one 8-bit byte, you can encode at most 256 distinct character codes. ASCII uses only 128 of those (0-127).

Unicode
-------

*   As time has rolled on, there are more and more ways to communicate via text.
*   Since there were not enough digits in binary to represent all the various characters that could be represented by humans, the _Unicode_ standard expanded the number of bits that can be transmitted and understood by computers. Unicode includes not only special characters, but emoji as well.
*   There are emoji that you probably use every day. The following may look familiar to you:
    
    😀 😃 😄 😁 😆 😅 😂 🙂 🙃 😉 😊 😇 😍 😘 😗 😙 😚 😋 😛 😜 😝 🤑 🤓 😎 🤗 😏 😶 😐 😑 😒 🙄 😬 😕 ☹️ 😟 😮 😯 😲 😳 😦 😧 😨
    
*   While the pattern of zeros and ones is standardized within Unicode, each device manufacturer may display each emoji slightly differently than another manufacturer.
*   More and more features are being added to the Unicode standard to represent further characters and emoji.
*   If you wish, you can learn more about [Unicode](https://en.wikipedia.org/wiki/Unicode).
*   If you wish, you can learn more about [emoji](https://en.wikipedia.org/wiki/Emoji).

RGB
---

*   Zeros and ones can be used to represent color.
*   Red, green, and blue (called `RGB`) are a combination of three numbers.
    
    72 73 33
    
*   Taking our previously used 72, 73, and 33, which said `HI!` via text, would be interpreted by image readers as a light shade of yellow. The red value would be 72, the green value would be 73, and the blue would be 33.
    
*   The three bytes required to represent various colors of red, blue, and green (or _RGB_) make up each _pixel_ (or dot) of color in any digital image. Images are simply collections of RGB values.
*   Zeros and ones can be used to represent images, videos, and music!
*   Videos are sequences of many images that are stored together, just like a flipbook.
*   Music can be represented similarly using various combinations of bytes.

Algorithms
----------

*   Problem-solving is central to computer science and computer programming. An _algorithm_ is a step-by-step set of instructions to solve a problem.
*   Imagine the basic problem of trying to locate a single name in a phone book.
*   How might one go about this?
*   One approach could be to simply read from page one to the next to the next until reaching the last page.
*   Another approach could be to search two pages at a time.
*   A final and perhaps better approach could be to go to the middle of the phone book and ask, “Is the name I am looking for to the left or to the right?” Then, repeat this process, cutting the problem in half and half and half.
*   Each of these approaches could be called algorithms. The speed of each of these algorithms can be pictured as follows in what is called _big-O notation_:
    
    n n/2 log₂ n size of problem time to solve
    
    Notice that the first algorithm, highlighted in red, has a big-O of `n` because if there are 100 names in the phone book, it could take up to 100 tries to find the correct name. The second algorithm, where two pages were searched at a time, has a big-O of `n/2` because we searched twice as fast through the pages. The final algorithm has a big-O of log2n, as doubling the problem would only result in one more step to solve the problem.
    
*   Programmers translate text-based, human instructions into _code_ to solve problems.

Pseudocode
----------

*   Pseudocode is human-readable instructions that often describe the steps of an algorithm.
*   The ability to create _pseudocode_ is central to one’s success in both this class and in computer programming.
*   For example, considering the third algorithm above, we could compose pseudocode as follows:
    
    ```
    1  Pick up phone book
    2  Open to middle of phone book
    3  Look at page
    4  If person is on page
    5      Call person
    6  Else if person is earlier in book
    7      Open to middle of left half of book
    8      Go back to line 3
    9  Else if person is later in book
    10     Open to middle of right half of book
    11     Go back to line 3
    12 Else
    13     Quit
    
    ```
    
*   Pseudocoding is such an important skill for at least two reasons. First, when you pseudocode before you create formal code, it allows you to think through the logic of your problem in advance. Second, when you pseudocode, you can later provide this information to others that are seeking to understand your coding decisions and how your code works.
*   Notice that the language within our pseudocode has some unique features. First, some of these lines begin with verbs like _pick up,_ _open,_ _look at._ Later, we will call these _functions_.
*   Second, notice that some lines include statements like `if` or `else if.` These are called _conditionals_.
*   Third, notice how there are expressions that can be stated as _true_ or _false,_ such as “person is earlier in the book.” We call these _boolean expressions_.
*   Finally, notice how there are statements like “go back to line 3.” We call these _loops_.
*   These building blocks are the fundamentals of programming.
*   In the context of _Scratch_, which is discussed below, we will use each of the above basic building blocks of programming.

What’s Ahead
------------

*   You will be learning this week about Scratch, a visual programming language.
*   Then, in future weeks, you will learn about C. That will look something like this:
    
    ```
    #include <stdio.h>
    int main(void)
    {
      printf("hello, world\n");
    }
    
    ```
    
*   By learning C, you will be far more prepared for future learning in other programming languages like _Python_.
*   Notice how programmers have used _abstraction_ to build off the work of other programmers. Rather than programming in ones and zeroes, programming languages were created to _abstract away_ from the incredibly challenging task of programming in binary to more and more easy-to-use programming languages. We can stand on the shoulders of others!

Scratch
-------

*   _Scratch_ is a visual programming language developed by MIT.
*   [Scratch](https://scratch.mit.edu/) utilizes the same essential coding building blocks that we covered earlier in this lecture.
*   Scratch is a great way to get into computer programming because it allows you to play with these building blocks in a visual manner, not having to be concerned about the syntax of curly braces, semicolons, parentheses, and the like.
*   Scratch `IDE` (integrated development environment) looks like the following:
    
    ![scratch interface](https://cs50.harvard.edu/cs50Week0Slide162.png "scratch interface")
    
    Notice that on the left, there is a palette of _building blocks_ that you can use in your programming. To the immediate right of the building blocks, there is the area to which you can drag blocks to build a program. To the right of that, you see the _stage_ where a cat stands. The stage is where your programming comes to life.
    
*   Scratch operates on a coordinate system as follows:
    
    ![scratch coordinate system](https://cs50.harvard.edu/cs50Week0Slide167.png "scratch coordinate system")
    
    Notice that the center of the stage is at coordinate (0,0). Right now, the cat’s position is at that same position.
    

Hello World
-----------

*   To begin, drag the “when green flag clicked” building block to the programming area. Then, drag the `say` building block to the programming area and attach it to the previous block.
    
    ```
    when green flag clicked
    say [hello, world]
    
    ```
    
    Notice that when you click the green flag now on the stage, the cat says, “hello, world.”
    
*   This illustrates quite well what we were discussing earlier regarding programming:
    
    ![scratch with black box](https://cs50.harvard.edu/cs50Week0Slide172.png "scratch with black box")
    
    Notice that the input `hello, world` is passed to the function `say`, and the _side effect_ of that function running is the cat saying `hello, world`.
    

Hello, You
----------

*   We can make your program more interactive by having the cat say `hello` to someone specific. Modify your program as below:
    
    ```
    when green flag clicked
    ask [What's your name?] and wait
    say (join [hello,] (answer))
    
    ```
    
    Notice that when the green flag is clicked, the function `ask` is run. The program prompts you, the user, `What's your name?` It then stores that name in the _variable_ called `answer`. The program then passes `answer` to a special function called `join`, which combines two strings of text `hello`, and whatever name was provided. The value of `answer` is passed as an argument to `join`. These collectively are passed to the `say` function. The cat says, `Hello,` and a name. Your program is now interactive.
    
*   Throughout this course, you will be providing inputs into an algorithm and getting outputs. This can be pictured in terms of the above program as follows:
    
    ![scratch as algorithm](https://cs50.harvard.edu/cs50Week0Slide169.png "hello and answer provided to join to get hello david")
    
    Notice that the inputs `hello,` and `answer` are provided to `join`, which returns `hello, David`. This return value is then passed to `say`, which produces the _side effect_ of the cat speaking.
    
*   Quite similarly, we can modify our program as follows:
    
    ```
    when green flag clicked
    ask [What's your name?] and wait
    speak (join [hello,] (answer))
    
    ```
    
    Notice that this program, when the green flag is clicked, passes the same variable, joined with `hello`, to a function called `speak`.
    

Meow, Loops, and Abstraction
----------------------------

*   Along with pseudocoding, _abstraction_ is an essential skill and concept within computer programming.
*   Abstraction is the act of simplifying a problem into smaller and smaller problems.
*   For example, if you were hosting a huge dinner for your friends, the _problem_ of having to cook the entire meal could be quite overwhelming! However, if you break down the task of cooking the meal into smaller and smaller tasks (or problems), the big task of creating this delicious meal might feel less challenging.
*   In programming, and even within Scratch, we can see abstraction in action. In your programming area, program as follows:
    
    ```
    when green flag clicked
    play sound (Meow v) until done
    wait (1) seconds
    play sound (Meow v) until done
    wait (1) seconds
    play sound (Meow v) until done
    
    ```
    
    Notice that you are doing the same thing over and over again. Indeed, if you see yourself repeatedly coding the same statements, it’s likely the case that you could program more artfully – abstracting away this repetitive code.
    
*   You can modify your code as follows:
    
    ```
    when green flag clicked
    repeat (3)
    play sound (Meow v) until done
    wait (1) seconds
    
    ```
    
    Notice that the loop does exactly as the previous program did. However, the problem is simplified by abstracting away the repetition to a block that _repeats_ the code for us.
    
*   We can even advance this further by using the `define` block, where you can create your own block (your own function)! Write code as follows:
    
    ```
    define meow
    play sound (Meow v) until done
    wait (1) seconds
    
    when green flag clicked
    repeat (3)
    meow
    
    ```
    
    Notice that we are defining our own block called `meow`. The function plays the sound `meow`, and then waits one second. Below that, you can see that when the green flag is clicked, our meow function is repeated three times.
    
*   We can even provide a way by which the function can take an input `n` and repeat a number of times:
    
    ```
    define meow n times
    repeat (n)
     play sound [meow v] until done
     wait (1) seconds
    
    ```
    
    Notice how `n` is taken from “meow n times.” `n` is passed to the meow function through the `define` block.
    
*   Overall, notice how this process of refinement led to better and better-designed code. Further, notice how we created our own algorithm to solve a problem. You will be exercising both of these skills throughout this course.

Conditionals
------------

*   _Conditionals_ are an essential building block of programming, where the program looks to see if a specific condition has been met. If a condition is met, the program does something.
*   To illustrate a conditional, write code as follows:
    
    ```
    when green flag clicked
    forever
    if <touching (mouse-pointer v)?> then
    play sound (Meow v) until done
    
    ```
    
    Notice that the `forever` block is utilized such that the `if` block is triggered over and over again, such that it can check continuously if the cat is touching the mouse pointer.
    
*   We can modify our program as follows to integrate video sensing:
    
    ```
    when video motion > (10)
    play sound (Meow v) until done
    
    ```
    
*   Remember, programming is often a process of trial and error. If you get frustrated, take time to talk yourself through the problem at hand. What is the specific problem that you are working on right now? What is working? What is not working?

Oscartime
---------

*   _Oscartime_ is one of David’s own Scratch programs – though the music may haunt him because of the number of hours he listened to it while creating this program. Take a few moments to play through the [Oscartime](https://scratch.mit.edu/projects/277537196) game yourself.
    
*   Building Oscartime ourselves, we first add the lamp post.
    
    ![oscartime interface](https://cs50.harvard.edu/cs50Week0Scratch10.png "oscartime interface")
    
*   Then, write code as follows:
    
    ```
    when green flag clicked
    switch costume to (oscar1 v)
    forever
    if <touching (mouse-pointer v)?> then
    switch costume to (oscar2 v)
    else
    switch costume to (oscar1 v)
    
    ```
    
    Notice that moving your mouse over Oscar changes his costume. You can learn more by [exploring these code blocks](https://scratch.mit.edu/studios/50827060/).
    
*   Then, modify your code as follows to create a falling piece of trash:
    
    ```
    when green flag clicked
    set drag mode [draggable v]
    go to x: (pick random (0) to (240)) y: (180)
    forever
      change y by (-1)
    end
    
    ```
    
    Notice that the trash’s position on the y-axis always begins at 180. The x position is randomized. While the trash is above the floor, it goes down 1 pixel at a time. You can learn more by [exploring these code blocks](https://scratch.mit.edu/projects/726782167).
    
*   Next, modify your code as follows to allow for the possibility of dragging trash.
    
    ```
    when green flag clicked
    forever
      if <touching [Oscar v] ?> then
        go to x: (pick random (0) to (240)) y: (180)
      end
    end
    
    ```
    
    You can learn more by [exploring these code blocks](https://scratch.mit.edu/projects/726780064).
    
*   Next, we can implement the scoring variables as follows:
    
    ```
    when green flag clicked
    set drag mode [draggable v]
    go to top
    forever
      change y by (-1)
    end
    
    ```
    ```
    when green flag clicked
    forever
      if <touching [Oscar v] ?> then
        go to top
      end
    end
    
    ```
    ```
    define go to top
      go to x: (pick random (0) to (240)) y: (180)
    
    ```
    
    You can learn more by [exploring these code blocks](https://scratch.mit.edu/projects/726779570).
    
*   Go try the full game [Oscartime](https://scratch.mit.edu/projects/277537196).
    

Ivy’s Hardest Game
------------------

*   Moving away from Oscartime to Ivy’s Hardest Game, we can now imagine how to implement movement within our program.
*   Our program has three main components.
*   First, write code as follows:
    
    ```
    when green flag clicked
    go to x: (0) y: (0)
    forever
    listen for keyboard
    feel for walls
    
    ```
    
    Notice that when the green flag is clicked, our sprite moves to the center of the stage at coordinates (0,0) and then listens for the keyboard and checks for walls forever.
    
*   Second, add this second group of code blocks:
    
    ```
    define listen for keyboard
    if <key (up arrow v) pressed?> then
    change y by (1)
    end
    if <key (down arrow v) pressed?> then
    change y by (-1)
    end
    if <key (right arrow v) pressed?> then
    change x by (1)
    end
    if <key (left arrow v) pressed?> then
    change x by (-1)
    end
    
    ```
    
    Notice how we have created a custom `listen for keyboard` script. For each of our arrow keys on the keyboard, it will move the sprite around the screen.
    
*   Finally, add this group of code blocks:
    
    ```
    define feel for walls
    if <touching (left wall v) ?> then
    change x by (1)
    end
    if <touching (right wall v) ?> then
    change x by (-1)
    end
    
    ```
    
    Notice how we also have a custom `feel for walls` script. When a sprite touches a wall, it moves it back to a safe position – preventing it from walking off the screen.
    
*   You can learn more by [exploring these code blocks](https://scratch.mit.edu/projects/577647301).
*   Scratch allows for many sprites to be on the screen at once.
*   Adding another sprite, add the following code blocks to your program:
    
    ```
    when green flag clicked
    go to x: (0) y: (0)
    point in direction (90)
    forever
    if <<touching (left wall v)?> or <touching (right wall v)?>> then
    turn right (180) degrees
    end
    move (1) steps
    end
    
    ```
    
    Notice how the Yale sprite seems to get in the way of the Harvard sprite by moving back and forth. When it bumps into a wall, it turns around until it bumps the wall again. You can learn more by [exploring these code blocks](https://scratch.mit.edu/projects/577647503).
    
*   You can even make a sprite follow another sprite. Adding another sprite, add the following code blocks to your program:
    
    ```
    when green flag clicked
    go to (random position v)
    forever
    point towards (Harvard v)
    move (1) steps
    
    ```
    
    Notice how the MIT logo now seems to follow around the Harvard one. You can learn more by [exploring these code blocks](https://scratch.mit.edu/projects/577647729).
    
*   Go try the full game [Ivy’s Hardest Game](https://scratch.mit.edu/projects/565742837).

Summing Up
----------

In this lesson, you learned how this course sits in the wide world of computer science and programming. You learned…

*   While AI can help remove human bottlenecks, learning the fundamentals in computer science and the foundational building blocks of programming will enable you to utilize emerging technologies better and better.
*   Reasonable and unreasonable ways to utilize AI in this course.
*   Problem-solving is the essence of the work of computer scientists.
*   How numbers, text, images, music, and video are understood and represented by computers.
*   The fundamental programming skill of pseudocoding.
*   How abstraction will play a role in your future work in this course.
*   The basic building blocks of programming including functions, conditionals, loops, and variables.
*   How to build a project in Scratch.

This was CS50! Welcome aboard! See you next time!

  

> 本文转自 [https://cs50.harvard.edu/x/notes/0/](https://cs50.harvard.edu/x/notes/0/)，如有侵权，请联系删除。