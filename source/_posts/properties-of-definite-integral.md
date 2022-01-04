---
title: 定积分的性质
mathjax: true
author: Jinzhong Xu
date: 2020-09-12 10:48:15
tags:
- maths
- integral
categories:
- maths
- mathematical analysis
---

了解定积分的性质可以帮助我们方便计算定积分，提高计算的速度。

<!--more-->

# 定积分的基本性质

**性质1**	若$f$在$[a,b]$上可积，$k$为常数，则$kf$在$[a,b]$上也可积，且
$$
\int^b_a k f(x) \mathrm{d}x = k \int^b_a f(x) \mathrm{d}x.
$$
**性质2**	若$f,g$都在$[a,b]$上可积，则$f\pm g$在$[a,b]$上也可积，且
$$
\int^b_a [f(x) \pm g(x)] \mathrm{d}x = \int^b_a f(x) \mathrm{d}x \pm \int^b_a g(x) \mathrm{d}x.
$$
**线性性质**
$$
\int^b_a [\alpha f(x) + \beta g(x)] \mathrm{d}x = \alpha \int^b_a f(x) \mathrm{d}x + \beta \int^b_a g(x) \mathrm{d}x.
$$
其中$\alpha, \beta$为常数。

**性质3**	若$f,g$都在$[a,b]$上可积，则$f\cdot g$在$[a,b]$上也可积。但一般情形下
$$
\int^b_a f(x)g(x)\mathrm{d}x \neq \int^b_a f(x)\mathrm{d}x \cdot \int^b_a g(x)\mathrm{d}x.
$$
**性质4**	$f$在$[a,b]$上可积的充要条件是任给$c\in (a,b)$，$f$在$[a,c]$与$[c,b]$上都可积。此时又有等式
$$
\int^b_a f(x) \mathrm{d}x = \int^c_a f(x) \mathrm{d}x + \int^b_c f(x) \mathrm{d}x.
$$
性质4称为关于积分区间的可加性。当$f(x)\geq 0$时，其几何意义就是曲边梯形面积的可加性。

**注意**：按定积分的定义，记号 $\int^b_a f(x) \mathrm{d}x$ 只有当 $a<b$ 时才有意义，而当 $a=b$ 或 $a>b$ 时本来是没有意义的。但为了运用上的方便，对它作如下规定：

**规定1**	当$a=b$时，令$\int^a_a f(x) \mathrm{d}x = 0$;

**规定2**	当$a>b$时，令$\int^b_a f(x) \mathrm{d}x = -\int^a_b f(x) \mathrm{d}x$.

有了这些规定以后，性质4对于$a,b,c$的任何大小顺序都能成立。

**性质5**	设$f$为$[a,b]$上的可积函数。若$f(x)\geq 0, x\in[a,b]$，则
$$
\int^b_a f(x) \mathrm{d}x \geq 0.
$$
**推理（积分不等式性）**	若$f$与$g$为$[a,b]$上的两个可积函数，且$f(x)\leq g(x), x\in [a,b]$，则有
$$
\int^b_a f(x) \mathrm{d}x \leq \int^b_a g(x) \mathrm{d}x.
$$
**性质6**	若$f$在$[a,b]$上可积，则$|f|$在$[a,b]$上也可积，且
$$
\left \vert \int^b_a f(x) \mathrm{d} x \right \vert \leq \int^b_a \left \vert f(x) \right \vert \mathrm{d} x.
$$
**注意：这个性质的逆命题一般不成立**，例如
$$
f(x) = 
\begin{cases}
1, x \in Q, \\\\
-1, x \in Q^C
\end{cases}
$$
在$[0,1]$上不可及（类似于狄利克雷函数）；但$|f(x)|\equiv 1$，它在$[0,1]$上可积。

**命题**	假设$f$为一非负可积函数，只要它在某一点$x_0$处连续，且$f(x_0) > 0$，那么必有$\int^b_a f(x) \mathrm{d}x > 0$.

**注意：可积函数必有连续点。**

可积等价于几乎处处连续。

连续具有局部保号性：如果$f$在$[a,b]$上连续，在点$x_0 \in (a,b)$处，$f(x_0)>0$，那么存在一个$\delta > 0$，使得$f(x) > 0, x \in [x_0 - \delta, x_0 + \delta]$.

# 积分中值定理

**定理 9.7 （积分第一中值定理）**	若$f$在$[a,b]$上连续，则至少存在一点$\xi \in [a,b]$，使得
$$
\int^b_a f(x) \mathrm{d} x = f(\xi) (b - a).
$$
**定理 9.8 （推广的积分第一中值定理）**	若$f$与$g$都在$[a,b]$上连续，且$g(x)$在$[a,b]$上不变号，则至少存在一点$\xi \in [a,b]$，使得
$$
\int^b_a f(x)g(x) \mathrm{d}x = f(\xi) \int^b_a g(x) \mathrm{d}x.
$$

# 重要习题

- 若$f$与$g$都在$[a,b]$上可积，则
  $$
  \lim_{\| T \| \to 0} \sum^n_{i=1} f(\xi_i)g(\eta_i) \Delta x_i = \int^b_a f(x)g(x) \mathrm{d}x.
  $$
  其中$\xi_i, \eta_i$是$T$所属小区间$\Delta_i$中的任意两点，$i=1,2,\cdots,n$.

- 设$f$与$g$都在$[a,b]$上可积，则
  $$
  M(x) = \max_{x\in[a,b]} \{f(x), g(x)\}, m(x) = \min_{x\in[a,b]}\{f(x),g(x)\}
  $$
  在$[a,b]$上都可积。

- 设$f$在$[a,b]$上可积，且在$[a,b]$上满足$|f(x)|\geq m > 0$，则$\frac{1}{f}$在$[a,b]$上也可积。

- 若$f$与$g$都在$[a,b]$上可积，且$g(x)$在$[a,b]$上不变号，$M,m$分别为$f(x)$在$[a,b]$上的上、下确界，则比存在某实数$\mu(m\leq \mu \leq M)$，使得
  $$
  \int^b_a f(x)g(x)\mathrm{d}x = \mu \int^b_a g(x) \mathrm{d}x.
  $$
  