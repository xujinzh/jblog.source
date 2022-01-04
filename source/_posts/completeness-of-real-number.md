---
title: 实数集的完备性
author: Jinzhong Xu
mathjax: true
date: 2020-03-20 23:29:45
tags:
- completeness
- real number
- maths
categories:
- maths
- mathematical analysis
---

在数学分析中，实数集的完备性表现为六个定理，分别是实数集合的确界原理、数列的单调有界定理、区间套定理、数列的致密性定理与聚点定理、柯西收敛准则、有限覆盖定理。  它们称为<font color='dd0000'>实数完备性基本定理</font>。  

<!--more-->

# 区间套定理

**定义 1** &emsp; 设闭区间列$\{[a_n, b_n]\}$具有如下性质：

&emsp; 1. $[a_n, b_n] \supset [a_{n + 1}, b_{n + 1}], \ n = 1, 2, \cdots$;

&emsp; 2. $\lim_{n \to \infty}(b_n - a_n) = 0$,

则称$\{[a_n, b_n]\}$为**闭区间套**，简称为**区间套**。  

**定理 7.1** &emsp; （区间套定理）&emsp; 若$\{[a_n, b_n]\}$是一个区间套，则在实数系中存在唯一的一个点$\xi$，使得$\xi \in [a_n, b_n], \ n = 1, 2, \cdots$，即
$$
a_n \leq \xi \leq b_n, \ n = 1, 2, \cdots.
$$
**推论** &emsp; 若$\xi \in [a_n, b_n], (n = 1, 2, \cdots)$是区间套$\{[a_n, b_n]\}$所确定的点，则对任给的$\varepsilon > 0$，存在$N > 0$，使得当$n > N$时有
$$
[a_n, b_n] \subset U(\xi; \varepsilon).
$$
<font color='dd00dd'>注意：这里的区间套是闭区间</font>。  

# 聚点定理

**定义 2** &emsp; 设$S$为数轴上的点集，$\xi$为定点（它可以属于$S$，也可以不属于$S$）。若$\xi$的任何邻域上都含有$S$中无穷多个点，则称$\xi$为点集$S$的一个聚点。  

另外两个等价定义：

**定义 $2^{\prime}$** &emsp; 对于点集$S$，若点$\xi$的任何$\varepsilon$邻域上都含有$S$中异于$\xi$的点，即$U^o(\xi; \varepsilon) \cap S \neq \varnothing$，则称$\xi$为$S$的一个聚点。

**定义$2^{\prime\prime}$** &emsp; 若存在各项互异的收敛数列$\{ x_n \} \subset S$，则其极限$\lim_{n \to \infty} x_n = \xi$称为$S$的一个聚点。  

**定理 7.2** &emsp; (魏尔斯特拉斯（Weierstrass）聚点定理) &emsp; 实轴上的任一有界无限点集$S$至少有一个聚点。  

# 开覆盖定理

**定义 3** &emsp; 设$S$为数轴上的点集，$H$为开区间的集合（即$H$的每一个元素都是形如$(\alpha, \beta)$的开区间）。若$S$中任何一点都含在$H$中至少一个开区间内，则称$H$为$S$的一个开覆盖，或称$H$覆盖$S$。若$H$中开区间的个数是无限（有限）的，则称$H$为$S$的一个无限开覆盖（有限开覆盖）。  

**定理 7.3** &emsp; (海涅-博雷尔（Heine-Borel）有限覆盖定理) &emsp; 设$H$为闭区间$[a, b]$的一个（无限）开覆盖，则从$H$中可选出有限个开区间来覆盖$[a, b]$.  

<font color='dd0000'>注意：涉及到有限开覆盖定理首先尝试反证法</font>  。  

