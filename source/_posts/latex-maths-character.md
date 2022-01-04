---
title: LaTeX 数学符号
author: Jinzhong Xu
mathjax: true
date: 2020-04-16 14:16:21
tags:
- latex
- maths
categories: maths
---

# LaTeX 数学符号

## 常用基本LaTeX书写

<!--more-->

### 幂和下脚标

```latex
$x_1$ \quad $x^2$ \quad $e^{-\alpha x}$ \quad $x^3_{ij}$ \quad $e^{x^3}$ \neq $e^{3x}$
```

$$
x_1 \quad x^2 \quad e^{-\alpha x} \quad x^3_{ij} \quad e^{x^3} \neq e^{3x}
$$

### 开方

```latex
$\sqrt{x}$ \quad $\sqrt{x^2 + y^2}$ \qquad $\sqrt[3]{x}$ \quad $\surd[x^3 + y^3]$
```

$$
\sqrt{x} \quad \sqrt{x^2 + y^2} \qquad \sqrt[3]{x} \quad \surd[x^3 + y^3]
$$

### 上下水平线

```latex
$\overline{x + y}$ \qquad $\underline{x - y}$
```

$$
\overline{x + y} \qquad \underline{x - y}
$$

### 上下大括号

```latex
$\overbrace{\Lambda + \Upsilon + \cdots + \Gamma}^{i}$ \qquad $\underbrace{\alpha + \beta + \cdots + \gamma}_{j}$
```

$$
\overbrace{\Lambda + \Upsilon + \cdots + \Gamma}^{i} \qquad \underbrace{\alpha + \beta + \cdots + \gamma}_{j}
$$

### 向量

```latex
$\vec{x}$ \qquad $\overrightarrow{XY}$
```

$$
\vec{x} \qquad \overrightarrow{XY}
$$

### 分式

```latex
$\frac{1}{2}$ \quad $\frac{x^2}{z + 2}$ \quad $e^{\frac{n}{\sin x + 1}}$ \quad \sqrt{x}=x^{1/2}$
```

$$
\frac{1}{2} \quad \frac{x^2}{z + 2} \quad e^{\frac{n}{\sin x + 1}} \quad \sqrt{x}=x^{1/2}
$$

### 累加、累积、积分

```latex
$\sum_{i = 1}^n i = \frac{n(n+1)}{2}$ \quad $\prod_{j = 1}^{n} = n!$ \quad $\int_{0}^{\pi/2} \sin x \mathrm{d}x = 1$ 
```

$$
\sum_{i = 1}^n i = \frac{n(n+1)}{2} \quad \prod_{j = 1}^{n} = n! \quad \int_{0}^{\pi/2} \sin x \mathrm{d}x = 1
$$

## 数学符号表

### 数学模式重音符

| $\hat{x}$  \hat{x}     | $\check{x}$  \check{x} | $\tilde{x}$  \tilde{x}     | $\acute{x}$  \acute{x}         |
| ---------------------- | ---------------------- | -------------------------- | ------------------------------ |
| $\grave{x}$  \grave{x} | $\dot{x}$  \dot{x}     | $\ddot{x}$  \ddot{x}       | $\breve{x}$  \breve{x}         |
| $\bar{x}$  \bar{x}     | $\vec{x}$  \vec{x}     | $\widehat{X}$  \widehat{X} | $\widetilde{Y}$  \widetilde{Y} |

### 小写希腊字母

| $\alpha$  \alpha     | $\beta$  \beta             | $\gamma$  \gamma       | $\delta$  \delta    |
| -------------------- | -------------------------- | ---------------------- | ------------------- |
| $\epsilon$  \epsilon | $\varepsilon$  \varepsilon | $\zeta$  \zeta         | $\eta$  \eta        |
| $\theta$  \theta     | $\vartheta$  \vartheta     | $\iota$  \iota         | $\kappa$  \kappa    |
| $\lambda$  \lambda   | $\mu$  \mu                 | $\nu$  \nu             | $\xi$  \xi          |
| $\omicron$  \omicron | $\pi$  \pi                 | $\varpi$  \varpi       | $\rho$  \rho        |
| $\sigma$  \sigma     | $\varsigma$  \varsigma     | $\tau$  \tau           | $\upsilon$ \upsilon |
| $\phi$  \phi         | $\varphi$  \varphi         | $\chi$  \chi           | $\psi$ \psi         |
| $\omega$  \omega     | $\varrho$  \varrho         | $\varsigma$  \varsigma |                     |

### 大写希腊字母

| $\Theta$  \Theta     | $\Psi$  \Psi     | $\Gamma$  \Gamma | $\Lambda$  \Lambda |
| -------------------- | ---------------- | ---------------- | ------------------ |
| $\Sigma$  \Sigma     | $\Delta$  \Delta | $\Xi$  \Xi       | $\Pi$  \Pi         |
| $\Upsilon$  \Upsilon | $\Phi$  \Phi     | $\Omega$  \Omega |                    |

### 二元关系符

| $<$  <                   | $>$  >                     | $=$  =              | $\neq$  \neq             |
| ------------------------ | -------------------------- | ------------------- | ------------------------ |
| $\leq$  \leq or \le      | $\geq$  \geq or \ge        | $\equiv$  \equiv    | $\doteq$  \doteq         |
| $\ll$  \ll               | $\gg$  \gg                 | $\sim$  \sim        | $\approx$  \approx       |
| $\prec$  \prec           | $\succ$  \succ             | $\simeq$  \simeq    | $\cong$  \cong           |
| $\preceq$  \preceq       | $\succeq$  \succeq         | $\in$  \in          | $\ni$  \ni or \owns      |
| $\subset$  \subset       | $\supset$  \supset         | $\Join$  \Join      | $\bowtie$  \bowtie       |
| $\subseteq$  \subseteq   | $\supseteq$  \supseteq     | $\propto$  \propto  | $\vdash$  \vdash         |
| $\sqsubset$  \sqsubset   | $\sqsupset$  \sqsupset     | $\models$  \models  | $\dashv$  \dashv         |
| $\sqsubseteq$  \sqsubset | $\sqsupseteq$  \sqsupseteq | $\smile$  \smile    | $\perp$  \perp           |
| $\mid$  \mid             | $\parallel$  \parallel     | $\frown$  \frown    | $\asymp$  \asymp         |
| $\not \leq$  \not \leq   | $\not\prec$  \not \prec    | $\not \in$ \not \in | $\not \perp$  \not \perp |

### 二元运算符

| $+$  +                          | $-$  -                               | $\pm$  \pm                    | $\mp$  \mp                      |
| ------------------------------- | ------------------------------------ | ----------------------------- | ------------------------------- |
| $\cdot$  \cdot                  | $\div$  \div                         | $\times$  \times              | $\setminus$  \setminus          |
| $\cup$  \cup                    | $\cap$  \cap                         | $\sqcup$  \sqcup              | $\sqcap$  \sqcap                |
| $\vee$ \vee or \lor             | $\wedge$ \wedge or \land             | $\triangleleft$ \triangleleft | $\triangleright$ \triangleright |
| $\star$ \star                   | $\ast$ \ast                          | $\circ$ \circ                 | $\bullet$  \bullet              |
| $\oplus$  \oplus                | $\ominus$  \ominus                   | $\odot$  \odot                | $\oslash$  \oslash              |
| $\otimes$  \otimes              | $\oslash$  \oslash                   | $\diamond$  \diamond          | $\uplus$  \uplus                |
| $\bigtriangleup$ \bigtriangleup | $\bigtriangledown$  \bigtriangledown | $\amalg$  \amalg              | $\wr$  \wr                      |
| $\lhd$  \lhd                    | $\rhd$  \rhd                         | $\dagger$  \dagger            | $\ddagger$  \ddagger            |
| $\unlhd$  \unlhd                | $\unrhd$  \unrhd                     |                               |                                 |

### 大尺寸运算符

| $\sum$  \sum         | $\prod$  \prod         | $\coprod$  \coprod     | $\int$  \int             |
| -------------------- | ---------------------- | ---------------------- | ------------------------ |
| $\bigcup$  \bigcup   | $\bigcap$  \bigcap     | $\bigsqcup$  \bigsqcup | $\oint$  \oint           |
| $\bigvee$  \bigvee   | $\bigwedge$  \bigwedge | $\bigoplus$  \bigoplus | $\bigotimes$  \bigotimes |
| $\bigodot$  \bigodot | $\biguplus$  \biguplus |                        |                          |

### 箭头

| $\leftarrow$  \leftarrow or \gets         | $\rightarrow$  \rightarrow or \to         | $\leftrightarrow$ \leftrightarrow       | $\mapsto$  \mapsto                    |
| ----------------------------------------- | ----------------------------------------- | --------------------------------------- | ------------------------------------- |
| $\Leftarrow$  \Leftarrow                  | $\Rightarrow$  \Rightarrow                | $\Leftrightarrow$ \Leftrightarrow       | $\longmapsto$ \longmapsto             |
| $\hookleftarrow$ \hookleftarrow           | $\hookrightarrow$ \hookrightarrow         | $\leftharpoonup$ \leftharpoonup         | $\rightharpoonup$ \rightharpoonup     |
| $\longleftarrow$ \longleftarrow           | $\longrightarrow$ \longrightarrow         | $\leftharpoondown$ \leftharpoondown     | $\rightharpoondown$ \rightharpoondown |
| $\Longleftarrow$ \Longleftarrow           | $\Longrightarrow$ \Longrightarrow         | $\rightleftharpoons$ \rightleftharpoons | $\iff$ \iff                           |
| $\longleftrightarrow$ \longleftrightarrow | $\Longleftrightarrow$ \Longleftrightarrow | $\uparrow$  \uparrow                    | $\downarrow$ \downarrow               |
| $\nearrow$  \nearrow                      | $\swarrow$ \swarrow                       | $\Uparrow$ \Uparrow                     | $\Downarrow$ \Downarrow               |
| $\searrow$ \searrow                       | $\nwarrow$  \\nwarrow                     | $\updownarrow$ \updownarrow             | $\Updownarrow$ \Updownarrow           |
| $\leadsto$ \leadsto                       |                                           |                                         |                                       |

### 定界符

| $($  (               | $)$ )                    | $\vert$  \vert or \|      | $\Vert$  \Vert or \\|
| -------------------- | ------------------------ | ------------------------- | ------------------------- |
| $[$  [ or \lbrack   | $]$ ] or \rbrack        | $\lceil$  \lceil          | $\rceil$  \rceil          |
| $\lbrace$ \\{ or \lbrace | $\rbrace$ \\} or \rbrace | $\lgroup$ \lgroup         | $\rgroup$  \rgroup        |
| $\langle$  \langle   | $\rangle$  \rangle       | $\lmoustache$  lmoustache | $\rmoustache$ \rmoustache |
| $\lfloor$  \lfloor   | $\rfloor$  \rfloor       | $\arrowvert$  \arrowvert  | $\Arrowvert$  \Arrowvert  |
| $/$  /               | $\backslash$  \backslash | $\bracevert$  bracevert   |                           |

### 其他符号

| $\cdots$  \cdots             | $\dots$  \dots           | $\vdots$  \vdots       | $\ddots$  \ddots         |
| ---------------------------- | ------------------------ | ---------------------- | ------------------------ |
| $\hbar$  \hbar               | $\imath$  \imath         | $\jmath$  \jmath       | $\ell$  \ell             |
| $\Re$  \Re                   | $\Im$  \Im               | $\aleph$  \aleph       | $\wp$  \wp               |
| $\forall$  \forall           | $\exists$  \exists       | $\mho$  \mho           | $\partial$  \partial     |
| $'$  '                       | $\prime$  \prime         | $\emptyset$  \emptyset | $\infty$  \infty         |
| $\nabla$  \nabla             | $\triangle$  \triangle   | $\Box$  \Box           | $\Diamond$  \Diamond     |
| $\bot$  \bot                 | $\top$  \top             | $\angle$  \angle       | $\surd$  \surd           |
| $\diamondsuit$  \diamondsuit | $\heartsuit$  \heartsuit | $\clubsuit$  \clubsuit | $\spadesuit$  \spadesuit |
| $\neg$  \neg or \lnot        | $\flat$  \flat           | $\natural$  \natural   | $\sharp$  \sharp         |
| $\S$  \S                     | $\ulcorner$ \ulcorner    | $\urcorner$  \urcorner | $\llcorner$  \llcorner   |
| $\lvert$  \lvert             | $\lVert$  \lVert         | $\rVert$  \rVert       | $\lrcorner$  lrcorner    |
| $\rvert$  \rvert             | $\digamma$  \digamma     | $\varkappa$  \varkappa | $\P$                     |
| $\beth$  \beth               | $\daleth$  \daleth       | $\gimel$  \gimel       |                          |

### AMS 二元关系符

| $\lessdot$  \lessdot         | $\gtrdot$  \gtrdot           | $\doteqdot$  \doteqdot                   | $\triangleq$  \triangleq                   |
| ---------------------------- | ---------------------------- | ---------------------------------------- | ------------------------------------------ |
| $\leqslant$  \leqslant       | $\geqslant$  \geqslant       | $\risingdotseq$  \risingdotseq           | $\bumpeq$  \bumpeq                         |
| $\eqslantless$  \eqslantless | $\eqslantgtr$  \eqslantgtr   | $\fallingdotseq$  \fallingdotseq         | $\Bumpeq$  \Bumpeq                         |
| $\leqq$  \leqq               | $\geqq$  \geqq               | $\eqcirc$  \eqcirc                       | $\thicksim$  \thicksim                     |
| $\lll$  \lll                 | $\ggg$  \ggg                 | $\circeq$  \circeq                       | $\thickapprox$  \thickapprox               |
| $\lesssim$  \lesssim         | $\gtrsim$  \gtrsim           | $\backsimeq$  \backsimeq                 | $\approxeq$  \approxeq                     |
| $\lessapprox$  \lessapprox   | $\gtrapprox$  \gtrapprox     | $\backsim$  \backsim                     | $\vDash$  \vDash                           |
| $\lessgtr$  \lessgtr         | $\gtrless$  \gtrless         | $\Vvdash$  \Vvdash                       | $\Vdash$  \Vdash                           |
| $\lesseqgtr$  \lesseqgtr     | $\gtreqless$  \gtreqless     | $\backepsilon$  \backepsilon             | $\varpropto$  \varpropto                   |
| $\lesseqqgtr$  \lesseqqgtr   | $\gtreqqless$  \gtreqqless   | $\between$  \between                     | $\pitchfork$  \pitchfork                   |
| $\preccurlyeq$  \preccurlyeq | $\succcurlyeq$  \succcurlyeq | $\vartriangleleft$  \vartriangleleft     | $\trianglelefteq$  \trianglelefteq         |
| $\curlyeqprec$  \curlyeqprec | $\curlyeqsucc$  \curlyeqsucc | $\vartriangleright$  \vartriangleright   | $\trianglerighteq$  \trianglerighteq       |
| $\precsim$  \precsim         | $\succsim$  \succsim         | $\blacktriangleleft$  \blacktriangleleft | $\blacktriangleright$  \blacktriangleright |
| $\precapprox$  \precapprox   | $\succapprox$ \succapprox    | $\therefore$  \therefore                 | $\because$  \because                       |
| $\subseteqq$  \\subseteqq    | $\supseteqq$  \\supseteqq    | $\shortmid$  \\shortmid                  | $\shortparallel$  \\shortparallel          |
| $\Subset$  \Subset           | $\Subset$  \\Subset          | $\smallsmile$  \\smallsmile              | $\smallfrown$  \\smallfrown                |

### AMS 箭头

| $\dashleftarrow$  \dashleftarrow       | $\leftarrowtail$  \leftarrowtail         | $\circlearrowleft$  \circlearrowleft   | $\rightleftarrows$  \rightleftarrows          |
| -------------------------------------- | ---------------------------------------- | -------------------------------------- | --------------------------------------------- |
| $\leftleftarrows$  \leftleftarrows     | $\leftrightharpoons$  \leftrightharpoons | $\circlearrowright$  \circlearrowright | $\Rrightarrow$   \Rrightarrow                 |
| $\leftrightarrows$  \leftrightarrows   | $\Lsh$  \Lsh                             | $\curvearrowright$  \curvearrowright   | $\twoheadrightarrow$  \twoheadrightarrow      |
| $\Lleftarrow$  \Lleftarrow             | $\looparrowleft$  \looparrowleft         | $\looparrowright$  \looparrowright     | $\rightarrowtail$  \rightarrowtail            |
| $\twoheadleftarrow$  \twoheadleftarrow | $\curvearrowleft$  \curvearrowleft       | $\Rsh$  \Rsh                           | $\rightleftharpoons$   \rightleftharpoons     |
| $\rightrightarrows$  \rightrightarrows | $\multimap$  \multimap                   | $\rightsquigarrow$  \rightsquigarrow   | $\leftrightsquigarrow$   \leftrightsquigarrow |
| $\dashrightarrow$  \dashrightarrow     | $\upuparrows$  \\upuparrows              | $\downdownarrows$  \\downdownarrows    |                                               |
| $\upharpoonleft$  \upharpoonleft       | $\upharpoonright$  \\upharpoonright      | $\downharpoonleft$  \\downharpoonleft  | $\downharpoonright$  \\downharpoonright       |

### AMS二元否定关系符和箭头

| $\nless$ \nless             | $\ngtr$ \ngtr               | $\varsubsetneqq$ \varsubsetneqq     | $\ncong$ \ncong                       |
| --------------------------- | --------------------------- | ----------------------------------- | ------------------------------------- |
| $\lneq$ \lneq               | $\gneq$ \gneq               | $\varsupsetneqq$ \varsupsetneqq     | $\nvdash$ \nvdash                     |
| $\nleq$ \nleq               | $\ngeq$ \ngeq               | $\nsubseteqq$ \nsubseteqq           | $\nvDash$ \nvDash                     |
| $\nleqslant$ \nleqslant     | $\ngeqslant$ \ngeqslant     | $\nsupseteqq$ \nsupseteqq           | $\nVdash$ \nVdash                     |
| $\lneqq$ \lneqq             | $\gneqq$ \gneqq             | $\nmid$ \nmid                       | $\nVDash$ \nVDash                     |
| $\lvertneqq$ \lvertneqq     | $\gvertneqq$ \gvertneqq     | $\nparallel$ \nparallel             | $\ntriangleleft$ \ntriangleleft       |
| $\nleqq$ \nleqq             | $\ngeqq$ \ngeqq             | $\nshortmid$ \nshortmid             | $\ntriangleright$ \ntriangleright     |
| $\lnsim$ \lnsim             | $\gnsim$ \gnsim             | $\nshortparallel$ \nshortparallel   | $\ntrianglelefteq$ \ntrianglelefteq   |
| $\lnapprox$ \lnapprox       | $\gnapprox$ \gnapprox       | $\nsim$ \nsim                       | $\ntrianglerighteq$ \ntrianglerighteq |
| $\nprec$ \nprec             | $\nsucc$ \nsucc             | $\nleftrightarrow$ \nleftrightarrow | $\nLeftrightarrow$ \nLeftrightarrow   |
| $\npreceq$ \npreceq         | $\nsucceq$ \nsucceq         | $\nleftarrow$ \nleftarrow           | $\nrightarrow$ \nrightarrow           |
| $\precneqq$ \precneqq       | $\succneqq$ \succneqq       | $\nLeftarrow$ \nLeftarrow           | $\nRightarrow$ \nRightarrow           |
| $\precnsim$ \precnsim       | $\succnsim$ \succnsim       | $\varsubsetneq$ \varsubsetneq       | $\varsupsetneq$ \varsupsetneq         |
| $\precnapprox$ \precnapprox | $\succnapprox$ \succnapprox | $\nsubseteq$ \nsubseteq             | $\nsupseteq$ \nsupseteq               |
| $\subsetneq$ \subsetneq     | $\supsetneq$ \supsetneq     | $\subsetneqq$ \subsetneqq           | $\supsetneqq$ \supsetneqq             |

### AMS二元运算符及其他符号

| $\dotplus$  \dotplus               | $\centerdot$  \centerdot             | $\intercal$  \intercal                   | $\hbar$  \hbar                 |
| ---------------------------------- | ------------------------------------ | ---------------------------------------- | ------------------------------ |
| $\ltimes$  \ltimes                 | $\rtimes$  \rtimes                   | $\divideontimes$  \divideontimes         | $\square$  \square             |
| $\Cup$  \Cup                       | $\Cap$  \Cap                         | $\smallsetminus$   \smallsetminus        | $\vartriangle$  \\vartriangle  |
| $\veebar$  \veebar                 | $\barwedge$  \barwedge               | $\doublebarwedge$  \doublebarwedge       | $\lozenge$  \lozenge           |
| $\boxplus$  \boxplus               | $\boxminus$  \boxminus               | $\circleddash$  \circleddash             | $\angle$   \angle              |
| $\boxtimes$  \boxtimes             | $\boxdot$  \boxdot                   | $\circledcirc$  \circledcirc             | $\diagup$  \diagup             |
| $\leftthreetimes$  \leftthreetimes | $\rightthreetimes$  \rightthreetimes | $\circledast$  \circledast               | $\nexists$  \nexists           |
| $\curlyvee$  \curlyvee             | $\curlywedge$  \curlywedge           | $\hslash$  \hslash                       | $\eth$  \eth                   |
| $\blacksquare$  \blacksquare       | $\blacktriangle$  \blacktriangle     | $\blacktriangledown$  \blacktriangledown | $\blacklozenge$  \blacklozenge |
| $\measuredangle$  \measuredangle   | $\diagdown$  \diagdown               | $\Finv$  \Finv                           | $\mho$  \mho                   |
| $\Bbbk$  \Bbbk                     | $\circledS$  \circledS               | $\complement$  \complement               | $\Game$  \Game                 |
| $\bigstar$  \bigstar               | $\sphericalangle$  \sphericalangle   | $\backprime$  \backprime                 | $\varnothing$  \varnothing     |

### 数学字母类

罗马字体 roman

```latex
$\mathrm{ABCdef}$
```

$$
\mathrm{ABCdef}
$$

斜体

```latex
$\mathit{ABCdef}$
```

$$
\mathit{ABCdef}
$$

美术字体-书法字体

```latex
$\mathcal{ABCdef}$
```

$$
\mathcal{ABCdef}
$$

花体 `\usepackage{mathrsfs}`

```latex
$\mathscr{ABCdef}$
```

$$
\mathscr{ABCdef}
$$

德文尖角体 `\usepackage{amssymb}`

```latex
$\mathfrak{ABCdef}$
```

$$
\mathfrak{ABCdef}
$$

黑板粗体 `\usepackage{amssymb}`

```latex
$\mathbb{ABCdef}$
```

$$
\mathbb{ABCdef}
$$

打字机体

```latex
$\mathtt{ABCdef}$
```

$$
\mathtt{ABCdef}
$$

无衬线

```latex
$\mathsf{ABCdef}$
```

$$
\mathsf{ABCdef}
$$

打字机体

```latex
$\mathtt{ABCdef}$
```

$$
\mathtt{ABCdef}
$$

# 常用特殊符号


$\ell$ (\ell) ：区分 l 和 1

$\Re$ (\Re) ：复数实部

$\Im$（\Im）：复数虚部

$\nabla$ (\nabla) ： 拉普拉斯算子

$\mathcal{L}$（\mathcal{L}）：机器学习损失函数

$\mathcal{N}$（\mathcal{N}）：高斯分布

$\mathbb{N}$（\mathbb{N}）：自然数

# Python 中输出 latex 特殊符号

介绍在 notebook 中输出 latex 符号

```python
%%latex
$\mathscr{a}$
```

```python
from IPython.display import Latex, display, Math

for i in range(97, 123):
    display(Latex(r"$\mathscr{{{0}}}$".format(chr(i))))
#    display(Math(r"$\mathscr{{{0}}}$".format(chr(i))))
```

**注释与说明**

1. 本篇为参考[常用数学符号的 LaTeX 表示方法](http://mohu.org/info/symbols/symbols.htm)总结的LaTeX常用数学运算符号和字母符号集。

2. 网页版显示效果跟浏览器有关系，但是，对于用LaTeX写文章都是一样的显示效果。

# 参考链接

1. [常用数学符号的 LaTeX 表示方法](http://mohu.org/info/symbols/symbols.htm)
2. [LaTeX —— 特殊符号与数学字体](https://blog.csdn.net/lanchunhui/article/details/54633576)
3. [Latex(数学)](https://www.cnblogs.com/MTandHJ/p/10527926.html)