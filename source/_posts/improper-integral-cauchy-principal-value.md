---
title: 反常积分与柯西主值
mathjax: true
author: Jinzhong Xu
date: 2021-11-22 22:21:12
tags:
- cauchy
- integral
- maths
categories:
- maths
- mathematical analysis
---

在讨论定积分时有两个默认的限制条件：一是积分区间有穷，二是被积函数有界。在实际问题中往往需要突破这些限制，如需要在无穷区间上进行积分，或求无解函数的积分(函数在积分区间的某点上无界)。此时，研究的积分称为反常积分(improper integral，或称为非正常积分，广义积分。原有的定积分为Riemann积分，或常义积分，记作$f\in R[a,b]$，表示函数$f$ 在闭区间$[a,b]$上黎曼可积)。在研究反常积分时常常研究其收敛性或发散性，这一点与数项级数一致。更进一步，含参量反常积分和函数项级数研究一致收敛性和发散性。

<!--more-->

# 两类反常积分

## 无穷积分

无穷积分是第一类反常积分，指积分区间的上限或下限为无穷的积分。

设函数$f$定义在无穷区间$[a,+\infty]$上，且在任何有限区间$[a,u]$上可积。如果存在极限
$$
\lim_{u\to +\infty}\int^u_a f(x) \mathrm{d}x = J,
$$
则称此极限$J$为函数$f$在$[a,+\infty]$上的**无穷限反常积分**(简称**无穷积分**)，记作
$$
J = \int^{+\infty}_a f(x) \mathrm{d}x,
$$
并称$\int^{+\infty}_a f(x) \mathrm{d}x$收敛，如果极限不存在，则称$\int^{+\infty}_a f(x) \mathrm{d}x$发散。

类似地，可定义$f$在$(-\infty,b)$上的无穷积分：
$$
\int^b_{-\infty}f(x)\mathrm{d}x=\lim_{v\to -\infty}\int^b_v f(x)\mathrm{d}x.
$$
对于$f$在$(-\infty,+\infty)$上的无穷积分，定义如下：
$$
\int_{-\infty}^{+\infty} f(x) \mathrm{d}x = \int^a_{-\infty} f(x) \mathrm{d}x + \int^{+\infty}_a f(x) \mathrm{d}x.
$$


其中，$a$为任一实数，当且仅当右边两个无穷积分都收敛时它才收敛。收敛性与收敛时的值都和实数$a$的选取无关。$f$在任何有限区间$[v,u]\subset(-\infty,+\infty)$上必须可积。前面定积分的换元法、分部积分法仍然适用。

$\int^{+\infty}_a f(x)\mathrm{d}x$收敛的几何意义：若$f$在$[a,+\infty]$上为非负连续函数，则曲线$y=f(x)$，直线$x=a$以及$x$轴之间所围区域面积为$J$.

无穷积分$\int_1^{+\infty} \frac{1}{x^p} \mathrm{d}x$在$p>1$时收敛到$\frac{1}{p-1}$；在$p\leq 1$时发散。

## 瑕积分

瑕积分为第二类反常积分，指被积函数在积分区间中含有不连续点的积分。

设函数$f$定义在区间$(a,b]$上，在点$a$的任意右邻域内无界，但在任何内闭区间$[u,b]\subset(a,b]$上有界且可积。如果存在极限
$$
\lim_{u\to a^+} \int_u^b f(x) \mathrm{d}x = J,
$$
则称此极限为无界函数$f$在$(a,b]$上的反常积分，记作
$$
J = \int_a^b f(x) \mathrm{d}x,
$$
并称反常积分$\int_a^b f(x) \mathrm{d}x$收敛。如果极限不存在，说反常积分$\int_a^b f(x) \mathrm{d}x$发散。称点$a$为$f$的瑕点，无界函数反常积分$\int_a^b f(x)\mathrm{d}x$又称为**瑕积分**。

类似地，可定义瑕点为$b$时的瑕积分：
$$
\int_a^b f(x) \mathrm{d}x = \lim_{v \to b^-} \int_a^v f(x) \mathrm{d}x.
$$
其中$f$在$[a,b)$有定义，在点$b$的任一左邻域内无界，但在任何$[a,v] \subset [a,b)$上可积。

若$f$的瑕点$c\in(a,b)$，则定义瑕积分
$$
\int_a^b f(x) \mathrm{d}x = \int_a^c f(x) \mathrm{d}x + \int_c^b f(x) \mathrm{d}x \\\\
= \lim_{u\to c^-} \int_a^u f(x)\mathrm{d}x + \lim_{v\to c^+}\int_v^b f(x)\mathrm{d}x.
$$
其中$f$在$[a,c)\cup(c,b]$上有定义，在点$c$的任一邻域内无界，但任何$[a,u]\subset [a,c)$和$[v,b]\subset(c,b]$上都可积。当且仅当上式右边两个瑕积分都收敛时，左边的瑕积分才是收敛的。

又若$a,b$两点都是$f$的瑕点，而$f$在任何$[u,v]\subset(a,b)$上可积，这是定义瑕积分
$$
\int_a^b f(x)\mathrm{d}x = \int_a^c f(x)\mathrm{d}x + \int_c^b f(x)\mathrm{d}x \\\\
= \lim_{u\to a^+} \int_u^c f(x)\mathrm{d}x + \lim_{v \to b^-} f(x) \mathrm{d}x,
$$
其中$c$为$(a,b)$内任一实数。同样地，当且仅当上式右边两个瑕积分都收敛时，左边的瑕积分才是收敛的。

瑕积分$\int_0^1 \frac{1}{x^p} \mathrm{d}x \ (p > 0)$，当$0<p<1$时收敛到$\frac{1}{1-p}$，当$p\geq 1$时发散.

反常积分$\int_0^{+\infty} \frac{1}{x^p}\mathrm{d}x \ (p>0)$收敛还是发散？我们定义
$$
\int_0^{+\infty}\frac{1}{x^p}\mathrm{d}x = \int_0^1 \frac{1}{x^p}\mathrm{d}x + \int_1^{+\infty}\frac{1}{x^p}\mathrm{d}x,
$$
它当且仅当右边的瑕积分和无穷积分都收敛时才收敛。有上面可知，这两个反常积分不能同时收敛，故对于任何实数$p>0$都是发散的。

# 柯西主值

设函数$f$ 在$(-\infty, +\infty)$上内闭可积（即在任何闭子区间上都可积），定义
$$
\textbf{PV} \int_{-\infty}^{+\infty} f(x) \mathrm{d}x = \lim_{A \to +\infty} \int_{-A}^A f(x) \mathrm{d}x
$$
为反常积分（广义积分）$\int_{-\infty}^{+\infty} f(x) \mathrm{d}x$的柯西主值（Cauchy Principal Value），如果右边极限存在的话。

对于无界积分。设$f$在区间$[a,b]$中只有一个瑕点$c$，$a<c<b$，则定义
$$
\textbf{PV} \int_a^b f(x)\mathrm{d}x = \lim_{\delta \to 0^+}(\int_a^{c-\delta} + \int_{c+\delta}^b) f(x)\mathrm{d}x
$$
为广义积分$\int_a^b f(x)\mathrm{d}x$的柯西主值，如果右边极限存在的话。

容易看出，若广义积分收敛，则其主值与广义积分的值相同；但是当广义积分发散时，它的主值仍可能存在。

如$\int_{-\infty}^{+\infty}x \mathrm{d}x$发散，但是$\textbf{PV}\int_{-\infty}^{+\infty}x\mathrm{d}x=\lim_{R\to +\infty} \int_{-R}^R x \mathrm{d}x = 0$.

如$\int_{-1}^1 \frac{1}{x}\mathrm{d}x$发散，但是$\textbf{PV}\int_{-1}^1 \frac{1}{x}\mathrm{d}x=\lim_{\varepsilon \to 0^+}\int_{-1+\varepsilon}^{1-\varepsilon}\frac{1}{x}\mathrm{d}x=0$.

若已知广义积分收敛时，可利用其主值来积分积分值。

# 例子

1. 举例说明：瑕积分$\int_a^b f(x)\mathrm{d}x$收敛时，$\int_a^b f^2(x)\mathrm{d}x$不一定收敛。如$\int_0^1 \frac{1}{\sqrt{x}} \mathrm{d}x$
2. 举例说明：$\int_a^{+\infty}f(x) \mathrm{d}x$收敛且$f$在$[a,+\infty)$上连续时，不一定有$\lim_{x\to +\infty}f(x)=0.$ 如$\int_1^{+\infty}\sin x^2 \mathrm{d}x$.
3. 证明：若$\int_a^{+\infty}f(x)\mathrm{d}x$收敛，且存在极限$\lim_{n\to +\infty}f(x)=A$，则$A=0$.
4. 若$\int_a^{+\infty}f(x)\mathrm{d}x$收敛，则$\lim_{x\to +\infty}f(x)$可以不存在或者存在且为0.
5. 证明：若$f$在$[a,+\infty)$上可导，且$\int_a^{+\infty}f(x)\mathrm{d}x$与$\int_a^{+\infty}f^{\prime}(x)\mathrm{d}x$都收敛，则$\lim_{x\to +\infty} f(x) = 0$.

