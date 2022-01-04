---
title: 在 markdown 中书写矩阵
mathjax: true
author: Jinzhong Xu
date: 2020-11-24 11:47:48
tags:
- markdown
- matrix
categories:
- technology
---

使用 markdown 写文章非常方便，简洁。对于数学符号书写也是非常的高效，本篇主要介绍如何在 markdown 中书写矩阵。

<!--more-->

# 不带括号的矩阵

代码之后的 tag 实现了后标

```markdown
$$
\begin{matrix}
    1 & 2 & 3 \\\\
    4 & 5 & 6 \\\\
    7 & 8 & 9
\end{matrix} \tag{1}
$$
```

效果如下：
$$
\begin{matrix}
    1 & 2 & 3 \\\\
    4 & 5 & 6 \\\\
    7 & 8 & 9
\end{matrix} \tag{1}
$$


# 带括号 { \}​ 的矩阵

```markdown
$$
\begin{Bmatrix}
    1 & 2 & 3 \\\\
    4 & 5 & 6 \\\\
    7 & 8 & 9
\end{Bmatrix} \tag{2}
$$
```

效果如下
$$
\begin{Bmatrix}
    1 & 2 & 3 \\\\
    4 & 5 & 6 \\\\
    7 & 8 & 9
\end{Bmatrix} \tag{2}
$$


# 带括号 $[]$ 的矩阵

```markdown
$$
\left[
\begin{matrix}
    1 & 2 & 3 \\\\
    4 & 5 & 6 \\\\
    7 & 8 & 9
\end{matrix}
\right] \tag{3}
$$
```

效果如下：
$$
\left [
\begin{matrix}
    1 & 2 & 3 \\\\
    4 & 5 & 6 \\\\
    7 & 8 & 9
\end{matrix}
\right ] \tag{3}
$$



```markdown
$$
\begin{bmatrix}
    1 & 2 & 3 \\\\
    4 & 5 & 6 \\\\
    7 & 8 & 9
\end{bmatrix} \tag{4}
$$
```

效果：
$$
\begin{bmatrix}
    1 & 2 & 3 \\\\
    4 & 5 & 6 \\\\
    7 & 8 & 9
\end{bmatrix} \tag{4}
$$

# 带省略号的矩阵

```markdown
$$
\left[
\begin{matrix}
    1      & 2      & \cdots & 4      \\\\
    7      & 6      & \cdots & 5      \\\\
    \vdots & \vdots & \ddots & \vdots \\\\
    8      & 9      & \cdots & 0      \\\\
\end{matrix}
\right] \tag{5}
$$
```

效果如下：
$$
\left[
\begin{matrix}
    1      & 2      & \cdots & 4      \\\\
    7      & 6      & \cdots & 5      \\\\
    \vdots & \vdots & \ddots & \vdots \\\\
    8      & 9      & \cdots & 0      \\\\
\end{matrix}
\right] \tag{5}
$$


# 带参数的矩阵

这里笔者希望在矩阵中画出一条分割线，以强调最右侧一列的特殊性。
其中`\begin{array}{cc|c}`中的c表示居中对齐元素；

```markdown
$$ 
\left[
\begin{array}{cc|c}
    1 & 2 & 3 \\\\
    4 & 5 & 6
\end{array}
\right] \tag{6}
$$
```

效果如下：
$$
\left[
\begin{array}{cc|c}
    1 & 2 & 3 \\\\
    4 & 5 & 6
\end{array}
\right] \tag{6}
$$


# 行间矩阵

表示将矩阵写在一行文本之中，这样不会占用太多的篇幅

```markdown
二维矩阵
$\bigl(
\begin{smallmatrix}
l&l\\j&z
\end{smallmatrix}
\bigr)$
的行列式
```

效果如下：

二维矩阵 $\bigl(
    \begin{smallmatrix}
		l&l\\\\j&z
	\end{smallmatrix}
\bigr)$ 的行列式

# 行列式

```markdown
$$
\begin{vmatrix}
    1 & 2 & 3	\\\\
    4 & 5 & 6	\\\\
    7 & 8 & 9
\end{vmatrix} \tag{7}
$$
```

效果如下：
$$
\begin{vmatrix}
    1 & 2 & 3	\\\\
    4 & 5 & 6	\\\\
    7 & 8 & 9
\end{vmatrix} \tag{7}
$$

# 矩阵范数

```markdown
$$
\begin{Vmatrix}
    1 & 2 & 3	\\\\
    4 & 5 & 6	\\\\
    7 & 8 & 9
\end{Vmatrix} \tag{8}
$$
```

效果如下：
$$
\begin{Vmatrix}
    1 & 2 & 3	\\\\
    4 & 5 & 6	\\\\
    7 & 8 & 9
\end{Vmatrix} \tag{8}
$$


# 分段函数

```markdown
$$
P(x|Pa_x)=\begin{cases} 
    1, & x=f(Pa_{x})	\\\\ 
    0, & \text{other values}
\end{cases} \tag{9}
$$
```

效果如下：
$$
P(x|Pa_x)=\begin{cases} 
    1, & x=f(Pa_{x})	\\\\ 
    0, & \text{other values}
\end{cases} \tag{9}
$$

# 参考链接

1.  [使用Markdown写矩阵](https://blog.csdn.net/qq_38228254/article/details/79469727)
2. [使用Markdown写矩阵、表格和一些数学公式(实用)](https://blog.csdn.net/nuoyanli/article/details/96179976)