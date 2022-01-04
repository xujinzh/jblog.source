---
title: 方向导数和梯度
author: Jinzhong Xu
mathjax: true
date: 2020-07-26 17:54:46
tags:
- gradient
- maths
categories:
- maths
- mathematical analysis
---

方向导数 $D_{\boldsymbol{l}}f(\boldsymbol{x})$ 一般是对多元函数而言的，表示函数  $f(\boldsymbol{x})$ 沿着某一方向 $\boldsymbol{l}$ 在点 $\boldsymbol{x}$ 的导数。方向导数的大小表示函数沿着该方向 $\boldsymbol{l}$ 在点 $\boldsymbol{x}$ 的函数值的变化率。对于多元函数有无穷多个方向，但只有一个方向是使得函数在点 $\boldsymbol{x}$ 的变化率（增长率）最大，那就是梯度方向 $\nabla f$ （其中，$\nabla$ 表示 nabla symbol），有时也用记号 grad $f(\boldsymbol{x})$ 表示函数 $f(\boldsymbol{x})$ 在点 $\boldsymbol{x}$ 的梯度（gradient）向量。那为什么变化率最大的方向是梯度方向呢？下面给出解释，不是一般性，假设 $\boldsymbol{x} = (x, y)$ ，记这里假设是 $f(\boldsymbol{x})$ 二元函数。

<!--more-->

# 方向导数



方向导数是一元函数导数概念推广到多元函数，在一元函数 $f(x)$ 中，导数 $f^{\prime}(x)$ 定义为
$$
f^{\prime}(x) = \lim_{\Delta x \to 0}\frac{f(x + \Delta x) - f(x)}{\Delta x }
$$
方向导数 $D_{\boldsymbol{l}}f(\boldsymbol{x})$ 定义为
$$
D_{\boldsymbol{l}}f(\boldsymbol{x}) = \lim_{\left |\boldsymbol{l} \right | \to 0}\frac{f(\boldsymbol{x} + \boldsymbol{l}) - f(\boldsymbol{x})}{\left | \boldsymbol{l} \right |}
$$
方向 $\boldsymbol{l}$ 表示与从 $\boldsymbol{x} = (x, y)$ 到 $\boldsymbol{x} + \Delta \boldsymbol{x} = (x + \Delta x, y + \Delta y)$ 的向量 $\Delta \boldsymbol{x} = (\Delta x, \Delta y)$ 同向的向量，因此，可以把定义式写成
$$
D_{\boldsymbol{l}}f(\boldsymbol{x}) = \lim_{\left | \Delta \boldsymbol{x} \right | \to 0} \frac{f(\boldsymbol{x} + \Delta \boldsymbol{x}) - f(\boldsymbol{x})}{\left | \Delta \boldsymbol{x} \right |}
$$
为了方便，记 $\left | \Delta \boldsymbol{x} \right | = \rho$ ，$\cos \alpha, \cos \beta$ 分别表示方向 $\boldsymbol{l}$ 与 $x, y$ 轴的夹角，那么当偏导数 $f_x, f_y$ 存在时，
$$
D_{\boldsymbol{l}}f(\boldsymbol{x}) = \lim_{\rho \to 0}\frac{f(x + \Delta x, y + \Delta y) - f(x, y)}{\rho} \\\\
= \lim_{\rho \to 0} \frac{f(x + \Delta x, y + \Delta y) - f(x, y + \Delta y) + f(x, y + \Delta y) - f(x, y)}{\rho} \\\\
= \lim_{\rho \to 0} \frac{f(x + \Delta x, y + \Delta y) - f(x, y + \Delta y)}{\rho} + \lim_{\rho \to 0} \frac{f(x, y + \Delta y) - f(x, y)}{\rho} \\\\
= \lim_{\rho \to 0} \frac{f(x + \Delta x, y + \Delta y) - f(x, y + \Delta y)}{\Delta x} \cdot \frac{\Delta x}{\rho} + \lim_{\rho \to 0} \frac{f(x, y + \Delta y) - f(x, y)}{\Delta y}\cdot \frac{\Delta y}{\rho} \\\\
= f_x \cos \alpha + f_y \cos \beta = (f_x, f_y) \cdot (\cos \alpha, \cos \beta) = (f_x, f_y)\cdot \Delta \boldsymbol{x}/\rho
$$
上面推导表示，方向导数可以利用偏导数表示。

# 梯度方向

方向导数有正有负，那么何时取得最大值呢？
$$
\left | D_{\boldsymbol{l}}f(\boldsymbol{x}) \right | = \left | (f_x, f_y)\cdot (\cos \alpha, \cos \beta) \right | = \left | (f_x, f_y) \right | \left | (\cos \alpha, \cos \beta) \right |\cos \gamma \leq \left | (f_x, f_y) \right |
$$
这里 $\left | (\cos \alpha, \cos \beta) \right | = \sqrt{\cos^2 \alpha + \cos^2 \beta} = 1$ ，角度 $\gamma$ 表示梯度向量 $\nabla f = (f_x, f_y)$ 与向量 $\boldsymbol{l}$ 同向单位向量 $\Delta \boldsymbol{x}/\rho = (\cos \alpha, \cos \beta)$ 之间的夹角。可以知道，当两者同向时方向导数取得最大值，反向时取得最小值。

# 更多学习

[导数、方向导数与梯度](https://www.cnblogs.com/bingjianing/p/9014246.html)