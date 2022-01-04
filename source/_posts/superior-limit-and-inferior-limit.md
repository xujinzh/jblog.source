---
title: 上极限和下极限
author: Jinzhong Xu
mathjax: true
date: 2020-03-22 11:22:05
tags:
- limit
- superior
- inferior
- maths
categories:
- maths
- mathematical analysis
---

上极限和下极限是针对数列而言的，需谨慎考虑数集或点集。

**定义 1** &emsp; 若在数$a$的任一领域内含有<font color='dd0000'>数列</font>$\\{x_n\\}$的无限多个项，则称$a$为数列$\\{x_n\\}$的一个聚点。

<!--more-->

*当不区分实数与数轴上的点的情况下，点列的聚点等同于数列的聚点，也称为极限点。需要说明的是，点集和数集的聚点不能叫作极限点*。

**点列的聚点就是其收敛子列的极限**。

**定理 7.4** &emsp; 有界点列（数列）$\\{x_n\\}$至少有一个聚点，且存在最大聚点和最小聚点。

**定义 2** &emsp; 有界数列（点列）$\\{x_n\\}$的最大聚点$\overline{A}$与最小聚点$\underline{A}$分别称为$\\{x_n\\}$的上极限与下极限，记作
$$
\overline{A} = \varlimsup_{n \to \infty}x_n, \quad \underline{A} = \varliminf_{n \to \infty}x_n. 
$$
**这里需要注意下极限的书写方法**。

**定理 7.5** &emsp; 对任何有界数列$\\{x_n\\}$有
$$
\varliminf_{n \to \infty}x_n \leq \varlimsup_{n \to \infty}x_n.
$$
**定理 7.6** &emsp; 对于数列$\\{x_n\\}$，
$$
\lim_{n \to \infty}x_n = A
$$
的充要条件是
$$
\varlimsup_{n \to \infty}x_n = \varliminf_{n \to \infty}x_n = A
$$
**定理 7.7** &emsp; 设$\\{x_n\\}$为有界数列，

1. $\overline{A}$为$\\{x_n\\}$上极限的充要条件是：任给$\varepsilon > 0$，

   i. 存在$N > 0$，使得当$n > N$时有$x_n < \overline{A} + \varepsilon$;

   ii. 存在子列$\\{x_{n_k}\\}$满足$x_{n_k} > \overline{A} - \varepsilon, \ k = 1, 2, \cdots.$

2. $\underline{A}$为$\\{x_n\\}$下极限的充要条件是：任给$\varepsilon > 0$，

   i. 存在$N > 0$，使得当$n > N$时有$x_n > \underline{A} - \varepsilon$；

   ii. 存在子列$\\{x_{n_k}\\}$满足$x_{n_k} < \underline{A} + \varepsilon, \ k = 1, 2, \cdots.$

该定理的另一种表述形式如下：

**定理 $7.7^{\prime}$** &emsp; 设$\\{x_n\\}$为有界数列，

1. $\overline{A}$为上极限的充要条件是对任何$\alpha > \overline{A}$，$\\{x_n\\}$中大于$\alpha$的项至多有有限个；对任何$\beta < \overline{A}$，$\\{x_n\\}$中大于$\beta$的项有无限个。
2. $\underline{A}$为下极限的充要条件是对任何$\alpha < \underline{A}$，$\\{x_n\\}$中小于$\alpha$的项至多有有限个；对任何$\beta > \underline{A}$，$\\{x_n\\}$中小于$\beta$的项有无限个。

**定理 7.8** &emsp; （上、下极限的保不等式性）&emsp; 设有界数列$\\{a_n\\}$, $\\{b_n\\}$满足：存在$N_0 > 0$，当$n > N_0$时，有$a_n \leq b_n$，则
$$
\varlimsup_{n \to \infty}a_n \leq \varlimsup_{n \to \infty}b_n, \ \varliminf_{n \to \infty}a_n \leq \varliminf_{n \to \infty}b_n.
$$
特别地，若$\alpha, \beta$为常数，又存在$N_0 > 0$，当$n > N_0$时有$\alpha \leq a_n \leq \beta$，则
$$
\alpha \leq \varliminf_{n \to \infty}a_n \leq \varlimsup_{n \to \infty}a_n \leq \beta.
$$
假设$\\{a_n\\}$,$\\{b_n\\}$为有界数列，则
$$
\varlimsup_{n \to \infty}(a_n + b_n) \leq \varlimsup_{n \to \infty}a_n + \varlimsup_{n \to \infty}b_n.
$$
下面给出一个非常重要的定理，该定理在实变函数中也会出现。

**定理 7.9**&emsp; 设$\\{x_n\\}$为有界数列，

1. $\overline{A}$为$\\{x_n\\}$上极限的充要条件是
   $$
   \overline{A} = \lim_{n \to \infty}\sup_{k \geq n}\\{x_k\\}
   $$
   
2. $\underline{A}$为$\\{x_n\\}$下极限的充要条件是
   $$
   \underline{A} = \lim_{n \to \infty}\inf_{k \geq n}\\{x_k\\}
   $$
   

一些事实：

1. 设$\\{a_n\\}$, $\\{b_n\\}$为有界数列，则

   1. $$
      \varliminf_{n \to \infty}a_n = - \varlimsup_{n \to \infty}(-a_n);
      $$

   2. $$
      \varliminf_{n \to \infty}a_n + \varliminf_{n \to \infty}b_n \leq \varliminf_{n \to \infty}(a_n + b_n);
      $$

   3. 若$a_n > 0, b_n > 0 \ (n = 0, 1, 2, \cdots)$，则
      $$
      \varliminf_{n \to \infty}a_n \cdot \varliminf_{n \to \infty}b_n \leq \varliminf_{n \to \infty}(a_n \cdot b_n);
      $$

      $$
      \varlimsup_{n \to \infty}a_n \cdot \varlimsup_{n \to \infty}b_n \geq \varlimsup_{n \to \infty}(a_n \cdot b_n);
      $$

   4. 若$a_n > 0, \ \varliminf_{n \to \infty}a_n > 0$，则
      $$
      \varliminf_{n \to \infty}\frac{1}{a_n} = \frac{1}{\varliminf_{n \to \infty}a_n}.
      $$
      

2. 若$\\{a_n\\}$为递增数列，则$\varlimsup_{n \to \infty}a_n = \lim_{n \to \infty}a_n$.

3. 若$a_n > 0 \ (n = 1, 2, \cdots)$ 且$\varlimsup_{n \to \infty}a_n \cdot \varlimsup_{n \to \infty}\frac{1}{a_n} = 1$，则数列$\\{a_n\\}$收敛。

例题

设$\varliminf_{n \to \infty}x_n = A < B = \varlimsup_{n \to \infty}x_n$且$\lim_{n \to \infty}(x_{n + 1} - x_n) = 0$，则$x_n$的聚点全体恰为闭区间$[A, B]$.

提示：采用反证法。