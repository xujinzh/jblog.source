---
title: 利用定积分求平面图形的面积
mathjax: true
author: Jinzhong Xu
date: 2020-10-10 13:59:32
tags:
- maths
- integral
- definite integral
categories:
- maths
- mathematical analysis
---

由定积分的几何意义我们知道，连续曲线 $y = f(x) (\geq 0)$ 在区间 $[a, b]$ 上与 $x$ 轴所围曲边梯形的面积为
$$
A = \int_a^b f(x) \mathrm{d}x = \int_a^b y \mathrm{d}x.
$$
<!--more-->

# 负面积

如果 $f(x)$ 在 $[a, b]$ 上不都是非负的，则所围图形的面积为
$$
A = \int_a^b |f(x)| \mathrm{d}x = \int_a^b |y| \mathrm{d}x.
$$
一般地，由上、下两条连续曲线 $y = f_2(x)$ 与 $y = f_1(x)$ 在区间 $[a, b]$ 上所围的平面图形面积计算公式为
$$
A = \int_a^b [f_2(x) - f_1(x)] \mathrm{d}x.
$$
同样 $x = g_2(y)$ 和 $x = g_1(y)$ 在区间 $[\alpha, \beta]$ 上所围面积，计算公式如下
$$
A = \int_{\alpha}^{\beta} [g_2(y) - g_1(y)] \mathrm{d}y.
$$

# 参数方程

设曲线 $C$ 由参数方程
$$
x = x(t), y = y(t), t \in [\alpha, \beta]
$$
给出，在 $[\alpha, \beta]$ 上 $y(t)$ 连续， $x(t)$连续可微且 $x^{\prime}(t) \neq 0$（对于 $y(t)$ 连续可微且 $y^{\prime}(t) \neq 0$ 的情形可类似地讨论）.记 $a = x(\alpha), b = x(\beta) (a < b 或者 b < a)$，则曲线 $C$ 及直线 $x = a, x = b$ 和 $x$ 轴所围的图形，其面积计算公式为
$$
A = \int_{\alpha}^{\beta} |y(t)x^{\prime}(t)| \mathrm{d}t.
$$
求图形面积最好画出图形，根据对称性约简计算过程。

如果参数方程所表示的曲线是封闭的，即
$$
x(\alpha) = x(\beta), y(\alpha) = y(\beta)
$$
且在 $(\alpha, \beta)$ 上曲线自身不再相交，那么曲线自身所围图形的面积为
$$
A = \left \vert \int_{\alpha}^{\beta} y(t) x^{\prime}(t) \mathrm{d}t \right \vert ,
$$
或
$$
A = \left \vert \int_{\alpha}^{\beta} x(t) y^{\prime}(t) \mathrm{d}t \right \vert .
$$

# 极坐标方程

设曲线 $C$ 由极坐标方程
$$
r = r(\theta), \theta \in [\alpha, \beta]
$$
给出，其中 $r(\theta)$ 在 $[\alpha, \beta]$ 上连续，$\beta - \alpha \leq 2 \pi$. 由曲线 $C$ 与两条射线 $\theta = \alpha, \theta = \beta$ 所围成的平面图形，通常也称为扇形，此扇形的面积计算公式为
$$
A = \frac{1}{2} \int_{\alpha}^{\beta} r^2 (\theta) \mathrm{d} \theta.
$$
圆扇形的弧长是 $r \cdot \theta$

圆扇形的面积是 $\frac{1}{2} r^2 \cdot \theta$

圆周长是 $2\pi r$

圆面积是 $\pi r^2$

面积求导得到弧长，弧长求导得到弧度。

