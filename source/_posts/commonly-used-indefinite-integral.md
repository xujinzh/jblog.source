---
title: 常用不定积分公式
author: Jinzhong Xu
mathjax: true
date: 2020-07-18 15:00:38
tags:
- antiderivatiive
- indefinite integral
categories:
- maths
- mathematical analysis
---

这里给出一些常用的不定积分公式。

<!--more-->
$$
\int \frac{\mathrm{d}x}{x^2 + a^2} = \frac{1}{a}\arctan\frac{x}{a} + C
$$

$$
\int \frac{\mathrm{d}x}{x^2 - a^2} = \frac{1}{2a}\ln \left |\frac{x-a}{x+a} \right | + C
$$

$$
\int \frac{\mathrm{d}x}{\sqrt{a^2 - x^2}} = \arcsin\frac{x}{a} + C
$$

$$
\int \frac{\mathrm{d}x}{\sqrt{x^2 \pm a^2}} = \ln \left | x + \sqrt{x^2 \pm a^2} \right | + C
$$

$$
\int \ln x \mathrm{d}x = x \ln x - x + C
$$

$$
\int e^{ax} \cos bx \mathrm{d}x = \frac{e^{ax}}{a^2 + b^2} (a\cos bx + b\sin bx) + C
$$

$$
\int e^{ax} \sin bx \mathrm{d}x = \frac{e^{ax}}{a^2 + b^2} (a \sin bx - b \cos bx) + C
$$

$$
\int x \cos nx \mathrm{d}x = \frac{1}{n^2} \cos nx + \frac{x}{n} \sin nx + C
$$

$$
\int x \sin nx \mathrm{d}x = \frac{1}{n^2} \sin nx - \frac{x}{n} \cos nx + C
$$

$$
\int \sec x \mathrm{d}x = \ln \left | \sec x + \tan x \right | + C
$$

$$
I(m, n) = \int \cos^m x \sin^n x \mathrm{d}x = 
\bigtriangleup  + \frac{m - 1}{m + n} I(m-2, n) \\\\
= \square + \frac{n - 1}{m + n} I(m, n-2)
$$

