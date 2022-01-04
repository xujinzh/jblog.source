---
title: 可积性理论补叙
mathjax: true
author: Jinzhong Xu
date: 2020-09-27 10:34:39
tags:
- maths
- integral
- definite integral
categories:
- maths
- mathematical analysis
---

在前面，介绍了上和$S(T)$和下和$s(T)$，即对于分割$T:a=x_0 < x_1 < \cdots < x_n = b$，以及$\Delta_i = [x_{i-1}, x_i], \Delta x_i = x_i - x_{i-1}$，有
$$
S(T) = \sum_{i=1}^n M_i \Delta x_i
$$
$$
s(T) = \sum_{i=1}^n m_i \Delta x_i
$$



<!--more-->

其中$M_i = \sup_{x \in \Delta_i} f(x), m_i = \inf_{x \in \Delta_i} f(x), i = 1, 2, \cdots, n$. 因为可积必有界，假设$f$在$[a, b]$上有界，因此，$M_i, m_i$分别有上、下确界$M, m$，而且对于任何$\xi_i \in \Delta_i$，有
$$
m(b - a) \leq s(T) \leq \sum_{i=1}^n f(\xi_i) \Delta x_i \leq S(T) \leq M(b - a).
$$

# 上和与下和的性质

**性质 1**	对同一个分割$T$，相对于任何点集$\{\xi_i\}$而言，上和是所有积分和的上确界，下和是所有积分和的下确界，即
$$
S(T) = \sup_{|\xi_i|} \sum_{i=1}^n f(\xi_i) \Delta x_i, \\\\
s(T) = \inf_{|\xi_i|} \sum_{i=1}^n f(\xi_i) \Delta x_i.
$$
**性质 2**	设$T^{\prime}$为分割$T$添加$p$个新分点后所得到的分割，则有
$$
S(T) \geq S(T^{\prime}) \geq S(T) - (M - m)p \|T\|, \\\\
s(T) \leq s(T^{\prime}) \leq s(T) + (M - m)p \|T\|.
$$
增加分点后，上和不增，下和不减。

**性质 3**	若$T^{\prime}$与$T^{\prime\prime}$为任意两个分割，$T = T^{\prime} + T^{\prime\prime}$ 表示把$T^{\prime}$与$T^{\prime\prime}$的所有分点合并而得到的分割（注意：重复的分点只取一次），则
$$
S(T) \leq S(T^{\prime})， s(T) \geq s(T^{\prime}), \\\\
S(T) \leq S(T^{\prime\prime}), s(T) \geq s(T^{\prime\prime}).
$$
**性质 4**	对任意两个分割$T^{\prime}$与$T^{\prime\prime}$，总有
$$
s(T^{\prime}) \leq s(T) \leq S(T) \leq S(T^{\prime\prime}).
$$
**性质 5**	$m(b - a) \leq s \leq S \leq M(b - a).$

**性质 6**	（达布定理）上、下积分也是上和与下和在$\|T\| \to 0$时的极限，即
$$
\lim_{\|T\| \to 0} S(T) = S, \lim_{\|T\| \to 0} s(T) = s.
$$

# 可积的充要条件

**定理 9.14**	（可积的第一充要条件）函数$f$在$[a, b]$上可积的充要条件是$f$在$[a, b]$上的上积分与下积分相等，即
$$
S = s.
$$
**定理 9.15**	（可积的第二充要条件）函数$f$在$[a, b]$上可积的充要条件是任给$\varepsilon > 0$，总存在某一分割$T$，使得
$$
S(T) - s(T) < \varepsilon,
$$
即
$$
\sum_{i=1}^n \omega_i \Delta x_i < \varepsilon.
$$
**定理 9.16**	（可积的第三充要条件）函数$f$在$[a, b]$上可积的充要条件是任给正数$\varepsilon, \eta$，总存在某一分割$T$，使得属于$T$的所有小区间中，对应于振幅$\omega_{k^{\prime}} \geq \varepsilon$的那些小区间$\Delta_{k^{\prime}}$的总长$\sum_{k^{\prime}} \Delta x_{k^{\prime}} < \eta.$

