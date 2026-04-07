# 第3讲

* [欢迎！](#welcome)
* [线性搜索](#linear-search)
* [二分搜索](#binary-search)
* [运行时间](#running-time)
* [search.c](#searchc)
* [phonebook.c](#phonebookc)
* [结构体](#structs)
* [排序与选择排序](#sorting-and-selection-sort)
* [冒泡排序](#bubble-sort)
* [递归](#recursion)
* [归并排序](#merge-sort)
* [总结](#summing-up)

---

## 欢迎！
* 第0周我们介绍了**算法（algorithm）**的概念：它就像一个黑盒，接收输入后会生成对应的输出。
* 本周我们会基于**伪代码（pseudocode）**深化对算法的理解，最终落地为可运行的代码。
* 同时我们会学习如何评估算法的效率，基于上周介绍的相关概念，搭建算法设计的知识体系。
* 我们可以参考下图的趋势：
  
  ![image-20260330194344048](./week_3_note_Chinese.assets/image-20260330194344048.png)
* 算法的效率有高有低：低效算法的时间和计算资源成本很高，高效算法则成本很低。
* 本周的学习中，你会理解算法的设计思路如何直接决定问题的解决速度；算法可以被不断优化，但效率存在理论极限。
* 今天我们的核心内容是算法设计，以及如何衡量算法的效率。

---

## 线性搜索
* 回顾上周的内容，我们学习了**数组（array）**的概念：它是一段连续的内存块，所有元素在内存中相邻排列。
* 你可以把数组想象成7个并排的红色储物柜，如下图所示：
  <table role="img" aria-label="7个红色储物柜组成的数组，下标从0到6" style="width:min(100%,420px);margin:0;margin-right:auto;border-collapse:collapse;"><tbody><tr><td style="background:#e10600;height:44px;"></td><td style="background:#e10600;height:44px;"></td><td style="background:#e10600;height:44px;"></td><td style="background:#e10600;height:44px;"></td><td style="background:#e10600;height:44px;"></td><td style="background:#e10600;height:44px;"></td><td style="background:#e10600;height:44px;"></td></tr><tr><td>[0]</td><td>[1]</td><td>[2]</td><td>[3]</td><td>[4]</td><td>[5]</td><td>[6]</td></tr></tbody></table>
* 最左侧的位置是「下标0」，也就是数组的起始位置；最右侧的是「下标6」，也就是数组的末尾位置。
* 我们面临一个非常基础的问题：「数字`50`是否在这个数组里？」计算机必须逐个查看每个储物柜的内容，才能判断`50`是否存在。我们把查找数字、字符、字符串或其他元素的过程统称为**搜索（searching）**。
* 我们可以把数组传入一个算法，让算法遍历所有储物柜，判断`50`是否在其中，最终返回`true`（存在）或`false`（不存在）。
* 我们可以给算法设计如下的执行步骤：
  ```tex
  从左到右遍历每一扇柜门
      如果柜门后是50
          返回true
  遍历结束后返回false
  ```
  上述步骤属于伪代码：它是人类可读的步骤描述，不需要严格遵循编程语言语法，用于快速梳理算法逻辑。
* 计算机科学家可以把上述伪代码进一步抽象为接近代码的形式：
  ```
  i从0遍历到n-1
      如果doors[i]后是50
          返回true
  返回false
  ```
  这段内容仍然不是可运行的代码，但已经非常接近最终的C语言代码结构。

---

## 二分搜索
* **二分搜索（Binary Search）**是另一种可以用来查找`50`的**搜索算法（search algorithm）**。
* 前提是储物柜中的元素已经按照从小到大的顺序排列，二分搜索的伪代码如下：
  ```tex
  如果没有剩余的柜门
      返回false
  如果中间柜门后是50
      返回true
  否则如果50 < 中间柜门的数字
      搜索左半部分
  否则如果50 > 中间柜门的数字
      搜索右半部分
  ```
  
* 用更接近代码的命名方式，可以把算法改写为：
```tex
  如果没有剩余的柜门
      返回false
  如果doors[middle]后是50
      返回true
  否则如果50 < doors[middle]
      搜索doors[0]到doors[middle - 1]区间
  否则如果50 > doors[middle]
      搜索doors[middle + 1]到doors[n - 1]区间
```
 看到这段接近代码的描述，你应该已经可以想象出实际可运行的代码是什么样的了。

---

## 运行时间
* 你可以思考一个算法解决问题需要花费多少时间。

* **运行时间（Running Time）**通常通过**大O表示法（Big O notation）**来分析，参考下图的趋势：
  
  ![image-20260330194344048](./week_3_note_Chinese.assets/image-20260330194344048.png)
  
* 计算机科学家不会纠结于算法效率的精确数学值，而是用运行时间的「量级」来描述效率。

* 上图中，第一种算法的复杂度为 $\mathcal{O}(n)$，也就是「n量级」；第二种的复杂度同样是$\mathcal{O}(n)$，因为大O表示法会忽略常数系数；第三种的复杂度为$\mathcal{O}(\log n)$。

* 算法的效率由时间增长的曲线趋势决定，常见的运行时间量级包括：

	- [ ] $\mathcal{O}(n^2)$
	
	- [ ] $\mathcal{O}(n \log n)$
	
	- [ ] $\mathcal{O}(n)$
	
	- [ ] $\mathcal{O}(\log n)$
	
	- [ ] $\mathcal{O}(1)$


​	  
​	
* 上述量级中，$\mathcal{O}(n^2)$的效率最低，$\mathcal{O}(1)$的效率最高。

* 线性搜索的复杂度为$\mathcal{O}(n)$，因为最坏情况下需要遍历全部n个元素才能得到结果。

* 二分搜索的复杂度为$\mathcal{O}(n \log n)$，即使在最坏情况下，需要的步骤也会随着搜索范围的缩小快速减少。

* 程序员通常会同时关注算法的最坏情况（即**上界 upper bound**）和最好情况（即**下界 lower bound**）。

* **大Ω（Omega）符号**用于表示算法的最好情况复杂度，例如$\mathcal{Ω}(n \log n)$。

* **大Θ（Theta）符号**用于表示算法的上界和下界相同的情况，也就是最好情况和最坏情况的运行时间量级一致。

* **渐近记号（Asymptotic notation）**是用于衡量输入规模不断增大时算法性能表现的统一标准。

* 随着你计算机科学知识的积累，后续的课程会更深入地讲解这些主题。

---

## search.c
* 你可以在终端中输入`code search.c`创建文件，编写如下代码实现线性搜索：
  ```c
  // Implements linear search for integers
  #include <cs50.h>
  #include <stdio.h>
  int main(void)
  {
      // An array of integers
      int numbers[] = {20, 500, 10, 5, 100, 1, 50};
  
      // Search for number
      int n = get_int("Number: ");
      for (int i = 0; i < 7; i++)
      {
          if (numbers[i] == n)
          {
              printf("Found\n");
              return 0;
          }
      }
      printf("Not found\n");
      return 1;
  }
  ```
  以`int numbers[]`开头的代码行在创建数组的同时直接为每个元素赋值；后续的for循环就是线性搜索的实现；`return 0`表示程序运行成功并退出；`return 1`表示程序运行失败（未找到目标）并退出。
* 我们已经用C语言自己实现了线性搜索！
* 如果我们要在数组中搜索字符串，需要修改代码如下：
  ```c
  // Implements linear search for strings
  #include <cs50.h>
  #include <stdio.h>
  #include <string.h>
  int main(void)
  {
      // An array of strings
      string strings[] = {"battleship", "boot", "cannon", "iron", "thimble", "top hat"};
  
      // Search for string
      string s = get_string("String: ");
      for (int i = 0; i < 6; i++)
      {
          if (strcmp(strings[i], s) == 0)
          {
              printf("Found\n");
              return 0;
          }
      }
      printf("Not found\n");
      return 1;
  }
  ```
  注意我们不能像之前的整数搜索一样直接用`==`比较字符串，而是需要使用`string.h`库提供的`strcmp`函数，当两个字符串相等时`strcmp`会返回`0`。另外注意代码中的数组长度`6`是直接写死的，这属于**硬编码（hard-coded）**，是不推荐的编程实践。
* 运行这段代码就可以遍历字符串数组，判断目标字符串是否存在。如果运行时出现**段错误（segmentation fault）**，也就是程序访问了没有权限的内存区域，请确认for循环的条件是`i < 6`而非`i < 7`。
* 你可以在[CS50官方手册](https://manual.cs50.io/3/strcmp)中查看`strcmp`的详细用法。

---

## phonebook.c
* 我们可以把数字和字符串的搜索逻辑结合到一个程序中，在终端输入`code phonebook.c`编写如下代码：
  ```c
  // Implements a phone book without structs
  #include <cs50.h>
  #include <stdio.h>
  #include <string.h>
  int main(void)
  {
      // Arrays of strings
      string names[] = {"Kelly", "David", "John"};
      string numbers[] = {"+1-617-495-1000", "+1-617-495-1000", "+1-949-468-2750"};
  
      // Search for name
      string name = get_string("Name: ");
      for (int i = 0; i < 3; i++)
      {
          if (strcmp(names[i], name) == 0)
          {
              printf("Found %s\n", numbers[i]);
              return 0;
          }
      }
      printf("Not found\n");
      return 1;
  }
  ```
  Kelly的号码以`+1-617`开头，David的号码同样以`+1-617`开头，John的号码以`+1-949`开头。`names[0]`对应Kelly，`numbers[0]`对应Kelly的号码，这段代码可以实现通讯录的搜索功能，输入姓名即可查询对应的号码。
* 虽然这段代码可以运行，但存在很多效率问题，而且姓名和号码的对应关系很容易因为代码修改错乱。如果我们能自定义一种数据类型，把姓名和对应的电话号码绑定在一起，是不是会方便很多？

---

## 结构体
* 事实上C语言允许我们通过**结构体（Struct）**自定义数据类型。
* 我们可以自定义一种名为`person`的数据类型，内部包含`name`（姓名）和`number`（号码）两个属性，代码如下：
  ```c
  typedef struct
  {
      string name;
      string number;
  } person;
  ```
  这段代码定义了我们自己的`person`类型，包含两个字符串类型的属性：`name`和`number`。
* 我们可以用结构体优化之前的通讯录程序，修改后代码如下：
  ```c
  // Implements a phone book with structs
  #include <cs50.h>
  #include <stdio.h>
  #include <string.h>
  typedef struct
  {
      string name;
      string number;
  } person;
  
  int main(void)
  {
      person people[3];
  
      people[0].name = "Kelly";
      people[0].number = "+1-617-495-1000";
  
      people[1].name = "David";
      people[1].number = "+1-617-495-1000";
  
      people[2].name = "John";
      people[2].number = "+1-949-468-2750";
  
      // Search for name
      string name = get_string("Name: ");
      for (int i = 0; i < 3; i++)
      {
          if (strcmp(people[i].name, name) == 0)
          {
              printf("Found %s\n", people[i].number);
              return 0;
          }
      }
      printf("Not found\n");
      return 1;
  }
  ```
  代码开头的`typedef struct`定义了名为`person`的新数据类型，内部包含`name`和`number`两个字符串属性。`main`函数中首先创建了长度为3的`person`类型数组`people`，然后为数组中三个元素的姓名和号码属性赋值。最关键的是**点表示法（dot notation）**的使用，例如`people[0].name`可以访问数组下标0的`person`元素的`name`属性，为其赋值。

---

## 排序与选择排序
* **排序（Sorting）**是将无序的值序列调整为有序序列的过程。
* 当数组有序时，搜索的成本会大幅降低：我们可以对有序数组使用二分搜索，但无法对无序数组使用。
* 排序算法有很多种不同的实现，**选择排序（Selection Sort）**是其中一种基础实现。
* 我们可以用如下示意图表示数组：
  <table role="img" aria-label="7个红色储物柜组成的数组，最后一个下标为n-1" style="width:min(100%,420px);margin:0;margin-right:auto;border-collapse:collapse;table-layout:fixed;"><colgroup><col span="7" style="width:14.2857%"></colgroup><tbody><tr><td style="background:#e10600;height:44px;"></td><td style="background:#e10600;height:44px;"></td><td style="background:#e10600;height:44px;"></td><td style="background:#e10600;height:44px;"></td><td style="background:#e10600;height:44px;"></td><td style="background:#e10600;height:44px;"></td><td style="background:#e10600;height:44px;"></td></tr><tr><td style="text-align:center;white-space:nowrap;">[0]</td><td style="text-align:center;white-space:nowrap;">[1]</td><td style="text-align:center;white-space:nowrap;">[2]</td><td style="text-align:center;white-space:nowrap;">[...]</td><td style="text-align:center;white-space:nowrap;">[n-3]</td><td style="text-align:center;white-space:nowrap;">[n-2]</td><td style="text-align:center;white-space:nowrap;">[n-1]</td></tr></tbody></table>
* 选择排序的伪代码如下：
  ```
  i从0遍历到n–1
      找到numbers[i]到numbers[n-1]区间内的最小数字
      将最小数字与numbers[i]交换
  ```
* 我们可以汇总一下步骤：第一次遍历需要`n-1`步，第二次需要`n-2`步，以此类推，总步骤数为：
  ```c
  (n - 1) + (n - 2) + (n - 3) + ... + 1
  ```
* 这个求和式可以简化为`n(n-1)/2`，用大O表示法记为$\mathcal{O}(n^2)$。选择排序的最坏情况（上界）复杂度为$\mathcal{O}(n^2)$，最好情况（下界）复杂度同样为$\mathcal{O}(n^2)$。

---

## 冒泡排序

* **冒泡排序（Bubble Sort）**是另一种基础排序算法，通过反复交换相邻的逆序元素，将较大的元素逐步“冒泡”到序列的末尾。
* 冒泡排序的伪代码如下：
  ``````
  重复n-1次
      i从0遍历到n–2
          如果numbers[i]和numbers[i+1]顺序错误
              交换两者
      如果本轮没有发生交换
          提前退出
  
* 随着排序的推进，数组末尾的有序部分会越来越长，因此我们只需要比较尚未完成排序的相邻元素对即可。
* 冒泡排序的复杂度可以这样推导：
  * 总操作次数为 $((n - 1) * times (n - 1))$
  * 展开后为 $(n^2 - n - n + 1)$
  * 简化为 $(n^2 - 2n + 1)$
  * 用大O表示法忽略低次项和常数系数，最终复杂度为 $\mathcal{O}(n^2)$
* 冒泡排序的最坏情况（上界）复杂度为 $\mathcal{O}(n^2)$，最好情况（下界）复杂度为 $\mathcal{O}(n)$——当数组本身已经完全有序时，第一轮遍历没有发生任何交换，就可以直接提前退出，仅需要n次比较。
* 你可以访问这个[可视化网站](https://www.cs.usfca.edu/~galles/visualization/ComparisonSort.html)，直观对比各类排序算法的执行过程差异。

---

## 递归
* 我们该如何进一步提升排序算法的效率呢？
* **递归（Recursion）**是编程中的核心概念，指函数可以调用自身的特性。我们之前介绍的二分搜索就用到了递归思路：
  ```
  如果没有剩余的柜门
      返回false
  如果中间柜门后是目标数字
      返回true
  否则如果目标数字 < 中间柜门的数字
      搜索左半部分
  否则如果目标数字 > 中间柜门的数字
      搜索右半部分
  ```
  可以看到，算法会不断在更小的问题区间上调用「搜索」逻辑本身。
* 同样，第0周我们介绍的查电话簿的伪代码也用到了递归：
  ```
  1 拿起电话簿
  2 翻到电话簿中间页
  3 查看当前页
  4 如果目标人物在当前页
  5      拨打电话
  6 否则如果目标人物在电话簿前半部分
  7      翻到左半部分的中间页
  8      回到第3步
  9 否则如果目标人物在电话簿后半部分
  10     翻到右半部分的中间页
  11     回到第3步
  12 否则
  13     退出
  ```
* 这段伪代码可以简化为更突出递归特性的形式：
  ```
  1 拿起电话簿
  2 翻到电话簿中间页
  3 查看当前页
  4 如果目标人物在当前页
  5      拨打电话
  6 否则如果目标人物在电话簿前半部分
  7      搜索电话簿的左半部分
  9 否则如果目标人物在电话簿后半部分
  10     搜索电话簿的右半部分
  12 否则
  13     退出
  ```
* **基线条件（Base case）**是递归的终止条件，用于避免递归无限执行，防止出现无限循环。
* **递归条件（Recursive case）**是递归函数中调用自身的部分，每次调用会缩小问题规模，逐步向基线条件靠近。

* 回顾第1周我们实现的金字塔图案，输出如下：
  ```
    #
    ##
    ###
    ####
  ```
* 你可以在终端输入`code iteration.c`，用迭代（循环）的方式实现这个功能：
  ```c
  // Draws a pyramid using iteration
  #include <cs50.h>
  #include <stdio.h>
  void draw(int n);
  
  int main(void)
  {
      // Get height of pyramid
      int height = get_int("Height: ");
  
      // Draw pyramid
      draw(height);
  }
  
  void draw(int n)
  {
      // Draw pyramid of height n
      for (int i = 0; i < n; i++)
      {
          for (int j = 0; j < i + 1; j++)
          {
              printf("#");
          }
          printf("\n");
      }
  }
  ```
  这段代码通过嵌套循环实现了金字塔的绘制。

* 如果要用递归实现同样的功能，可以输入`code recursion.c`编写如下代码：
  ```c
  // Draws a pyramid using recursion
  #include <cs50.h>
  #include <stdio.h>
  void draw(int n);
  
  int main(void)
  {
      // Get height of pyramid
      int height = get_int("Height: ");
  
      // Draw pyramid
      draw(height);
  }
  
  void draw(int n)
  {
      // If nothing to draw
      if (n <= 0)
      {
          return;
      }
  
      // Draw pyramid of height n - 1
      draw(n - 1);
  
      // Draw one more row of width n
      for (int i = 0; i < n; i++)
      {
          printf("#");
      }
      printf("\n");
  }
  ```
  注意基线条件确保了代码不会无限运行：当`n <= 0`时递归终止，因为已经没有需要绘制的内容。每次`draw`函数调用自身时，传入的参数都是`n-1`，最终`n`会降到0，触发基线条件，函数依次返回，程序正常结束。

---

## 归并排序
* 我们可以利用递归实现一种效率更高的排序算法——**归并排序（Merge Sort）**，它是目前通用排序算法中效率最高的实现之一。
* 归并排序的伪代码非常简洁：
  ```
  如果当前区间只有1个数字
      退出
  否则
      排序左半部分数字
      排序右半部分数字
      合并两个有序的半区
  ```
* 我们用如下的数字序列举例：
  ```
    6 3 4 1
  ```
  （注：原文中`6341`为排版省略空格的写法，实际是4个独立数字6、3、4、1）
* 第一步，归并排序会判断：「当前区间只有1个数字吗？」答案是否，继续执行。
  ```
    6 3 4 1
  ```
* 第二步，将序列从中间拆分，先排序左半部分：
  ```
    6 3 | 4 1
  ```
* 第三步，查看左半部分，判断「当前区间只有1个数字吗？」答案是否，继续将左半部分拆分为两个小区间：
  ```
    6 | 3
  ```
* 第四步，再次判断「当前区间只有1个数字吗？」答案是，因此退出当前拆分，回到上一层任务：
  ```
    6 3 | 4 1
  ```
* 第五步，将左半部分的两个有序小区间合并，得到有序的左半区：
  ```
    3 6 | 4 1
  ```
* 左半区排序完成后，回到之前的逻辑，对右半区重复第三步到第五步的操作，最终得到有序的右半区：
  ```
    3 6 | 1 4
  ```
* 现在左右两个半区都已经有序，最后一步是合并两个半区：依次比较左右半区的首元素，每次取更小的元素放入结果序列，重复这个过程直到所有元素都被合并，最终得到完全有序的序列：
  ```
    1 3 4 6
  ```
* 归并排序完成，程序退出。
* 归并排序的效率非常高，最坏情况复杂度为 $\mathcal{O}(n \log n)$；最好情况复杂度同样为$\mathcal{Ω}(n \log n)$ ，因为算法无论如何都需要遍历所有元素至少一次。由于最好和最坏情况的复杂度一致，归并排序的复杂度也可以用 $\mathcal{Θ}(n \log n)$ 表示。
* 课程中还分享了一个归并排序的[可视化视频](https://www.youtube.com/watch?v=ZZuD6iUe3Pc)，可以直观感受算法的执行过程。

---

## 总结
本节课我们学习了算法思维，以及如何自定义数据类型，具体知识点包括：
* 算法的核心概念
* 大O表示法与复杂度分析
* 二分搜索与线性搜索
* 多种排序算法：冒泡排序、选择排序、归并排序
* 递归的原理与实现

我们下节课见！

---

【核心术语对照表】
| 英文原文 | 标准译法 | 概念说明 |
|----------|----------|----------|
| algorithm | 算法 | 用于解决特定问题的一系列明确步骤，接收输入后可生成对应输出，是计算机科学的核心基础概念。 |
| pseudocode | 伪代码 | 用于描述算法逻辑的人类可读文本，无需严格遵循编程语言语法，用于快速梳理思路、交流算法逻辑。 |
| array | 数组 | 内存中连续存储的同类型元素集合，通过下标（索引）访问元素，是最基础的数据结构之一。 |
| searching | 搜索 | 在数据集合中查找特定目标元素的过程，常见的搜索算法有线性搜索、二分搜索等。 |
| search algorithm | 搜索算法 | 用于实现搜索功能的算法，根据数据的特性（有序/无序）可选择不同效率的实现。 |
| linear search | 线性搜索 | 最基础的搜索算法，从序列起点到终点逐个比较元素，最坏情况复杂度为O(n)，适用于无序序列。 |
| binary search | 二分搜索 | 仅适用于有序序列的高效搜索算法，每次将搜索范围缩小一半，最坏情况复杂度为O(log n)，也译作折半搜索。 |
| running time | 运行时间 | 衡量算法效率的核心指标，通常用时间复杂度（大O表示法）描述输入规模增大时的效率变化趋势。 |
| Big O notation | 大O表示法 | 用于描述算法时间/空间复杂度的标准记号，忽略常数系数和低次项，仅保留最高阶的增长趋势，表示算法的最坏情况复杂度。 |
| upper bound | 上界 | 算法在最坏情况下的复杂度上限，用大O表示。 |
| lower bound | 下界 | 算法在最好情况下的复杂度下限，用大Ω（Omega）表示。 |
| asymptotic notation | 渐近记号 | 描述算法复杂度的统一符号体系，包括大O、大Ω、大Θ三种，分别对应上界、下界和上下界相等的情况。 |
| hard-coded | 硬编码 | 将本应可配置的参数（如数组长度、固定值）直接写死在代码中的做法，会降低代码的可维护性，属于不推荐的编程实践。 |
| segmentation fault | 段错误 | C/C++等编译型语言中常见的运行时错误，指程序访问了未被分配、或无权限访问的内存区域，通常由数组越界、空指针访问等问题导致。 |
| struct | 结构体 | C语言中允许用户自定义的复合数据类型，可以将多个不同类型的属性打包为一个整体，方便关联存储相关数据。 |
| dot notation | 点表示法 | 访问结构体属性的语法，格式为`结构体变量.属性名`，用于读取或修改结构体的对应属性。 |
| sorting | 排序 | 将无序序列调整为有序序列的过程，是计算机科学中最经典的基础问题之一，有多种不同效率的实现算法。 |
| selection sort | 选择排序 | 基础排序算法，每次遍历未排序区间找到最小元素，交换到有序区间末尾，复杂度为O(n²)。 |
| bubble sort | 冒泡排序 | 基础排序算法，通过反复交换相邻逆序元素将大元素逐步移动到序列末尾，最坏复杂度O(n²)，优化后最好复杂度可达Ω(n)。 |
| recursion | 递归 | 函数调用自身的编程技巧，适用于可以拆解为相同结构子问题的场景，需要定义基线条件避免无限递归。 |
| base case | 基线条件 | 递归的终止条件，触发后不再继续调用自身，避免无限循环，也译作边界条件、终止条件。 |
| recursive case | 递归条件 | 递归函数中调用自身的逻辑，每次调用会缩小问题规模，逐步向基线条件靠近。 |
| merge sort | 归并排序 | 基于递归实现的高效排序算法，采用分治思想将序列拆分排序后合并，复杂度为O(n log n)，是稳定的高效排序算法。 |

> 本文转自 [https://cs50.harvard.edu/x/notes/3/](https://cs50.harvard.edu/x/notes/3/)，如有侵权，请联系删除。