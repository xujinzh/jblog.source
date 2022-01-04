---
title: 牛顿方法求解方程根
author: Jinzhong Xu
mathjax: true
date: 2020-07-26 20:31:11
tags:
- maths
- Newton
categories:
- maths
- optimization
---

牛顿法求极小值和方程根

<!--more-->

牛顿法求解函数的极小值，用二阶泰勒展开近似目标函数：
$$
f(x) \approx f(x_0) + f^{\prime}(x_0)(x - x_0) + \frac{1}{2} f^{\prime\prime}(x_0)(x - x_0)^2 \triangleq g(x)
$$
要求原函数 $f(x)$ 的极小值，可以用求近似函数 $g(x)$ 的极小值来近似。因为 $g(x)$ 是关于 $x$ 的二次函数，所以令 $ g(x) = 0$ 求极小值点：
$$
f^{\prime}(x_0) + f^{\prime\prime}(x_0)(x - x_0) = 0
$$
即
$$
x = x_0 - \frac{f^{\prime}(x_0)}{f^{\prime\prime}(x_0)}
$$
得到迭代公式：
$$
x_n = x_{n - 1} - \frac{f^{\prime}(x_{n - 1})}{f^{\prime\prime}(x_{n - 1})}
$$
**对此公式的解释**：

求解函数 $f(x)$ 的极小值，相当于求解导函数 $f^{\prime}$ 的零点。对于求函数的零点可以用切线的与 $x$ 轴的交点来迭代计算。首先，选择一个接近函数 $f^{\prime}(x_0)$ 零点的 $x_0$ ，计算相应的 $f^{\prime}(x_0)$ 和切线斜率 $f^{\prime\prime}(x_0)$ 。然后我们计算穿过点 $(x_0, f^{\prime}(x_0))$ 并且斜率为 $f^{\prime\prime}(x_0)$ 的直线和 $x$ 轴的交点的横坐标，也就是求如下方程的解：
$$
f^{\prime}(x_0) - 0 = f^{\prime\prime}(x_0)(x - x_0)
$$
我们将求得的点的 $x$ 坐标命名为 $x_1$ ，通常 $x_1$ 会比 $x_0$ 更接近方程 $f^{\prime}(x) = 0$ 的解。因此我们现在可以利用 $x_1$ 开始下一轮迭代。迭代公式可以化简为如下所示：
$$
x_n = x_{n-1} - \frac{f^{\prime}(x_{n-1})}{f^{\prime\prime}(x_{n-1})}
$$
转化成求极值点就是上面的导数形式。

**对于多元函数的牛顿法：**

**基本牛顿法**（还有阻尼牛顿法以及修正牛顿法）

设 $f(\boldsymbol{x})$ 具有连续的二阶偏导数，当前迭代点是 $\boldsymbol{x}_k$ 。 $f(\boldsymbol{x})$ 在 $\boldsymbol{x}_k$ 处的Taylor展开为
$$
f(\boldsymbol{x}_k + \boldsymbol{d}) = f_k + \boldsymbol{g}^T_k \boldsymbol{d} + \frac{1}{2}\boldsymbol{d}^T \boldsymbol{G}_k \boldsymbol{d} + o(\|\boldsymbol{d}\|^2)
$$
其中 $ \boldsymbol{d} = \boldsymbol{x} - \boldsymbol{x}_k, f_k = f(\boldsymbol{x}_k)$，$\boldsymbol{g}_k$ 是梯度方向，$\boldsymbol{G}_k$ 是黑塞矩阵。. 在点 $\boldsymbol{x}_k$ 的邻域内，用二次函数
$$
q_k(\boldsymbol{d}) \triangleq f_k + \boldsymbol{g}^T_k \boldsymbol{d} + 
\frac{1}{2} \boldsymbol{d}^T \boldsymbol{G}_k \boldsymbol{d}
$$
近似$f(\boldsymbol{x}_k + \boldsymbol{d})$，求解问题
$$
\min q_k(\boldsymbol{d}) = f_k + \boldsymbol{g}^T_k \boldsymbol{d} + \frac{1}{2} \boldsymbol{d}^T \boldsymbol{G}_k \boldsymbol{d}
$$
若 $\boldsymbol{G}_k$ 正定，对向量 $\boldsymbol{d}$ 求导，则方程组
$$
\boldsymbol{G}_k \boldsymbol{d} = - \boldsymbol{g}_k
$$
的解 $\boldsymbol{d}_k = - \boldsymbol{G}^{-1}_k \boldsymbol{g}_k$ 为上式的唯一解，我们称上式为Newton方程， $\boldsymbol{d}_k$ 为牛顿方向。用牛顿方向作为迭代方向的最优化方法称为牛顿方法。

基本牛顿方法指取步长 $\alpha_k = 1$ 的牛顿方法。在不引起混淆的情况下，基本牛顿方法简称为牛顿方法。在牛顿方法中，只要黑塞矩阵（Hessian Matrix）$\boldsymbol{G}_k$ 正定，牛顿方向 $\boldsymbol{d}_k$ 就是下降方向，因为
$$
\boldsymbol{g}^T_k \boldsymbol{d}_k = -\boldsymbol{g}^T_k\boldsymbol{G}^{-1}_k \boldsymbol{g}_k < 0
$$
算法（基本牛顿法）

步1 给出 $ \boldsymbol{x}_0 \in \mathbb{R}^n, \varepsilon > 0, k = 0$ ;

步2 若终止准则满足，则输出有关信息，停止迭代；

步3 由牛顿方程计算 $\boldsymbol{d}_k$ ；

步4 $\boldsymbol{x}_{k + 1} = \boldsymbol{x}_k + \boldsymbol{d}_k, k = k + 1$，转步2.

**牛顿方法的优缺点：**

牛顿方法的收敛性依赖于初始点的选择。当初始点接近极小点时，迭代序列收敛于极小点，并且收敛很快；否则就会出现迭代序列收敛到铵点或极大点的情形，或者在迭代过程中出现矩阵奇异或病态的情形，使线性方程组不能求解或不能很好地求解，导致迭代失败。

定理（基本牛顿方法的收敛性）设 $f(x) \in \mathbb{C}^2$, $f(x)$   的黑塞矩阵 $G(x)$ 满足 Lipschitz 条件，即存在$\beta > 0$ ，对任给的 $x$ 与 $y$ ，有 $\|G(x) - G(y)\| \leq \beta \|x - y\|$ 若 $x_0$ 充分接近 $f(x)$ 的局部极小点 $x^{\star}$ ，且 $G^{\star}$ 正定，则牛顿方法对所有的 $k$ 有定义，并以二阶收敛速度收敛。

该定理说，只有当迭代点充分接近 $x^\star$ 时，基本牛顿方法的收敛性才能保证。

**优点**：

1， 当 $x_0$ 充分接近问题的极小点 $x^\star$ 时，方法以二阶收敛速度收敛；

2， 方法具有二次终止性。

**缺点：**

1， 当 $x_0$ 没有充分接近问题的极小点 $x^\star$ 时，$G_k$ 会出现不正定或奇异的情形，使 $\{x_k\}$ 不能收敛到 $x^\star$ ，或使迭代无法进行；即使 $G_k$ 正定，也不能保证 $\{f_k\}$ 单调下降；

2， 每步迭代需要计算黑塞矩阵，即计算 $\frac{n(n+1)}{2}$ 个二阶偏导数；

3， 每步迭代需要解一个线性方程组，计算量为 $O(n^3)$ .

阻尼牛顿方法

为了改善牛顿方法的局部收敛性，我们可以采用带一维搜索的牛顿方法，即
$$
x_k = x_{k-1} + \alpha_{k-1}d_{k-1}
$$
其中，$\alpha_{k-1}$是一维搜索的结果。该方法称为阻尼牛顿方法，此方法能够保证对正定的$G_k$，$\{f_k\}$ 单调下降，即使$x_k$离$x^\star$稍远，该方法产生的点列$\{x_k\}$仍可能收敛至 $x^\star$。 对严格凸函数，采用 Wolfe 准则的阻尼牛顿方法具有全局收敛性。