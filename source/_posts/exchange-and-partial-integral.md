---
title: 换元积分和分部积分
author: Jinzhong Xu
mathjax: true
date: 2020-03-24 20:30:36
tags:
- integral
- maths
categories:
- maths
- mathematical analysis
---

这节讲求不定积分的方法，包括换元积分法和分部积分法。其中换元积分法是利用复合函数求导法得到，分部积分法是利用乘积求导法得到。

<!--more-->

# 换元积分

**定理 8.4** &ensp; （换元积分法）&ensp; 设函数$f(x)$在区间$I$上有定义，$\varphi(t)$在区间$J$上可导，且$\varphi(J) \subset I$.

1. 如果不定积分$\int f(x) \mathrm{d}x = F(x) + C$在$I$上存在，则不定积分$\int f(\varphi(t)) \varphi^{\prime}(t) \mathrm{d}t$在$J$上也存在，且
   $$
   \int f(\varphi(t)) \varphi^{\prime}(t) \mathrm{d}t = F(\varphi(t)) + C.
   $$

2. 如果$x = \varphi(t)$在$J$上存在反函数$t = \varphi^{-1}(x), \ x \in I$，且不定积分$\int f(x) \mathrm{d}x$在$I$上存在，则当不定积分$\int f(\varphi(t)) \varphi^{\prime}(t) \mathrm{d}t = G(t) + C$在$J$上存在时，在$I$上有
   $$
   \int f(x) \mathrm{d}x = G(\varphi^{-1}(x)) + C.
   $$
   

上述两个条分别反映了正、负两种换元方式，习惯上分别称为第一换元积分法和第二换元积分法，相应的换元公式称为第一换元公式和第二换元公式。

# 分部积分

**定理 8.5** &ensp; （分部积分法）&ensp; 若$u(x)$与$v(x)$可导，不定积分$\int u^{\prime}(x) v(x) \mathrm{d}x$在，则$\int u(x) v^{\prime}(x) \mathrm{d}x$也存在，并有
$$
\int u(x) v^{\prime}(x) \mathrm{d}x = u(x) v(x) - \int u^{\prime}(x) v(x) \mathrm{d}x.
$$
该公式称为分部积分公式，可以简写为$\int u dv = uv - \int v du$.

