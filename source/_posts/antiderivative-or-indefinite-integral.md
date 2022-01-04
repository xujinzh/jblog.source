---
title: 不定积分
author: Jinzhong Xu
mathjax: true
date: 2020-03-24 12:30:15
tags:
- antiderivatiive
- indefinite integral
categories:
- maths
- mathematical analysis
---

积分法属于微分法的逆运算，就像减法是加法的逆运算，除法是乘法的逆运算一样。

<!--more-->

# 原函数与不定积分

**定义 1** &emsp; 设函数$f$与$F$在区间$I$上都有定义。若
$$
F^{\prime}(x) = f(x), \ x \in I,
$$
则称$F$为$f$在区间$I$上的一个原函数。

这一节解决满足什么样条件的函数存在原函数。

**定理 8.1** &emsp; 若函数$f$在区间$I$上连续，则$f$在$I$上存在原函数$F$，即$F^{\prime} = f(x), \ x \in I$.

初等函数在定义域内是连续函数，因此，初等函数都存在原函数。注意，初等函数的原函数不一定还是初等函数。

**定理 8.2** &emsp; 设$F$是$f$在区间$I$上的一个原函数，则

1. $F + C$ 也是$f$在$I$上的原函数，其中$C$为任意常量函数；
2. $f$在$I$上的任意两个原函数之间，只可能相差一个常数。

**定义 2** &emsp; 函数$f$在区间$I$上的全体原函数称为$f$在$I$上的不定积分，记作
$$
\int f(x) \mathrm{d}x,
$$
其中称$\int$为积分号，$f(x)$为被积函数，$f(x)\mathrm{d}x$为被积表达式，$x$为积分变量。**尽管上述记号中各个部分都有其特定的名称，但在使用时必须把它们看作一个整体。**

不定积分与原函数是总体与个体的关系，即若$F$是$f$的一个原函数，则$f$的不定积分是一个函数族$\\{F + C\\}$，其中$C$是任意常数，为了方便常写作
$$
\int f(x) \mathrm{d}x = F(x) + C.
$$
称$C$为积分常数，它可取任一实数值。

**不定积分的几何意义** &emsp; 若$F$是$f$的一个原函数，则称$y = F(x)$的图像为$f$的一条积分曲线。于是，$f$的不定积分在几何上表示$f$的某一积分曲线沿纵轴方向任意平移所得一切积分曲线组成的曲线族。显然，若在每一条积分曲线上横坐标相同的点处作切线，则这些切线互相平行。

当给定原函数的某一个初始条件后，可以求出满足该初始条件的某一原函数子集合。

# 基本积分表

求导数相对于求积分会容易，对于某些基本的积分公式列出如下：

1. $\int 0 \mathrm{d}x = C$;
2. $\int 1 \mathrm{d}x = \int \mathrm{d}x = x + C$;
3. $\int x^{\alpha} \mathrm{d}x = \frac{x^{\alpha + 1}}{\alpha + 1} + C \ (\alpha \neq -1, \ x > 0)$;
4. $\int \frac{1}{x} \mathrm{d}x = \ln |x| + C \ (x \neq 0)$;
5. $\int e^x \mathrm{d}x = e^x + C$;
6. $\int a^x \mathrm{d}x = \frac{a^x}{\ln a} + C \ (a > 0, \ a \neq 1)$;
7. $\int \cos ax \mathrm{d}x = \frac{1}{a} \sin ax + C \ (a \neq 0)$;
8. $\int \sin ax \mathrm{d}x = - \frac{1}{a} \cos ax + C \ (a \neq 0)$;
9. $\int \sec^2 x \mathrm{d}x = \tan x + C$;
10. $\int \csc^2 x \mathrm{d}x = -\cot x + C$;
11. $\int \sec x \cdot \tan x \mathrm{d}x = \sec x + C$;
12. $\int \csc x \cdot \cot x \mathrm{d}x = -\csc x + C$;
13. $\int \frac{1}{\sqrt[2]{1 - x^2}} \mathrm{d}x = \arcsin x + C = -\arccos x + C$;
14. $\int \frac{1}{1 + x^2} \mathrm{d}x = \arctan x + C = -\text{arccot}\ x + C_1$.

**定理 8.3 **&ensp; 若函数$f$与$g$在区间$I$上都存在原函数，$k_1, k_2$为两个任意常数，则$k_1 f + k_2 g$在$I$上也存在原函数，且当$k_1$与$k_2$不同时为零时有
$$
\int [k_1 f(x) + k_2 g(x)] \mathrm{d}x = k_1 \int f(x) \mathrm{d}x + k_2 \int g(x) \mathrm{d}x.
$$
一些事实：

每一个含有第一类间断点的函数都没有原函数。

导函数不存在第一类间断点。

某些导函数可能存在第二类间断点。如下函数的导函数$f(x)$在$x = 0$处是第二类间断点
$$
F(x) = 
\begin{cases}
x^{3/2} \sin \frac{1}{x}, \quad x \in (0, 1]; \\\\
0, \quad \quad \quad x = 0.
\end{cases}
$$
