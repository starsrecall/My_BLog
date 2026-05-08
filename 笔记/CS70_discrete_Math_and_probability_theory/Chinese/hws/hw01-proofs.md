# hw01-proofs.pdf
## CS 70 离散数学（Discrete Mathematics）与概率论（Probability Theory）
### 2020年秋季学期 第1次作业习题集（HW 1）
- 截止时间：9月4日（周五）晚10:00
- 宽限期截止至9月4日（周五）晚11:59

---

#### 其他说明（Sundry）
在撰写最终提交的作业前，请简要说明你完成本次作业的过程：你与哪些人合作完成了本次作业？请列出他们的姓名与电子邮箱。（如果是作业派对集体完成，仅描述小组情况即可。）
[译者注：作业派对（Homework Party）是UC Berkeley学生常见的集体作业形式，多人聚集共同讨论解题]

---

### 1 微积分复习
(a) 请计算下述求和式的闭式表达式：
$$\sum_{k=1}^{\infty} \frac{9}{2k}$$
[译者注：此处求和式的分母疑似为$2^k$（2的k次方），若为$2k$则该级数为调和级数，发散无闭式解，建议核对原文]

(b) 请使用求和符号写出与下述陈述等价的表达式：
从1开始的前n个连续正奇数的和

(c) 请计算下述积分：
$$\int_{0}^{\infty} \sin(t)e^{-t} dt$$

(d) 求下述函数的最大值，并指出最大值点：
$$f(x) = -x \cdot \ln x$$

---

### 2 命题逻辑（Propositional Logic）练习
(a)-(c)小题请将英语句子转换为命题逻辑表达式；(d)-(f)小题请将命题转换为自然语言。其中(f)小题中，$P(a)$表示“a是素数（Prime）”的命题。

(a) 方程$x2 = 0$有且仅有一个实数解。
(b) 对于任意两个不相等的有理数，二者之间存在另一个有理数。
(c) 若一个整数的平方大于4，则该整数大于2或小于-2。
(d) $(\forall x \in \mathbb{R}) (x \in \mathbb{C})$
(e) $(\forall x, y \in \mathbb{Z})(x2 − y2 6 = 10)$
[译者注：此处“6 =”系OCR识别误差，应为“≠”（不等于）；“x2”“y2”应为x、y的平方，即$x^2$、$y^2$]
(f) $(\forall x \in \mathbb{N}) \left[ (x > 1) \implies (\exists a, b \in \mathbb{N}) \left( (a + b = 2x) \land P(a) \land P(b) \right) \right]$

---

### 3 重言式（Tautology）与矛盾式（Contradiction）
请将以下每个命题归类到下述三类之一，其中$P$和$Q$是任意命题：
• 对$P$和$Q$的所有真值组合都为真（重言式 (Tautology)）
• 对$P$和$Q$的所有真值组合都为假（矛盾式 (Contradiction)）
• 既非重言式也非矛盾式

请使用真值表说明你的结论。
(a) $P \implies (Q \land P) \lor (\neg Q \land P)$
(b) $(P \lor Q) \lor (P \lor \neg Q)$
(c) $P \land (P \implies \neg Q) \land Q$
(d) $(\neg P \implies Q) \implies (\neg Q \implies P)$
(e) $(\neg P \implies \neg Q) \land (P \implies \neg Q) \land Q$
(f) $\neg(P \land Q) \land (P \lor Q)$

---

### 4 证明或证伪
对于以下每个命题，要么证明其成立，要么通过反例证伪。
(a) $(\forall n \in \mathbb{N})$ 若$n$是奇数，则$n2 + 4n$是奇数。
[译者注：此处“n2”应为$n^2$（n的平方），系OCR识别时上标丢失]
(b) $(\forall a, b \in \mathbb{R})$ 若$a + b \leq 15$，则$a \leq 11$或$b \leq 4$。
(c) $(\forall r \in \mathbb{R})$ 若$r^2$是无理数（Irrational Number），则$r$是无理数。
[译者注：此处“r2”应为$r^2$（r的平方），系OCR识别时上标丢失]
(d) $(\forall n \in \mathbb{Z}^+) \ 5n3 > n!$。（注：$\mathbb{Z}^+$表示正整数集）
[译者注：此处“5n3”应为$5n^3$（5乘以n的三次方），系OCR识别时上标丢失]

---

### 5 孪生素数（Twin Prime）
(a) 设$p > 3$是素数。证明：存在整数$k$，使得$p$可以表示为$3k+1$或$3k-1$的形式。
(b) 孪生素数是指差为2的素数对$(p,q)$。请利用(a)小题的结论，证明：5是唯一一个可以同时属于两个不同孪生素数对的素数。

---



### 6 社交网络
本题与预习题（Vitamin）第2题的设定一致：一场派对共有$n$人，每两人之间要么是朋友关系，要么是陌生人关系。请证明以下命题，或给出反例证伪。
[译者注：Vitamin是UC Berkeley CS70课程的课前预习题，通常用于巩固前置知识]

(a) 当$n=5$时，对所有可能的情况，都存在一个3人小组，其中所有人互为朋友，或所有人互为陌生人。
(b) 当$n=6$时，对所有可能的情况，都存在一个3人小组，其中所有人互为朋友，或所有人互为陌生人。

---

### 7 集合运算的保持性
对于函数$f$，定义集合$X$的**像（Image）**为集合$f(X) = \{y \mid \exists x \in X, y = f(x)\}$。定义集合$Y$的**逆像（Inverse Image / Preimage）**为集合$f^{-1}(Y) = \{x \mid f(x) \in Y\}$。请证明以下关于集合$A,B$的命题，通过这些证明你将了解：逆像运算保持集合运算，而像运算通常不保持。

提示：对于集合$X$和$Y$，$X=Y$当且仅当$X \subseteq Y$且$Y \subseteq X$。要证明$X \subseteq Y$，只需证明$(\forall x) (x \in X \implies x \in Y)$。

(a) $f^{-1}(A \cup B) = f^{-1}(A) \cup f^{-1}(B)$
(b) $f^{-1}(A \cap B) = f^{-1}(A) \cap f^{-1}(B)$
(c) $f^{-1}(A \setminus B) = f^{-1}(A) \setminus f^{-1}(B)$
(d) $f(A \cup B) = f(A) \cup f(B)$
(e) $f(A \cap B) \subseteq f(A) \cap f(B)$，并给出一个等号不成立的例子。
(f) $f(A \setminus B) \supseteq f(A) \setminus f(B)$，并给出一个等号不成立的例子。

