---
title: 有理函数的不定积分
author: Jinzhong Xu
mathjax: true
date: 2020-04-18 12:09:53
tags:
- maths
- integral
categories:
- maths
- mathematical analysis
---

# 有理函数的不定积分

有理函数是指由两个多项式函数的商所表示的函数，其一般形式为

<!--more-->
$$
R(x) = \frac{P(x)}{Q(x)} = \frac{\alpha_0 x^n + \alpha_1 x^{n-1} + \cdots + \alpha_n}{\beta_0 x^m + \beta_1 x^{m-1} + \cdots + \beta_m},
$$
其中$n,m$为非负整数，$\alpha_0, \alpha_1,\cdots, \alpha_n$与$\beta_0,\beta_1, \cdots, \beta_m$都是常数，且$\alpha_0 \neq 0, \beta_0 \neq 0$.若$m > n$，则称它为真分式；若$m \leq n$，则称它为假分式。由多项式的除法可知，假分式总能化为一个多项式与一个真分式之和。多项式的不定积分容易求得，只需要关注真分式的不定积分。

根据代数知识，有理真分式必定可以表示成若干个部分分式之和（称为部分分式分解）。因而问题归结为求那些部分分式的不定积分。为此，先把如何分解部分分式的方法简述如下：

第一步	对分母$Q(x)$在实系数内作标准因式分解：
$$
Q(x) = (x - a_1)^{\lambda_1}\cdots(x - a_k)^{\lambda_k}(x^2 + p_1 x + q_1)^{\mu_1}\cdots(x^2 + p_l x + q_l)^{\mu_l}, 
$$
其中$\beta_0 = 1, \lambda_i, \mu_j(i = 1, 2, \cdots, k; j = 1, 2, \cdots, l)$均为自然数，而且
$$
\sum^k_{i=1}\lambda_i + 2\sum^l_{j=1}\mu_j = m; \\\\
p^2_j - 4q_j < 0, j = 1, 2, \cdots, l.
$$
第二步	根据分母的各个因式分别写出与之相应的部分分式：对于每个形如$(x-a)^k$的因式，它所对应的部分分式是
$$
\frac{A_1}{x-a} + \frac{A_2}{(x-a)J^2} + \cdots + \frac{A_k}{(x-a)^k};
$$
对每个形如$(x^2 + px + q)^k$的因式，它所对应的部分分式是
$$
\frac{B_1 x + C_1}{x^2 + px + q} + \frac{B_2 x + C_2}{(x^2 + px + q)^2} + \cdots + \frac{B_k x + C_k}{(x^2 + px + q)^k}.
$$
把所有部分分式加起来，使之等于$R(x)$. 其中的$A_i, B_i, C_i$为待定的。

第三步	确定待定系数：一般方法是将所有部分分式同分相加，所得分式的分母即为原分母$Q(x)$，而其分子也应与原分子$P(x)$恒等。于是，按同幂项系数必定相等，得到一组关于待定系数的线性方程，这组方程的解就是需要确定的系数。

**例子**	对$R(x) = \frac{2x^4 - x^3 + 4x^2 + 9x - 10}{x^5 + x^4 - 5x^3 - 2x^2 + 4x - 8}$作部分分式分解。

解	首先对分母$Q(x) = x^5 + x^4 - 5x^3 - 2x^2 + 4x - 8$作因式分解。因$Q(x)$首项系数为1，且常数项-8的整数因子有$\pm 1, \pm 2, \pm 4, \pm8$，分别带入这8个因子到$Q(x)$发现$\pm 2$为其整数根。

其次利用综合除法对$Q(x)$按照整数根进行因式分解。

| 整因子 | 最高次项系数         | 次高次项系数            | 更低一次项系数           | 更低一次项系数           | 更低一次项系数                                               | 更低一次项系数                                             |
| ------ | -------------------- | ----------------------- | ------------------------ | ------------------------ | ------------------------------------------------------------ | ---------------------------------------------------------- |
| 2      | 1                    | 1                       | -5                       | -2                       | 4                                                            | -8                                                         |
|        |                      | 2（左下角值1乘2）       | 6（左下角值3乘2）        | 2（左下角值1乘2）        | 0（左下角值0乘2）                                            | 8（左下角值4乘2）                                          |
| -2     | 1（从上上格值1拉下） | 3（上上格1值加上格值2） | 1（上上格-5值加上格值6） | 0（上上格-2值加上格值2） | 4（上上格4值加上格值0）                                      | 0（上上格-8值加上格值8，如果该值为0，说明$x-2$整除$Q(x)$） |
|        |                      | -2                      | -2                       | 2                        | -4                                                           |                                                            |
|        | 1                    | 1                       | -1                       | 2                        | 0（说明$x-(-2) = x+2$整除$\frac{Q(x)}{x-2} = x^4 + 3x^3 + 1x^2 + 4$ |                                                            |

得到$Q(x) = (x-2)(x+2)(x^3 + x^2 - x + 2)$

对于$x^3 + x^2 - x + 2$可检验常数项4的整数分解因子$\pm 1, \pm 2$，发现只有$-2$是其整数根，即$x + 2$能够整除$x^3 + x^2 - x + 2$，利用如上表的综合除法，可以分解如下
$$
x^3 + x^2 - x + 2 = (x + 2)(x^2 - x + 1).
$$
因此，对$R(x)$的分母$Q(x)$可以因式分解为如下
$$
Q(x) = (x - 2)(x + 2)^2(x^2 - x + 1).
$$
然后，按照本节的有理函数因式分解方法，可写出部分分式分解的待定形式为
$$
R(x) = \frac{A_0}{x - 2} + \frac{A_1}{x + 2} + \frac{A_2}{(x + 2)^2} + \frac{Bx + C}{x^2 - x + 1}.
$$
通分后，比较分子与$2x^4 - x^3 + 4x^2 + 9x - 10$的各次项系数，可求得参数$A_0, A_1, A_2, B, C$.

最后，代入待定形式记得$R(x)$的部分分式分解
$$
R(x) = \frac{1}{x - 2} + \frac{2}{x + 2} - \frac{1}{(x + 2)^2} - \frac{x - 1}{x^2 - x + 1}.
$$
一旦完成了部分分式分解，最后归结为求如下两类有理真分式的不定积分：

(1)  $\int \frac{1}{(x - a)^k} \mathrm{d}x$;	(2) $\int \frac{Lx + M}{(x^2 + ps + q)^k} \mathrm{d}x( p^2 - 4q < 0)$.

对于（1），
$$
\int \frac{1}{(x - a)^k} \mathrm{d}x =
\begin{cases}
\ln |x - a| + C, k = 1, \\\\
\frac{1}{(1 - k)(x - a)^{k - 1}} + C, k > 1.
\end{cases}
$$
对于（2），只要作适当换元（令$t = x + \frac{p}{2}$），便化为
$$
\int \frac{Lx + M}{(x^2 + px + q)^k} \mathrm{d}x = \int \frac{Lt + N}{(t^2 + r^2)^k}\mathrm{d}t \\\\
= L\int\frac{t}{(t^2 + r^2)^k} \mathrm{d}t + N\int \frac{dt}{(t^2 + r^2)^k},
$$
其中$r^2 = q - \frac{p^2}{4}, N = M - \frac{p}{2}L.$

当$k = 1$时，上式右边两个不定积分分别为
$$
\int \frac{t}{t^2 + r^2}\mathrm{d}t = \frac{1}{2}\ln (t^2 + r^2) + C, \\\\
\int \frac{\mathrm{d}t}{t^2 + r^2} = \frac{1}{r} \arctan \frac{t}{r} + C.
$$
当$k \geq 2$时，上式右边第一个不定积分为
$$
\int \frac{t}{t^2 + r^2} \mathrm{d}t = \frac{1}{2(1 - k)(t^2 + r^2)^{k - 1}} + C.
$$
对于第二个不定积分，记
$$
I_k = \int \frac{\mathrm{d}t}{(t^2 + r^2)^k},
$$
可用分部积分法导出递推公式如下:
$$
I_k = \frac{1}{r^2} \int \frac{(t^2 + r^2) - t^2}{(t^2 + r^2)^k}\mathrm{d}t \\\\
\quad = \frac{1}{r^2}I_{k-1} - \frac{1}{r^2}\int \frac{t^2}{(t^2 + r^2)^k}\mathrm{d}t \\\\
\quad = \frac{1}{r^2}I_{k-1} + \frac{1}{2r^2(k-1)}\int t \mathrm{d}(\frac{1}{(t^2 + r^2)^{k-1}}) \\\\
\quad = \frac{1}{r^2}I_{k-1} + \frac{1}{2r^2(k-1)}[\frac{t}{(t^2 + r^2)^{k-1}} - I_{k-1}].
$$
经整理得到
$$
I_k = \frac{t}{2r^2(k-1)(t^2+r^2)^{k-1}} + \frac{2k-3}{2r^2}(k-1)I_{k-1}.
$$
重复使用上面的递推公式，最终归为计算$I_1$，$I_1$已经上面计算出。把所有这些局部结果代回，并令$t = x + \frac{p}{2}$就完成了对不定积分（2）的计算。

# 三角函数有理式的不定积分

由$u(x), v(x)$及常数经过有限次四则运算所得到的函数称为关于$u(x), v(x)$的有理式，并用$R(u(x), v(x))$表示。

$\int R(\sin x, \cos x) \mathrm{d}x$ 是三角函数有理式的不定积分。一般使用如下的万能公式，令$t = \tan \frac{x}{2}$,有
$$
\sin x = \frac{2\sin \frac{x}{2}\cos \frac{x}{2}}{\sin^2\frac{x}{2} + \cos^2 \frac{x}{2}} = \frac{2\tan \frac{x}{2}}{1 + \tan^2 \frac{x}{2}} = \frac{2t}{1 + t^2}, \\\\
\cos x = \frac{\cos^2 \frac{x}{2} - \sin^2\frac{x}{2}}{\sin^2\frac{x}{2} + \cos^2\frac{x}{2}} = \frac{1-\tan^2\frac{x}{2}}{1+\tan^2\frac{x}{2}} = \frac{1-t^2}{1+t^2}, \\\\
\tan x = \frac{\sin x}{\cos x} = \frac{2t}{1-t^2}.
$$
又
$$
\mathrm{d}x = \frac{2}{1 + t^2} \mathrm{d}t,
$$
所以，$\int R(\sin x, \cos x) \mathrm{d}x = \int R(\frac{2t}{1+t^2}, \frac{1-t^2}{1+t^2})\frac{2}{1+t^2} \mathrm{d}t$. 这就是上面的有理函数的不定积分。

虽然万能公式适用性比较广，但是在某些情况下却不是简便的方法。如在如下类型中
$$
\int \frac{a \sin^2 x + b\cos^2 x}{c \sin^2 x + d \cos^2 x} \mathrm{d}x,
$$
（分子分母同除以$\cos^2 x$后令$t = \tan x$）

**通常被积函数是$\sin^2 x, \cos^2 x, \sin x \cos x$的有理式时，采用变换$t = \tan x$往往简便**
$$
\int \frac{a \sin x + b \cos x}{c \sin x + d \cos x} \mathrm{d}x,
$$
（把分子形式的写为
$$
a \sin x + b \cos x = A(c \sin x + d \cos x) + B(c \sin x + d \cos x)^{\prime}
$$
然后，比较系数求出$A, B$，即可化为
$$
A\int \mathrm{d}x + B\int \frac{\mathrm{d}(c \sin x + d \cos x)}{c \sin x + d \cos x}.
$$

# 某些无理根式的不定积分

## 第一种无理根式

$\int R(x, \sqrt[n]{\frac{ax + b}{cx + d}}) \mathrm{d}x$ 型不定积分$(ad - bc \neq 0)$。对此只需令$t = \sqrt[n]{\frac{ax + b}{cx + d}}$, 就可化为有理函数的不定积分。

## 第二种无理根式

$\int R(x, \sqrt{ax^2 + bx + c}) \mathrm{d}x$型不定积分（$a > 0$时$b^2 - 4ac \neq 0,\ a < 0$时$b^2 - 4ac > 0$）。由于
$$
ax^2 + bx + c = a[(x + \frac{b}{2a})^2 + \frac{4ac - b^2}{4a^2}],
$$
若记$u = x + \frac{b}{2a}, k^2 = |\frac{4ac - b^2}{4a^2}|$，则此二次三项式必属于以下三种情形之一：
$$
|a|(u^2 + k^2), \quad |a|(u^2 - k^2), \quad |a|(k^2 - u^2).
$$
因此上述无理根式的不定积分也就转化为以下三种类型之一：
$$
\int R(u, \sqrt{u^2 \pm k^2}) \mathrm{d}u, \quad \int R(u, \sqrt{k^2 - u^2}) \mathrm{d}u.
$$
当分别令$u = k\tan t, u = k\sec t, u = k \sin t$后，它们都化为三角有理式的不定积分。

**一般地，二次三项式$ax^2 + bx + c$中若$a>0$，则可令**
$$
\sqrt{ax^2 + bx + c} = \sqrt{a}x \pm t;
$$
**若$c>0$，还可令**
$$
\sqrt{ax^2 + bx + c} = xt \pm \sqrt{c}.
$$
**这类变换称为<font color=#dd0000dd>欧拉变换</font>**。

至此，已经学了求不定积分的基本方法，以及某些特殊类型不定积分的求法。需要指出的是，通常所说的“求不定积分”是指<font color=dddddd00>用初等函数的形式把不定积分表示出来</font>。在这个意义下，并不是任何初等函数的不定积分都能“求出来”的，如
$$
\int e^{\pm x^2} \mathrm{d}x, \quad \int \frac{\mathrm{d}x}{\ln x}, \quad \int \frac{\sin x}{x}\mathrm{d}x, \quad \int \sqrt{1 - k^2 \sin^2 x} \mathrm{d}x \ (0 < k^2 < 1).
$$
等等，虽然它们都存在，但却无法用初等函数来表示。这一结论是有刘维尔（Liouville）于1835年作出过证明。因此，初等函数的原函数不一定是初等函数。但是，这些函数可以采用定积分形式来表示。

求不定积分除了使用手动推到的方法，也可以利用某些计算器（如TI-92型）和计算机软件（如Mathemetica, Maple, Python包Sympy）来作。例如使用Python的Sympy包求函数:



```python
from sympy import *
x = symbols('x')
expr = sin(x) / x
expr_int = Integral(expr, x)
expr_int
```

结果显示
$$
\int \frac{\sin(x)}{x} \mathrm{d}x
$$


```python
expr_int.doit()
```

结果显示
$$
Si(x)
$$
值得注意的是，虽然该函数有显示表达式，但却不属于初等函数范畴，其定义式如下
$$
Si(x) = \int^x_0 \frac{\sin(x)}{x} \mathrm{d}x.
$$

利用python还可以算出
$$
\int e^{-x^2} \mathrm{d}x = \frac{\sqrt{\pi}\cdot erf(x)}{2}, \\\\
\int e^{x^2} \mathrm{d}x = \frac{\sqrt{\pi}\cdot erfi(x)}{2}, \\\\
\int \frac{1}{\ln x} \mathrm{d}x = li(x), \\\\
\int \sqrt{1 - k^2 \sin^2 x} \mathrm{d}x = E(x|k^2).
$$
需要注意的是，$\int \sqrt{1 - k^2 \sin^2 x} \mathrm{d}x \ (0 < k^2 < 1)$, 计算花费的时间会长一些，其他都是及时返回结果。

# 重要例子

(1) $\int \frac{\mathrm{d}x}{1 + x^4}$

解	因为
$$
1 + x^4 = (1 + 2x^2 + x^4) - 2x^2 \\\\
\quad = (1 + x^2)^2 - (\sqrt{2}x)^2 \\\\
\quad = (1 + \sqrt{2}x + x^2)(1 - \sqrt{2}x + x^2).
$$
所以，原式子可以化为
$$
\int \frac{1}{1 + x^4} \mathrm{d}x = \int \frac{1}{(1 + \sqrt{2}x + x^2)(1 -\sqrt{2}x + x^2)} \mathrm{d}x
$$
然后，再利用有理函数的不定积分方法计算。

值得注意的，也可以利用如下方法求解。

解	注意到
$$
x^2 + \frac{1}{x^2} = (x + \frac{1}{x})^2 - 2 = (x - \frac{1}{x})^2 + 2.
$$
所以，
$$
\int \frac{1}{1 + x^4} \mathrm{d}x = \int \frac{\frac{1}{x^2}}{x^2 + \frac{1}{x^2}} \mathrm{d}x = \frac{1}{2}\int \frac{(1+\frac{1}{x^2}) - (1 - \frac{1}{x^2})}{x^2 + \frac{1}{x^2}} \mathrm{d}x \\\\
\quad = \frac{1}{2}[\int \frac{\mathrm{d}(x - \frac{1}{x})}{(x-\frac{1}{x})^2 + 2} - \int \frac{\mathrm{d}(x + \frac{1}{x})}{(x + \frac{1}{x})^2 - 2}] \\\\
\quad = \frac{1}{2}[\frac{1}{\sqrt{2}}\arctan\frac{x-\frac{1}{x}}{\sqrt{2}} - \frac{1}{2\sqrt{2}}\ln |\frac{x + \frac{1}{x} - \sqrt{2}}{x + \frac{1}{x} + \sqrt{2}}|] + C.
$$
（2）$\int \frac{\mathrm{d}x}{\sqrt{x^2 + x}}$

解	
$$
\int \frac{\mathrm{d}x}{\sqrt{x^2 + x}} = \int \frac{\mathrm{d}x}{\sqrt{x}\sqrt{x + 1}} = 2\int\frac{\mathrm{d}\sqrt{x}}{\sqrt{(\sqrt{x})^2 + 1}} = 2 \ln |\sqrt{x} + \sqrt{x + 1}| + C.
$$
利用换元$\sqrt{x} = \tan t$.

