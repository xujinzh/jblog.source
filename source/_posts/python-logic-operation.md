---
title: Python 逻辑运算
mathjax: true
author: Jinzhong Xu
date: 2021-05-29 20:50:28
tags:
- python
- logic
categories:
- technology
- python
---

Python 的逻辑运算符包括 and, or, not，但位运算符 & (按位与运算符), | (按位或运算符) 有时也可以达到 and, or 的效果，有时，只有位运算符才能完成，如 pandas 中。下面进行总结，主要介绍 and, or 和 &, |.
<!--more-->

# 基本介绍

离散数学中，假设 p 和 q 都是命题，那么

p 且 q: 当 p 和 q 都取值为真时结果才为真；

p 或 q:    当 p 和 q 都取值为假时结果才为假；

# Python 逻辑运算

## 命题为逻辑变量时

当命题为逻辑变量时，即取值为 True, False 布尔值时，and, or 和 &, | 相对应的运算符用法基本一致，除了在pandas中。

也就是说，以下的表达式结果都为 True

```python
(True & True) == (True and True)
(True & False) == (True and False)
(False & True) == (False and True)
(False & False) == (False and False)

(True | True) == (True or True)
(True | False) == (True or False)
(False | True) == (False or True)
(False | False) == (False or False)
```

<font size=4 color=cyan>但在 pandas 中，下面情况只能使用 & 和 ｜，注意不能使用 and 或 or</font>，不然报错。

```python
import pandas as pd
df = pd.DataFrame({"a": [1, 2, 3], "b": [3, 4, 5]})
df[(df.a > 1) & (df.a < 3)]
> 	a	b
 1	2	4
```

## 命题为数值变量时

当命题为数值变量时，即命题取值为整数值，如 2， 3 等，此时 & 和 | 是位运算符，如下结果都为 True

```python
2 & 4 == 0
2 | 4 == 6
bin(2) == "0b10"
bin(4) == "0b100"
```

但 and 和 or 仍然表示逻辑运算，它把非 0 整数当成真，0 或 False 当假，如下结果都为 True

```python
(0 and 3) == 0
(2 and 3) == 3
(3 or 0) == 3
(0 or 2) == 2
# and 遇到 False或0 就返回
# or 遇到 True或非0 就返回
# 因为 and 和 or 都是短路操作符（short-circuitlogic）或者惰性求值（lazy evaluation）
# 它们的参数从左向右解析，一旦结果可以确定就停止
```

## 命题中既有逻辑变量又有数值变量

当左右命题中既有逻辑变量又有数值变量时，逻辑变量自动类型转化为数值变量的  0 或 1，False 转化为 0, True 转化为 1，如下结果都为 True，但注意因为惰性求值，结果可能是第一个命题的类型

```python
(False and 3) == False
(False or 3) == 3
(True and 3) == 3
(True or 3) == True
```

也可以这样理解：

1. <font size=5 color=red> 对于 and 当第一个命题为假时返回第一个命题值；当第一个命题为真时返回第二个命题值</font>；
2. <font size=5 color=orange>对于  or  当第一个命题为真时返回第一个命题值；当第一个命题为假时返回第二个命题值</font>。

# 注意运算优先级

因为逻辑运算的优先级比较低，因此<font size=4 color=cyan>建议对命题加上小括号</font>，这样逻辑上比较清晰，不易出现意想不到的结果，如

```python
(0 and 3) == 0
> True
0 and 3 == 0
> 0
```



# 参考链接

1. [Python 中 （&，|）和（and，or）之间的区别](https://blog.csdn.net/weixin_40041218/article/details/80868521)

2. [Python中的逻辑运算符‘and’、‘or’和‘not’](https://blog.csdn.net/lqzdreamer/article/details/77171255)

3. [Python逻辑运算符及其用法](http://c.biancheng.net/view/2186.html)

4. [Python 运算符](https://www.runoob.com/python/python-operators.html#ysf4)

   
