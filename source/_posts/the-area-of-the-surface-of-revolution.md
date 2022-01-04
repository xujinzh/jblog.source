---
title: 旋转曲面的面积
mathjax: true
author: Jinzhong Xu
date: 2021-11-07 11:06:14
tags:
- maths
- definite integral
categories:
- maths
- mathematical analysis
---

定积分的所有应用一般总可以安装“分割, 近似求和, 去极限”三个步骤导出所求量的积分形式. 但为简便实用起见, 也常采用本篇介绍的“微元法”. 其实在求旋转体的体积时我们采用微小区间上柱体近似旋转体体积, 但是在求旋转曲面的面积时却不能用旋转体的侧面积近似旋转曲面的侧面积. 就像求弧长时需要用微小区间的弦长近似弧长, 不能用弦长直角边近似一样, 求旋转曲面的表面积不能用圆柱侧面积需要用圆台侧面积近似.

 <!--more-->

# 微元法

在上一篇中我们知道, 若令 $\Phi(x) = \int^x_a f(t) \mathrm{d}t$, 则当 $f$ 为连续函数时, $\Phi^{\prime}(x) = f(x)$, 或 $\mathrm{d}\Phi = f(x)\mathrm{d}x$, 且
$$
\Phi(a) = 0, \Phi(b) = \int^b_a f(x) \mathrm{d}x.
$$
现在问题恰好反过来: 如果所求量 $\Phi$ 是某区间 $[a, x]$ 上的,或者说它是该区间端点 $x$ 的函数, 即 $\Phi = \Phi(x), x\in [a, b]$, 而且当 $x = b$ 时, $\Phi(b)$ 适为最终所求的值.

在任意小区间 $[x, x + \Delta x] \subset [a, b]$ 上, 恰当选取 $\Phi$ 的微小增量 $\Delta \Phi$ 的近似可求量 $\Delta^{\prime} \Phi$ (所谓近似可求量是指用来近似代替 $\Delta \Phi$ 的有确定意义且可以计算的量). 若能把 $\Delta^{\prime} \Phi$ 近似表示为 $\Delta x$ 的线性形式
$$
\Delta^{\prime} \Phi \approx f(x) \Delta x,
$$
其中 $f$ 为某一连续函数,而且当 $\Delta x \to 0$ 时, $\Delta^{\prime} \Phi - f(x)\Delta x = o(\Delta x)$, 则记
$$
\mathrm{d} \Phi = f(x) \mathrm{d}x,
$$
那么只有定积分 $\int^b_a f(x) \mathrm{d}x$ 计算出来,就是该问题所求的结果.

上述方法通常称为 **微元法**. 在采用微元法时, 必须注意如下三点:

- 所求量 $\Phi$ 关于分布区间必须代数可加;
- 微元法的关键是正确给出 $\Delta \Phi$ 的近似可求量 $\Delta^{\prime} \Phi$. 一般来说, 近似可求量的选取不唯一;
- 当我们将 $\Delta^{\prime}\Phi$ 用线性形式 $f(x) \Delta x$ 代替时, 要严格检验 $\Delta^{\prime} \Phi - f(x) \Delta x$ 是否为 $\Delta x$ 的高阶无穷小量, 以保证其对应的积分和的极限是相等的.

# 旋转曲面的面积

设平面光滑曲线 $C$ 的方程为
$$
y = f(x), x \in [a, b] \ (\text{不妨设} f(x) \geq 0).
$$
这段曲线绕 $x$ 轴旋转一周得到旋转曲面.

通过 $x$ 轴上点 $x$ 与 $x + \Delta x$ 分别作垂直于 $x$ 轴的平面,它们在旋转曲面上截下一条夹在两个圆形截线间的狭带. 当 $\Delta x$ 很小时,此狭带的面积 $\Delta S$ 近似于由这两个圆所确定的圆台的侧面积 $\Delta^{\prime} S$, 即
$$
\Delta^{\prime} S = \pi [f(x) + f(x + \Delta x)] \sqrt{\Delta x^2 + \Delta y^2} \\\\
= \pi [2f(x) + \Delta y] \sqrt{1 + (\frac{\Delta y}{\Delta x})^2} \Delta x,
$$
(假设圆柱高$h$, 上下圆面半径$r$, 则圆柱侧面积$S = 2 \pi r h$; 圆台斜边长 $s$, 上圆面半径 $r_1$, 下圆面半径 $r_2$, 则圆台侧面积 $S = 2 \pi \frac{r_1 + r_2}{2} s = \pi (r_1 + r_2) s$)

其中 $\Delta y = f(x + \Delta x) - f(x)$. 由于
$$
\lim_{\Delta x \to 0} \Delta y = 0, \\\\
\lim_{\Delta x \to 0} \sqrt{1 + (\frac{\Delta y}{\Delta x})^2} = \sqrt{1 + {f^{ \prime } }^2(x)},
$$
因此,由 $f^{\prime}(x)$ 的连续性可以保证
$$
\pi [2f(x) + \Delta y]\sqrt{1 + (\frac{\Delta y}{\Delta x})^2} \Delta x - 2 \pi f(x) \sqrt{1 + {f^{ \prime } }^2(x)} \Delta x = o(\Delta x).
$$
所以得到
$$
\Delta^{\prime} S \approx 2 \pi f(x) \sqrt{1 + {f^{ \prime } }^2 (x)} \Delta x, 
$$

$$
\mathrm{d} S = 2 \pi f(x) \sqrt{1 + {f^{\prime } }^2} \mathrm{d}x,
$$

$$
S = 2 \pi \int^b_a f(x) \sqrt{1 + {f^{\prime} }^2} \mathrm{d}x.
$$

## 参数方程

如果光滑曲线 $C$​​ 由参数方程
$$
x = x(t), y = y(t), t \in [\alpha, \beta]
$$
给出, 且 $y(t) \geq 0$, 那么由弧微分知识知道曲线 $C$ 绕 $x$ 轴旋转所得旋转曲面的面积为
$$
S = 2 \pi \int^{\beta}_{\alpha} y(t) \sqrt{ {x^{\prime} }^2(t) + {y^{\prime} }^2(t)} \mathrm{d}t.
$$

## 极坐标方程

如果光滑曲线 $C$​ 由极坐标方程
$$
r = r(\theta), a \leq \theta \leq \alpha ([\alpha, \beta] \subset [0, \pi], r(\theta) \geq 0)
$$
给出, 则由曲线 $C$ 绕极轴旋转所得旋转曲面的面积为
$$
S = 2 \pi \int^{\beta}_{\alpha} r \sin{\theta} \sqrt{r^2 + {r^{\prime} }^2} \mathrm{d} \theta.
$$

