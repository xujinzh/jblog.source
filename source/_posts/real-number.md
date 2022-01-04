---
title: 实数
author: Jinzhong Xu
mathjax: true
date: 2020-03-19 17:27:07
tags:
- real number
- maths
categories:
- maths
- mathematical analysis
---

实数包含有理数和无理数。有理数可以用分数形式$\frac{p}{q}, p, q$为整数，$q \neq 0$表示；也可以用有限十进制小数或无限十进制循环小数表示。无理数可以用不循环的小数表示。有理数和无理数统称为实数。

<!--more-->

# 实数

为了统一，将有限小数（包括整数）也表示为无限小数。规定：对于正有限小数$x$, 当$x = a_0.a_1 a_2 \cdots a_n$时，其中$0 \leq a_i \leq 9, i = 1, 2, \cdots, n, a_n \neq 0, a_0$为非负整数，记
$$
x = a_0. a_1 a_2 \cdots (a_n - 1)9999 \cdots,
$$
而当$x = a_0$为正整数时，则记
$$
x = (a_0 - 1).9999 \cdots,
$$
对于负数，可以先转化为正数考虑，如$x$为负数，先将$-x$表示为无限循环小数，然后在添加负号。

对于0，规定表示为$0.0000 \cdots$，于是任意实数都可以用一个确定的无限小数表示。

**定义1** 给定两个非负实数
$$
x = a_0.a_1 a_2 \cdots a_n \cdots,\quad y = b_0.b_1 b_2 \cdots b_n \cdots,
$$
其中$a_0, b_0$为非负整数，$a_k, b_k \ (k = 1, 2, \cdots)$为整数，$0 \leq a_k \leq 9, \ 0 \leq b_k \leq 9$. 若有
$$
a_k = b_k, \ k = 0, 1, 2, \cdots,
$$
则称$x$与$y$相等，记为$x = y$；若$a_0 > b_0$或存在非负整数$l$，使得
$$
a_k = b_k \ (k = 0, 1, 2, \cdots, l) \ 但是 \ a_{i + 1} > b_{i + 1}
$$
则称$x$大于$y$或$y$小于$x$，分别记为$x > y$或$y < x$.

**定义2** 设$x = a_0.a_1 a_2 \cdots a_n \cdots$为非负实数。称有理数
$$
x_n = a_0.a_1 a_2 \cdots a_n
$$
为实数$x$的$n$位不足近似，而有理数
$$
\overline{x_n} = x_n + \frac{1}{10^n}
$$
称为$x$的$n$位过剩近似，$n = 0, 1, 2, \cdots$.

对于负实数$x = -a_0.a_1 a_2 \cdots a_n \cdots$,其$n$位不足近似和过剩近似分别规定为
$$
x_n = -a_0.a_1 a_2 \cdots a_n - \frac{1}{10^n} \ 与 \ \overline{x_n} = -a_0.a_1 a_2 \cdots a_n.
$$
注意，实数$x$的不足近似$x_n$是单调递增的，而过剩近似$\overline{x_n}$是单调递减的。

全体实数构成的集合一般用$\mathbb{R}$表示，实数具有如下一些主要性质：

1. 实数集对加、减、乘、除（除数不为0）四则运算是封闭的，即任意两个实数的和、差、积、商仍然是实数；
2. 实数集是有序的，即任意两个实数$a, b$必满足下述三个关系之一：$a < b, \ a = b, \ a > b$;
3. 实数的大小关系具有传递性，即若$a > b, \ b > c$，则$a > c$;
4. 实数具有阿基米德（Archimedes）性，即对任何$a, b \in \mathbb{R}$，若$b > a > 0$，则存在正整数$n$，使得$na > b$；
5. 实数集$\mathbb{R}$具有稠密性，即任何两个不相等的实数之间必有另一个实数，且既有有理数，也有无理数；
6. 如果在一直线上确定一点$O$作为原点，指定一个正方向，规定一个单位长度，则此直线称为一个数轴。实数集和数轴上的点一一对应。

# 绝对值和不等式

实数$x$的绝对值定义为
$$
|x| =
\begin{cases}
x, \ x \geq 0, \\\\
-x, \ x < 0.
\end{cases}
$$
表示在数轴上点$x$到原点的距离。

实数的绝对值具有如下一些性质：

1. $|x| = |-x| \geq 0$；当且仅当$x = 0$时有$|x| = 0$；

2. $- |x| \leq x \leq |x|$；

3. $|x| < h \Leftrightarrow - h < x < h;\ |x| \leq h \Leftrightarrow -h \leq x \leq h \ (h > 0)$；

4. 对于任何$a, b \in \mathbb{R}$有如下的三角不等式：
   $$
   |x| - |y| \leq |x \pm y| \leq |x| + |y|;
   $$

5. $|xy| = |x| |y|$；
6. $|\frac{x}{y}| = \frac{|x|}{|y|} \ y \neq 0$；

# 例题

1. 设$a, b \in \mathbb{R}$，若对任何正数$\varepsilon$有$|a - b| < \varepsilon$，则$a = b$.
2. 设$x > 0, b >0, a \neq b$, 则$\frac{a + x}{b + x}$介于$1$与$\frac{a}{b}$之间。