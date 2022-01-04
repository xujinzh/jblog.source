---
title: 在 Python 中使用 importlib 和 getattr 动态调用函数
mathjax: true
author: Jinzhong Xu
date: 2021-12-17 10:34:07
tags:
- python
categories:
- technology
- python
---

Python 是一种动态编程语言。在编写 Python 代码时利用 importlib 能够动态的导入需要的模块，利用 getattr 能够获取模块的属性或方法，返回属性值和函数。本篇对它们进行介绍，并给出一个例子展示如何使用它们。

<!--more-->

# importlib

importlib 是一个模块，使用时需要首先导入该模块。

```python
import importlib
```

导入模块的方法

```python
expr_module = importlib.import_module("mymodule.test")
```

# getattr

getattr 是内置函数，用于返回一个对象属性值。

如，编写一个类：

```python
class Test:
    def __init__(self):
        self.name = 'test'
```

获取实例的属性值

```python
t = Test()
getattr(t, 'name')
```

结果

```python
'test'
```

# 一个例子

编写模块 primes.py，放到当前目录的 pyscripts 文件夹下

```python
from numba import jit


@jit(nopython=True)
def isPrime(n: int) -> str:
    """
    判断给定的n是否为素数
    """
    # 只需要用n除以2 ~ n/2 + 1 是否能整除
    # 如果有整数整除n，则n不是素数
    # 如果所有的都不能整数n，则n是素数
    for i in range(2, n // 2 + 1):
        # 如果余数是0，则表示整除，不是素数，直接返回0
        if not (n % i):
            return f"{n}不是素数，是合数"
    # 如果所有的数都不能整除n，则说明n是素数，则返回1
    return f"{n}是素数"
```

调用模块，并使用模块的方法（在 notebook 中演示）

```python
import importlib

# 导入模块
expr_module = importlib.import_module('pyscripts.primes')
# 获取模块的方法
expr_func = getattr(expr_module, "isPrime")
# 使用该方法，判断数值 158963 是否是素数
expr_func(158963), expr_func(158863)
```

结果如下

```python
('158963不是素数，是合数', '158863是素数')
```

