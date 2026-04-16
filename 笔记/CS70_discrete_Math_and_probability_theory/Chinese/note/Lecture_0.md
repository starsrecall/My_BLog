EECS 70 离散数学（Discrete Mathematics）与概率论（Probability Theory）
2020年秋季学期 第0讲课程讲义（Note 0）

# 集合与数学符号复习

集合（set）是由确定的对象构成的整体，这些对象称为集合的**元素（element）**或成员（member），可以是任意类型，包括数字、字母、人物、城市，甚至其他集合。按照数学惯例，集合通常用大写字母表示，可以通过列出其所有元素并用大括号包裹的方式来定义或描述。例如，我们可以定义集合$A$为前5个素数构成的集合，也可以显式写作：$A = \{2, 3, 5, 7, 11\}$。若$x$是$A$的元素，则记为$x \in A$；同理，若$y$不是$A$的元素，则记为$y \notin A$。

若两个集合$A$和$B$包含完全相同的元素，则称二者相等，记为$A = B$。元素的顺序和重复次数不影响集合的相等性，因此$\{\text{red}, \text{white}, \text{blue}\} = \{\text{blue}, \text{white}, \text{red}\} = \{\text{red}, \text{white}, \text{white}, \text{blue}\}$。对于更复杂的集合，有时可以使用描述式记法来定义。例如，全体有理数（rational number）构成的集合记为$\mathbb{Q}$，可以写作：
$$\left\{ \frac{a}{b} \mid a, b \text{ 为整数，} b \neq 0 \right\}$$
读作“所有分子为整数、分母为非零整数的分数构成的集合”。

## 基数
我们也可以讨论集合的大小，即其**基数（cardinality）**。若$A = \{1,2,3,4\}$，则$A$的基数记为$|A|$，值为4。集合的基数可以为0，该集合称为**空集（empty set）**，记为符号$\emptyset$。集合也可以包含无穷多个元素，例如全体整数、素数或奇数构成的集合。

## 子集与真子集
若集合$A$的每一个元素都属于集合$B$，则称$A$是$B$的**子集（subset）**，记为$A \subseteq B$；等价地可以写作$B \supseteq A$，即$B$是$A$的**超集（superset）**。若$A$严格包含于$B$，即$A$中至少缺少一个$B$的元素，则称$A$是$B$的**真子集（proper subset）**，记为$A \subset B$。

例如，考虑集合$B = \{1, 2, 3, 4, 5\}$，则$\{1, 2, 3\}$既是$B$的子集也是$B$的真子集，而$\{1, 2, 3, 4, 5\}$是$B$的子集但不是真子集。关于子集有如下几条基本性质：
- 空集（记为$\{\}$或$\emptyset$）是任意非空集合$A$的真子集：$\{\} \subset A$
- 空集是任意集合$B$的子集：$\{\} \subseteq B$
- 任意集合$A$都是其自身的子集：$A \subseteq A$

## 交集与并集
集合$A$与集合$B$的**交集（intersection）**记为$A \cap B$，是包含所有同时属于$A$和$B$的元素的集合。若$A \cap B = \emptyset$，则称两个集合**不相交（disjoint）**。

集合$A$与集合$B$的**并集（union）**记为$A \cup B$，是包含所有属于$A$、或属于$B$、或同时属于二者的元素的集合。例如，若$A$是全体正偶数构成的集合，$B$是全体正奇数构成的集合，则$A \cap B = \emptyset$，$A \cup B = \mathbb{Z}^+$，即全体正整数构成的集合。

交集与并集有如下几条性质：
- $A \cup B = B \cup A$
- $A \cup \emptyset = A$
- $A \cap B = B \cap A$
- $A \cap \emptyset = \emptyset$

## 补集

若$A$和$B$是两个集合，则$A$在$B$中的**相对补集（relative complement）**记为$B - A$或$B \setminus A$，是包含所有属于$B$但不属于$A$的元素的集合：$B \setminus A = \{x \in B \mid x \notin A\}$。

例如，若$B = \{1, 2, 3\}$，$A = \{3, 4, 5\}$，则$B \setminus A = \{1, 2\}$。再例如，若$\mathbb{R}$是全体实数构成的集合，$\mathbb{Q}$是全体有理数构成的集合，则$\mathbb{R} \setminus \mathbb{Q}$是全体无理数构成的集合。

补集有如下几条重要性质：
- $A \setminus A = \emptyset$
- $A \setminus \emptyset = A$
- $\emptyset \setminus A = \emptyset$

## 常用集合
在数学中，部分集合的使用频率极高，因此被赋予了专用符号。这些数值集合包括：
- $\mathbb{N}$ 表示全体自然数（natural number）构成的集合：$\{0, 1, 2, 3, \dots\}$
  [译者注：国内部分教材约定自然数从1开始计数，本课程统一规定自然数包含0]
- $\mathbb{Z}$ 表示全体整数（integer）构成的集合：$\{\dots, -2, -1, 0, 1, 2, \dots\}$
- $\mathbb{Q}$ 表示全体有理数构成的集合：$\left\{ \frac{a}{b} \mid a, b \in \mathbb{Z}, b \neq 0 \right\}$
- $\mathbb{R}$ 表示全体实数（real number）构成的集合
- $\mathbb{C}$ 表示全体复数（complex number）构成的集合

此外，两个集合$A$和$B$的**笛卡尔积（Cartesian product，又称叉积）**记为$A \times B$，是所有第一个分量属于$A$、第二个分量属于$B$的有序对构成的集合，用集合记号表示为：$A \times B = \{(a, b) \mid a \in A, b \in B\}$。例如，若$A = \{1, 2, 3\}$，$B = \{u, v\}$，则$A \times B = \{(1, u), (1, v), (2, u), (2, v), (3, u), (3, v)\}$。

给定集合$S$，另一个常用集合是$S$的**幂集（power set）**，记为$\mathcal{P}(S)$，是$S$的所有子集构成的集合：$\{T \mid T \subseteq S\}$。例如，若$S = \{1, 2, 3\}$，则其幂集为：$\mathcal{P}(S) = \{\{\}, \{1\}, \{2\}, \{3\}, \{1, 2\}, \{1, 3\}, \{2, 3\}, \{1, 2, 3\}\}$。一个值得注意的性质是：若$|S| = k$，则$|\mathcal{P}(S)| = 2^k$。

## 数学符号
### 求和与乘积
对于大量项的求和或乘积，我们有简化的记法。例如，$1 + 2 + \dots + n$无需用省略号表示，可以写作$\sum_{i=1}^n i$。更一般地，和式$f(m) + f(m + 1) + \dots + f(n)$可以写作$\sum_{i=m}^n f(i)$，因此$\sum_{i=5}^n i^2 = 5^2 + 6^2 + \dots + n^2$。

对于乘积$f(m)f(m + 1)\dots f(n)$，我们使用记号$\prod_{i=m}^n f(i)$。例如，$\prod_{i=1}^n i = 1 \cdot 2 \cdot \dots \cdot n$。

### 全称量词与存在量词
考虑如下命题：对于所有自然数$n$，$n^2 + n + 41$都是素数。这里$n$的取值范围是全体自然数集合$\mathbb{N}$，用记号可以写作$(\forall n \in \mathbb{N})(n^2 + n + 41 \text{ 是素数})$，其中我们使用了**全称量词（universal quantifier）**$\forall$（意为“对于所有”）。

这个命题是真命题吗？如果代入较小的$n$值，你会发现$n^2 + n + 41$确实都是素数，但只要稍作推导就能找到使其不成立的较大$n$值，你能找出来吗？因此命题$(\forall n \in \mathbb{N})(n^2 + n + 41 \text{ 是素数})$是假命题。

**存在量词（existential quantifier）**$\exists$（意为“存在”）用于如下类型的命题：$\exists x \in \mathbb{Z},\ x < 2 \text{ 且 } x^2 = 4$。该命题表示存在一个小于2的整数$x$，其平方等于4，这是一个真命题。

我们也可以同时使用两类量词构造命题：
1. $\forall x \in \mathbb{Z}\ \exists y \in \mathbb{Z}\ \ y > x$
2. $\exists y \in \mathbb{Z}\ \forall x \in \mathbb{Z}\ \ y > x$

第一个命题表示，对于任意整数，我们都能找到一个比它更大的整数；第二个命题的含义则完全不同：存在一个最大的整数！第一个命题为真，第二个为假。

