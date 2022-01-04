---
title: 微积分基本定理和定积分计算
mathjax: true
author: Jinzhong Xu
date: 2020-09-19 09:44:17
tags:
- maths
- definite integral
- integral
categories:
- maths
- mathematical analysis
---

首先给出一个结论：连续函数必定存在原函数。

<!--more-->

# 变限积分与原函数的存在性

假设$f(x)$在$[a,b]$上可积，对于任何的$x\in[a,b]$，$f$在$[a,x]$上也可积，定义
$$
\Phi(x) = \int^x_a f(t) \mathrm{d}t, x\in [a, b]
$$
为以积分上限$x$为自变量的函数，称为变上限的定积分。同样，定义变下限的定积分为
$$
\Psi(x) = \int^b_x f(t) \mathrm{d}t, x\in[a,b].
$$
统称$\Psi$与$\Phi$为变限积分。由于$\int^b_x f(t) \mathrm{d}t = -\int^x_b f(t)\mathrm{d}t$，因此下面只讨论变上限积分。

**定理 9.9**	若$f$在$[a,b]$上可积，则$\Phi$在$[a,b]$上连续。

**定理 9.10**	（原函数存在定理）若$f$在$[a,b]$上连续，则$\Phi$在$[a,b]$上处处可导，且
$$
\Phi^{\prime}(x) = \frac{\mathrm{d}}{\mathrm{d}x}\int^x_a f(t)\mathrm{d}t = f(x), x \in [a, b].
$$
**定理 9.11**	（积分第二中值定理）设函数$f$在$[a,b]$上可积，

- 若函数$g$在$[a,b]$上减，且$g(x)\geq 0$，则存在$\xi \in [a,b]$，使得

$$
\int^b_a f(x)g(x) \mathrm{d}x = g(a) \int^{\xi}_a f(x) \mathrm{d}x.
$$
- 若函数$g$在$[a,b]$上增，且$g(x)\geq 0$，则存在$\eta \in [a,b]$，使得

$$
\int^b_a f(x) g(x) \mathrm{d}x = g(b) \int^b_{\eta} f(x) \mathrm{d}x.
$$
- 推广：若函数$g$在$[a,b]$上单调，则存在$\xi \in [a,b]$使得

$$
\int_{a}^{b} f(x)g(x) \mathrm{d}x = g(a) \int_{a}^{\xi} f(x) \mathrm{d}x + g(b) \int_{\xi}^{b} f(x) \mathrm{d}x.
$$

# 换元积分法与分部积分法 	

**定理 9.12**	（定积分换元积分法）若函数$f$在$[a,b]$上连续，$\varphi^{\prime}$在$[a,b]$上可积，且满足
$$
\varphi(\alpha)=a, \varphi(\beta)=b, \varphi([\alpha, \beta]) \subset [a,b],
$$
则有定积分换元公式：
$$
\int^b_a f(x) \mathrm{d}x = \int^{\beta}_{\alpha}f(\varphi(t))\varphi^{\prime}(t) \mathrm{d}t.
$$
**定理 9.13**	（定积分分部积分法）若$u(x),v(x)$为$[a,b]$上的可微函数，且$u^{\prime}(x)$和$v^{\prime}(x)$都在$[a,b]$上可积，则有定积分分部积分公式：
$$
\int^b_a u(x)v^{\prime}(x) \mathrm{d}x = u(x)v(x)\vert^b_a - \int^b_a u^{\prime}(x)v(x)\mathrm{d}x.
$$

# 泰勒公式的积分型余项

若在$[a,b]$上$u(x),v(x)$有$n+1$阶连续导函数，则有
$$
\int^b_a u(x)v^{(n+1)}(x)\mathrm{d}x = [u(x)v^{(n)}(x) - u^{\prime}v^{(n-1)}(x) + \cdots + \\\\
(-1)^n u^{(n)}(x)v(x)]^b_a + (-1)^{n+1}\int^b_a u^{(n+1)}(x) v(x) \mathrm{d}x \\\\
(n = 1, 2, \cdots)
$$
假设函数$f$在点$x_0$的某领域$U(x_0)$上有$n+1$阶连续导函数，令$x\in U(x_0)$，$u(t) = (x-t)^n, v(t) = f(t), t\in [x_0, x]$ （或$[x, x_0]$），
$$
\int^x_{x_0} (x-t)^n f^{(n+1)}(t) \mathrm{d}t = [(x-t)^n f^{(n)}(t) + n(x-t)^{n-1}f^{(n-1)}(t) + \cdots + \\\\
n! f(t)]^x_{x_0} + \int^x_{x_0} 0 \cdot f(t) \mathrm{d}t \\\\
= n! f(x) - n! [f(x_0) + f^{\prime}(x_0)(x-x_0) + \cdots + \frac{f^{(n)}(x_0)}{n!}(x-x_0)^n] \\\\
= n! R_n(x)
$$
其中$R_n(x)$即为泰勒公式的$n$阶余项。由此求得
$$
R_n(x) = \frac{1}{n!} \int^x_{x_0} f^{(n+1)}(t)(x-t)^n \mathrm{d}t,
$$
这就是泰勒公式的积分型余项。

由于$f^{(n+1)}(t)$连续，$(x-t)^n$在$[x_0, x]$（或$[x, x_0]$）上保持同号，因此由推广的积分第一中值定理，可得
$$
R_n(x) = \frac{1}{n!} f^{(n+1)}(\xi) \int^x_{x_0} (x-t)^n \mathrm{d}t \\\\
= \frac{1}{(n+1)!}f^{(n+1)}(\xi)(x-x_0)^{n+1},
$$
其中$\xi = x_0 + \theta(x-x_0), 0\leq \theta \leq 1$，得到得结果就是拉格朗日型余项。

如果直接运用积分第一中值定理，那么
$$
R_n(x) = \frac{1}{n!}f^{(n+1)}(\xi)(x-\xi)^n(x-x_0), \\\\
\xi = x_0 + \theta(x-x_0), 0 \leq \theta \leq 1.
$$
由于
$$
(x-\xi)^n (x-x_0) = [x-x_0 - \theta(x-x_0)]^n(x-x_0) \\\\
=(1-\theta)^n(x-x_0)^{n+1},
$$
因此
$$
R_n(x) = \frac{1}{n!}f^{(n+1)}(x_0 + \theta(x-x_0))(1-\theta)^n(x-x_0)^{n+1}, \\\\
0 \leq \theta \leq 1.
$$
特别地，当$x_0 = 0$时，又有
$$
R_n(x) = \frac{1}{n!}f^{(n+1)}(\theta x)(1 - \theta)^n x^{n+1}, 0\leq \theta \leq 1.
$$
称为泰勒公式得柯西型余项。

# 三角函数积分技巧

- $\int_{0}^{\frac{\pi}{2}} f(\sin x, \cos x)\mathrm{d}x = \int_{0}^{\frac{\pi}{2}} f(\cos x, \sin x)\mathrm{d}x.$ 

- $I(m,n) = \int^{\frac{\pi}{2}}_0 \cos^m x \sin^n x \mathrm{d}x$，$m,n$为正整数，则

$$
I(m,n) = \frac{m-1}{m+n}I(m-2,n) = \frac{n-1}{n+m}I(m,n-2), m,n=2,3,\cdots.
$$

$$
I(m,n) = 
\begin{cases}
\frac{(m-1)!! \cdot (n-1)!!}{(m+n)!!} \frac{\pi}{2}, m,n 都是偶数;\\\\
\frac{(m-1)!! \cdot (n-1)!!}{(m+n)!!}, 其他.
\end{cases}
$$

- $\int^{\frac{\pi}{2}}_0 \sin^n x \mathrm{d}x = \int^{\frac{\pi}{2}}_0 \cos^n x \mathrm{d}x = I(n, 0)$

$$
I(n, 0) = 
\begin{cases}
\frac{(n-1)!!}{n!!}\frac{\pi}{2}, n 是偶数;\\\\
\frac{(n-1)!!}{n!!}.
\end{cases}
$$
