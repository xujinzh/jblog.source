---
title: 由平行截面面积求体积
mathjax: true
author: Jinzhong Xu
date: 2020-10-17 13:45:21
tags:
- maths
- volume
- definite integral
categories:
- maths
- mathematical analysis
---

本节讲通过定积分求解三维空间中立体的体积。

<!--more-->

假设 $\Omega$ 为三维空间中的一个立体，它夹在垂直于 $x$ 轴的两平面 $x = a$ 与 $x = b$ 之间，这里 $a <b$. 若在任意一点 $x \in [a, b]$ 处作垂直于 $x$ 轴的平面，它截得 $\Omega$ 的截面面积显然是 $x$ 的函数，记为 $A(x), x \in [a, b]$，并称之为 $\Omega$ 的截面面积函数。假设截面面积函数 $A(x)$ 是 $[a, b]$ 上的一个连续函数，且把 $\Omega$ 的上述平行截面投影到某一垂直于 $x$ 轴的平面上，它们永远是一个含在另一个里面。对 $[a, b]$ 作分割
$$
T: a = x_0 < x_1 < \cdots < x_n = b.
$$
过各个分点作垂直于 $x$ 轴的平面 $x = x_i, i = 1, 2, \cdots, n$，它们把 $\Omega$ 切割成 $n$ 个薄片 $\Omega_i, i = 1, 2, \cdots, n$. 任取 $\xi_i \in [x_{i-1}, x_i]$，那么每一个薄片的体积
$$
\Delta V_i \approx A(\xi_i) \Delta x_i.
$$
于是
$$
V \approx \sum_{i=1}^n A(\xi_i)\Delta x_i.
$$
由定积分的定义和连续函数的可积性，当 $\|T\|\to 0$时，上式右边的极限存在，即为函数 $A(x)$ 在 $[a, b]$ 上的定积分。于是我们定义立体 $\Omega$ 的体积为
$$
V = \int_a ^b A(x) \mathrm{d}x.
$$
假设 $\Omega_A, \Omega_B$ 为位于同一区间 $[a, b]$ 上的两个立体，其体积分别为 $V_A, V_B$，若在 $[a, b]$ 上它们的截面面积函数 $A(x)$ 与 $B(x)$ 皆连续，且 $A(x) = B(x)$，则$V_A = V_B$. 该定理在我国齐梁时期的数学家祖暅（祖冲之之子）早发现，在《九章算术》一书中记载的祖暅原理是：“夫叠棊成立积，缘幂势既同则积不容异”。

对于旋转体，体积计算公式定义如下。

假设 $f$ 是 $[a, b]$ 上的连续函数，$\Omega$ 是由平面图形
$$
0 \leq |y| \leq |f(x)|, a \leq x \leq b
$$
绕 $x$ 轴旋转一周所得的旋转体。那么易知截面面积函数为
$$
A(x) = \pi [f(x)]^2, x \in [a, b].
$$
得旋转体 $\Omega$ 的体积公式为
$$
V = \pi \int_a ^b [f(x)]^2 \mathrm{d}x.
$$
