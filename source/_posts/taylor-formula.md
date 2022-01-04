---
title: 泰勒公式
author: Jinzhong Xu
mathjax: true
date: 2020-03-18 22:58:51
tags:
- taylor's formula
- maths
categories:
- maths
- mathematical analysis
---

Taylor's Formula (泰勒公式) 本节为参考华东师范大学《数学分析 上册》，学习本书的心得以及重点知识点总结。

1. 泰勒公式源于多项式的简单性，多项式逼近函数用于近似计算和理论分析；
2. 在$x = 0$处展开的泰勒公式又称为 Maclaurin(麦克劳林) 公式；
3. 本节讨论了两种余项的泰勒公式，分别是用于定性分析的佩亚诺余项和用于定量分析的拉格朗日余项；
4. 带有佩亚诺余项的泰勒公式多用于求函数极限，一般先考虑等价无穷小，然后对于复杂函数考虑泰勒公式；
5. 利用泰勒公式可以近似计算无理数等的近似值。

<!--more-->

# 带有佩亚诺型余项的泰勒公式

**定理6.9** 若函数$f$在点$x_0$存在直至$n$阶导数，则有$f(x) = T_n(x) + o((x - x_0)^n)$，这里
$$
T_n(x) = f(x_0) + f^{\prime}(x_0)(x - x_0) + \frac{f^{(2)}(x_0)}{2!}(x - x_0)^2 + \cdots + \frac{f^{(n)}(x_0)}{n!}(x - x_0)^n.
$$
称为函数$f$在点$x_0$处的泰勒多项式，$T_n(x)$的各项系数$\frac{f^{(k)}(x_0)}{k!}\ (k = 1, 2, \cdots, n)$称为泰勒系数。具有性质
$$
f^{(k)}(x_0) = T^{(k)}_n(x_0), \ k = 0, 1, 2, \cdots, n.
$$

**注意，这里只要求在一点$x_0$处$n$阶可导，所以，$n - 1$阶导函数后不能使用柯西中值定理** 

$R_n(x) = f(x) - T_n(x)$称为泰勒公式的余项，形如$o((x - x_0)^n)$的余项称为佩亚诺（Peano）型余项，所以上面公式又称为带有佩亚诺型余项的泰勒公式。  


称如下
$$
f(x) = f(0) + f^{\prime}(0)x + \frac{f^{\prime\prime}(0)}{2!} x^2 + \cdots + \frac{f^{(n)}(0)}{n!} x^n + o(x^n). 
$$

为（带有佩亚诺型余项的）麦克劳林（Maclaurin）公式。

**下面列出一些常用的带有佩亚诺型余项的麦克劳林公式**：



1. $$
   e^x = 1 + x + \frac{x^2}{2!} + \cdots + \frac{x^n}{n!} + o(x^n);
   $$

2. $$
   \sin x = x - \frac{x^3}{3!} + \frac{x^5}{5!} + \cdots + (-1)^{m-1}\frac{x^{2m - 1}}{(2m - 1)!} + o(x^{2m});
   $$

3. $$
   \cos x = 1 - \frac{x^2}{2!} + \frac{x^4}{4!} + \cdots + (-1)^m \frac{x^{2m}}{(2m)!} + o(x^{2m + 1});
   $$

4. $$
   \ln(1 + x) = x - \frac{x^2}{2} + \frac{x^3}{3} + \cdots + (-1)^{n-1}\frac{x^n}{n} + o(x^n);
   $$

5. $$
   (1 + x)^{\alpha} = 1 + \alpha x + \frac{\alpha(\alpha - 1)}{2!} x^2 + \frac{\alpha(\alpha - 1)\cdots(\alpha - n + 1)}{n!} x^n + o(x^n);
   $$

6. $$
   \frac{1}{1 - x} = 1 + x + x^2 + \cdots + x^n + o(x^n).
   $$

   

**记忆方法：**

1. $e^x$ 各阶导数相同，因此$f^{\prime}(0) = f^{\prime\prime}(0) = \cdots = f^{(n)}(0) = e^0 = 1$，形式上最解决泰勒公式。
2. $\sin x \sim x, \ x \to 0$，且为基函数，只能取奇数幂项。
3. $\cos x \sim 1, x \to 0$，且为偶函数，只能取偶次幂项。
4. $\frac{1}{1 - x}$为等比数列极限$\frac{a_1}{1 - q}$的特殊形式。
5. $\ln(1 + x)$的一次导数为$\frac{1}{1 - (-x)}$，因此是导数展开的一次积分。另外，$\ln(1 + x) \sim x,\ x \to 0.$
6. $(1 + x)^{\alpha}$可以参考$(1 + x)^n$的二项式展开，即每项为$C^i_n x^{i}$.

# 带有拉格朗日型余项的泰勒公式

拉格朗日型余项是一种定量形式的余项，便于对逼近误差进行具体的计算或估计。

**定理 6.10** &emsp; (泰勒定理) &emsp; 若函数$f$在$[a, b]$上存在直至$n$阶的连续导函数，在$(a, b)$上存在$(n + 1)$阶导函数，则对任意给定的$x, x_0 \in [a, b]$，至少存在一点$\xi \in (a, b)$，使得
$$
f(x) = f(x_0) + f^{\prime}(x_0)(x - x_0) + \frac{f^{\prime\prime}(x_0)}{2!}(x - x_0)^2 + \cdots + \frac{f^{(n)}(x_0)}{n!}(x - x_0)^n + \frac{f^{(n + 1)}(\xi)}{(n + 1)!}(x - x_0)^{n + 1}.
$$
<font color='dd0000'>注意：这里是在区间上存在$n$阶连续导函数，可以使用柯西中值定理</font>。  

上面公式称为带有拉格朗日型余项的泰勒公式，当取$x_0 = 0$时，称下式
$$
f(x) = f(0) + f^{\prime}(0)x + \frac{f^{\prime\prime}(0)}{2!}x^2 + \cdots + \frac{f^{(n)}(0)}{n!}x^n + \frac{f^{(n + 1)}(\theta x)}{(n + 1)!}x^{n + 1} \\\\
(0 < \theta < 1)
$$

为带有拉格朗日型余项的麦克劳林公式。  

**下面列出一些常用的带有拉格朗日型余项的麦克劳林公式：**

1. $$
   e^x = 1 + x + \frac{x^2}{2!} + \cdots + \frac{x^n}{n!} + \frac{e^{\theta x}}{(n + 1)!}x^{n + 1}, \ 0 < \theta < 1, \ x \in (-\infty, +\infty).
   $$

2. $$
   \sin x = x - \frac{x^3}{3!} + \frac{x^5}{5!} + \cdots + (-1)^{m - 1}\frac{x^{2m - 1}}{(2m - 1)!} + (-1)^m \frac{\cos \theta x}{(2m + 1)!}x^{2m + 1}, \\\\
   0 < \theta < 1, \ x \in (-\infty, +\infty).
   $$

3. $$
   \cos x = 1 - \frac{x^2}{2!} + \frac{x^4}{4!} + \cdots + (-1)^m \frac{x^{2m}}{(2m)!} + (-1)^{m + 1}\frac{\cos \theta x}{(2m + 2)!}x^{2m + 2}, \\\\
   0 < \theta < 1, \ x \in (-\infty, +\infty).
   $$

4. $$
   \ln(1 + x) = x - \frac{x^2}{2} + \frac{x^3}{3} + \cdots + (-1)^{n - 1}\frac{x^n}{n} + (-1)^n \frac{x^{n + 1}}{(n + 1)(1 + \theta x)^{n + 1}}, \\\\
   0 < \theta < 1, \ x > -1.
   $$

5. $$
   (1 + x)^{\alpha} = 1 + \alpha x + \frac{\alpha(\alpha - 1)}{2!}x^2 + \cdots + \frac{\alpha(\alpha - 1) \cdots (\alpha - n + 1)}{n!}x^n + \\\\
   \frac{\alpha(\alpha - 1)\cdots(\alpha - n)}{(n + 1)!}(1 + \theta x)^{\alpha - n - 1}x^{n + 1}, \  0 < \theta < 1, \ x > -1.
   $$

6. $$
   \frac{1}{1 - x} = 1 + x + x^2 + \cdots + x^n + \frac{x^{n + 1}}{(1 - \theta x)^{n + 2}}, \ 0 < \theta < 1, \ |x| < 1.
   $$

