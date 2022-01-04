---
title: 牛顿-莱布尼茨公式
author: Jinzhong Xu
mathjax: true
date: 2020-08-15 14:12:08
tags:
- maths
- Newton
- Leibniz
categories:
- maths
- mathematical analysis
---

牛顿-莱布尼茨公式不仅为定积分提供了一个有效的计算方法，而且是联系不定积分和定积分的桥梁。

<!--more-->

**定理 9.1**	若函数 $f$ 在 $[a, b]$ 上连续，且存在原函数 $F$，即 $F^{\prime}(x) = f(x), x\in[a, b]$，则 $f$ 在 $[a, b]$ 上可积，且
$$
\int^b_a f(x) \mathrm{d}x = F(b) - F(a).
$$
上式称为牛顿-莱布尼茨公式，也常写作
$$
\left. \int_a^b f(x) \mathrm{d}x = F(x) \right\vert_a^b.
$$
证明（拉格朗日中值定理和定积分定义）由定积分定义，任给 $\varepsilon > 0$，需要证明存在 $\delta > 0$，当 $\left \| T \right \| < \delta$ 时，有
$$
\left\vert \sum_{i=1}^n f(\xi_i) \Delta x_i - [F(b) - F(a)] \right\vert < \varepsilon.
$$
事实上，对于 $[a, b]$ 的任一分割 $T = \{a = x_0, x_1, \cdots, x_n = b\}$，在每一个小区间 $[x_{i-1}, x_i]$ 上对 $F(x)$ 使用拉格朗日中值定理，则分别存在 $\eta_i \in (x_{i-1}, x_i), i = 1, 2, \cdots, n$，使得
$$
F(b) - F(a) = \sum_{i=1}^n [F(x_i) - F(x_{i-1})] \\\\
= \sum_{i=1}^n F^{\prime}(\eta_i) \Delta x_i = \sum_{i=1}^n f(\eta_i)\Delta x_i.
$$
因为 $f$ 在 $[a, b]$ 上连续，从而一致连续，所以对上述 $\varepsilon > 0$，存在 $\delta > 0$，当 $x^{\prime}, x^{\prime\prime} \in [a, b]$ 且 $\left\vert x^{\prime} - x^{\prime\prime} \right\vert < \delta$ 时，有
$$
\left\vert f(x^{\prime}) - f(x^{\prime\prime}) \right\vert < \frac{\varepsilon}{b-a}.
$$
于是，当 $\Delta x_i \leq \left\|T\right\| < \delta$ 时，任取 $\xi_i \in [x_{i-1}, x_i]$，便有 $\left\vert \xi_i - \eta_i \right\vert < \delta$，这就证明
$$
\left\vert \sum_{i=1}^n f(\xi_i)\Delta x_i - [F(b) - F(a)] \right\vert \\\\
= \left\vert \sum_{i=1}^n [f(\xi_i) - f(\eta_i)]\Delta x_i \right\vert \\\\
\leq \sum_{i=1}^n \left\vert f(\xi_i) - f(\eta_i) \right\vert \Delta x_i \\\\
< \frac{\varepsilon}{b-a} \cdot \sum_{i=1}^n \Delta x_i = \varepsilon.
$$
所以，$f$ 在 $[a,b]$ 上可积，且有牛顿-莱布尼茨公式成立。

*注意* 定理的条件可适当减弱，

1. 对 $F$ 的要求可减弱为：在 $[a,b]$ 上连续，在 $(a,b)$ 上可导，且 $F^{\prime}(x)=f(x), x\in(a,b)$
2. 对 $f$ 的要求可减弱为：在 $[a, b]$ 上可积
3. 其实，连续函数必有原函数，因此，定理中“且存在原函数”要求可以省略

例题 

证明：若 $f$ 在 $[a,b]$ 上可积，$F$ 在 $[a,b]$ 上连续，且除有限个点外有 $F^{\prime}=f(x)$，则
$$
\int_a^b f(x) \mathrm{d}x=F(b)-F(a).
$$
