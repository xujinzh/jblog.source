---
title: 平面曲线的弧长和曲率
mathjax: true
author: Jinzhong Xu
date: 2021-01-27 17:21:47
tags:
- definite integral
categories:
- maths
- mathematical analysis
---

通过定积分我们能够计算平面曲线的弧长和曲率。对于平面曲线的画出，通过利用参数方程和对曲线划分以直代曲的定积分思想来计算弧长。

<!--more-->

# 平面曲线的弧长

设$C = \stackrel\frown{AB}$ 是一条没有**自交点**的非闭的平面曲线。在$C$上从$A$到$B$依次取分点：
$$
A=P_0, P_1, P_2, \cdots, P_{n-1}, P_n = B,
$$
它们成为对曲线$C$的一个分割，记为$T$。然后用线段联结$T$中每个相邻的两点，得到$C$的$n$条弦$\overline{P_{i-1}P_i} (i=1,2,\cdots,n)$，这$n$条弦又成为$C$的一条内接折线。记
$$
\|T\| = \max_{1\leq i \leq n} |P_{i-1}P_i|, s_T = \sum^n_{i=1}|P_{i-1}P_i|,
$$
分别表示最长弦的长度和折线的总长度。

**定义 1**	如果存在有限极限
$$
\lim_{\|T\|\to 0}s_T = s,
$$
即任给$\varepsilon > 0$，恒存在$\delta > 0$，使得$C$的任何分割$T$，只要$\|T\| < \delta$，就有
$$
|s_T - s| < \varepsilon,
$$
则称曲线$C$是可求长的，并把极限$s$定义为曲线$C$的弧长。

**定理 1**	设曲线$C$是一条没有自交点的非闭的平面曲线，由参数方程
$$
x=x(t), y=y(t), t\in [\alpha, \beta]
$$
给出。若$x(t)$与$y(t)$在$[\alpha, \beta]$上连续可微，则$C$是可求长的，且弧长为
$$
s = \int^{\beta}_{\alpha} \sqrt{[x^{\prime}(t)]^2 + [y^{\prime}(t)]^2} \mathrm{d}t.
$$
对于光滑曲线，上面公式仍然成立。

## 直角坐标方程定义的曲线

假设曲线$C$由直角坐标方程
$$
y=f(x), x\in [a, b]
$$
表示，把它看作参数方程时，即为
$$
x=x, y=f(x),x\in[a,b].
$$
当$f(x)$在$[a,b]$上连续可微时，此曲线即为一光滑曲线。弧长公式为
$$
s = \int^b_a \sqrt{1 + {f^{\prime}}^2(x)} \mathrm{d}x.
$$

## 极坐标方程定义的曲线

若曲线$C$由极坐标方程
$$
r = r(\theta), \theta \in [\alpha, \beta]
$$
表示，把它化为参数方程，则为
$$
x = r(\theta)\cos \theta, y= r(\theta)\sin\theta, \theta\in[\alpha, \beta].
$$
由于
$$
x^{\prime}(\theta)=r^{\prime}(\theta)\cos\theta - r(\theta)\sin\theta, \\\\
y^{\prime}(\theta)=r^{\prime}(\theta)\sin\theta + r(\theta)\cos\theta, \\\\
{x^{\prime}}^2(\theta) + {y^{\prime}}^2(\theta)=r^2(\theta)+{r^{\prime}}^2(\theta),
$$
因此当$r^{\prime}(\theta)$在$[\alpha,\beta]$上连续，且$r(\theta)$与$r^{\prime}(\theta)$不同时为零时，此极坐标曲线为一光滑曲线。这时弧长公式为
$$
s = \int^{\beta}_{\alpha}\sqrt{r^2(\theta) + {r^{\prime}}^2(\theta)}\mathrm{d}\theta.
$$


