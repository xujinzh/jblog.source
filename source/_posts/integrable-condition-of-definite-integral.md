---
title: 定积分的可积条件
author: Jinzhong Xu
date: 2020-08-29 09:57:10
mathjax: true
tags:
- maths
- integral
- definite integral
categories:
- maths
- mathematical analysis
---

定积分的可积条件就是研究函数满足什么条件下可进行积分（充分性），函数可积分会有什么性质（必要性），以及可积函数的充分必要条件等。

**关于函数$f(x)$在闭区间$[a,b]$上的积分总结一句话就是：连续必可积，可积必有界。**

<!--more-->

# 可积的必要条件

在介绍可积的必要条件之前，首先引入有界（无界）函数$f(x)$的定义。

## 有界函数和无界函数

定义	设函数$f(x)$的定义域为$D$，如果存在一个常数$M(L)$，使得$\forall x \in D$，都有
$$
f(x)\leq M (f(x)\geq L).
$$
则称$f(x)$在$D$内有上（下）界的函数，数$M(L)$称为$f(x)$在$D$内的一个上（下）界。

定义	设函数$f(x), x\in D$，如果存在一个正数$K>0$，使得$\forall x \in D$，都有
$$
\left \vert f(x) \right \vert \leq K,
$$
那么称$f(x)$在$D$内是有界函数；否则，称$f(x)$是无界函数。

*如果 $f(x)$ 在 $D$ 内既有上界又有下界，则称$f(x)$在$D$内是有界函数*

*函数$f(x)$在$D$内有界当且仅当数集$f(D)$是有界集，即$\forall f(x) \in f(D), L \leq f(x) \leq M$, 其中$M,L$是常数，分别称为$f(D)$的一个上界和一个下界.*

定义	函数$f(x), x \in D$是无界函数当且仅当$\forall M > 0, \exists x \in D$，使得$\left \vert f(x) \right \vert > M$. 

有了有界和无界函数的定义，我们就可以来研究函数可积的必要条件了。

## 可积的必要条件

定理 9.2	若函数$f(x)$在$[a,b]$上可积，则$f(x)$在$[a,b]$上必定有界。

证明	用反证法。若$f(x)$在$[a,b]$上无界，则对于$[a,b]$的任一分割$T$，必存在属于$T$的某个小区间$\Delta_k$，$f(x)$在$\Delta_k$上无界。在$i \neq k$的各个小区间$\Delta_i$上任意取定$\xi_i$，并记
$$
G = \left \vert \sum_{i \neq k} f(\xi_i) \Delta x_i \right \vert.
$$
现对任意大的正数$M$，由于$f(x)$在$\Delta_k$上无界，故存在$\xi_k \in \Delta_k$，使得
$$
\left \vert f(\xi_k) \right \vert > \frac{M + G}{\Delta x_k}.
$$
于是有
$$
\left \vert \sum^n_{i=1} f(\xi_i) \Delta x_i \right \vert \geq \left \vert f(\xi_k) \Delta x_k \right \vert - \left \vert \sum_{i \neq k} f(\xi_i) \Delta x_i \right \vert > \frac{M + G}{\Delta x_k} \cdot \Delta x_k - G = M.
$$
由此可见，对于无论多小的$\|T\|$，按上述方法选取点集$\{\xi_i\}$时，总能使积分和的绝对值大于任何预先给出的正数，这与$f(x)$在$[a,b]$上可积相矛盾。

**该定理指出，任何可积函数一定是有界函数，但是，反过来不一定成立。**

例如，如下的狄利克雷函数
$$
D(x) = 
\begin{cases} 
  1, x \in Q \\\\  
  0, x \in Q^C.
\end{cases}
$$
显然该函数是有界的，因为 $\left \vert D(x) \right \vert \leq 1, x \in [0, 1]$ . 但是，根据定积分的定义，无论分割多么细密，都可以只选取有理点进行积分或者只选取无理点进行积分，当选取有理点积分时，积分值等于1，当选取无理点积分时，积分值等于0.因此不可积。

**因此，有界是可积的必要条件。所以，当以后假设函数是可积函数时，默认指明函数是有界函数。**

# 可积的充要条件

要判断一个函数是否可积不能依靠必要条件（有界），当然，仅仅使用充分条件（连续）又有些太强，毕竟不连续函数比连续函数多得多，就好比无理数比有理数多一样。但是，根据定积分的定义来判断，又相对比较麻烦，所以，需要找到一个新的方法来判断一个函数到底是可积还是不可积。现假设函数$f(x)$在$[a,b]$上可积（必有界），来找到其充分必要条件。

设$T=\{\Delta_i | i = 1, 2, \cdots, n\}$ 为对$[a, b]$ 的任一分割。由于$f(x)$在$[a,b]$上有界，它在每个$\Delta_i$上存在上、下确界：
$$
M_i = \sup_{x \in \Delta_i} f(x), m_i = \inf_{x \in \Delta_i} f(x), i = 1, 2, \cdots, n.
$$
作和
$$
S(T) = \sum^n_{i=1} M_i \Delta x_i, s(T) = \sum^n_{i=1} m_i \Delta x_i,
$$
分别称为$f$ 关于分割$T$ 的上和与下和（或称为达布上和与达布下和，统称达布和）。

任给$\xi_i \in \Delta_i, i = 1, 2, \cdots, n$，显然有
$$
s(T) \leq \sum^n_{i=1}f(\xi_i) \Delta x_i \leq S(T).
$$
与积分和相比较，达布和只与分割$T$有关，而与点集$\{\xi_i\}$ 无关，根据上面不等式，就能通过讨论上和与下和当 $\| T \| \to 0$时的极限来揭示$f$在$[a,b]$上是否可积。

定理9.3	（可积准则）	函数$f$在$[a,b]$上可积的充要条件是：任给$\varepsilon > 0$，总存在相应的一个分割$T$，使得
$$
S(T) - s(T) < \varepsilon.
$$
设 $\omega_i = M_i - m_i$，称为$f$在$\Delta_i$ 上的振幅，有必要时也记为 $\omega^f_i$，由于
$$
S(T) - s(T) = \sum^n_{i=1} \omega_i \Delta x_i=\sum_T \omega_i \Delta x_i.
$$
定理可重述为：

定理9.4	函数$f$在$[a,b]$上可积的充要条件是任给$\varepsilon > 0$，总存在相应的某一分割$T$，使得
$$
\sum_T \omega_i \Delta x_i < \varepsilon.
$$
几何意义：若$f$在$[a,b]$上可积，则达布上和与达布下和在图像上所围成的一系列小矩形面积之和可以达到任意小，只要分割充分地细；反之亦然。

# 可积函数类（可积的充分条件）

定理9.4	若$f$为$[a,b]$上的连续函数，则$f$在$[a,b]$上可积。

定理9.5	若$f$是区间$[a,b]$上只有有限个间断点的有界函数，则$f$在$[a,b]$上可积。

定理9.6	若$f$是区间$[a,b]$上的单调函数，则$f$在$[a,b]$上可积。

例题	黎曼函数
$$
f(x) = 
\begin{cases}
\frac{1}{q}, x=\frac{p}{q}, p, q 互素, q > p, \\\\
0, x = 0, 1 以及 (0, 1) 内的无理数
\end{cases}
$$
在区间$[0,1]$上可积，且
$$
\int^1_0 f(x) \mathrm{d}x = 0.
$$
例题	若$T^{\prime}$是$T$增加若干个分点后所得的分割，则
$$
\sum_{T^\prime} \omega^{\prime}_i \Delta x^{\prime}_i \leq \sum_T \omega_i \Delta x_i.
$$
例题	若$f$在$[a,b]$上可积，$[\alpha, \beta] \subset [a,b]$，则$f$在$[\alpha, \beta]$上也可积。

例题	设$f,g$均为定义在$[a,b]$上的有界函数，仅在有限个点处$f(x) \neq g(x)$，则 $f,g$ 均在$[a,b]$上可积，且$\int^b_a f(x) \mathrm{d}x = \int^b_a g(x) \mathrm{d}x.$

例题	设$f$在$[a,b]$上有界，$\{a_n\} \subset [a,b], \lim_{n \to \infty} a_n = c$，若$f$ 在$[a,b]$上只有$a_n(n=1,2,\cdots)$为其间断点，则$f$在$[a,b]$上可积。

例题	设函数$f$在$[a,b]$上有定义，且对于任给的$\varepsilon > 0$，存在$[a,b]$上的可积函数$g$，使得
$$
|f(x) - g(x)| < \varepsilon, x \in [a, b].
$$
则$f$在$[a,b]$上可积。