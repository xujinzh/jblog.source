---
title: Python 调用 C 语言的函数
author: Jinzhong Xu
mathjax: true
date: 2020-08-19 15:32:08
tags:
- python
- c
categories:
- technology
- python
---

Python 是一个高级的、解释性的编程语言，其代码比较其他编程语言，如C、C++、Java 等简单，通俗更易懂，开发效率高，但运行效率较弱。其本身是利用C语言（C语言是为了编写Unix操作系统而被开发研究出来）开发的，因此，在Python中调用C函数、代码将是比较自然的。方法很多，这里介绍一个比较简单的方法。在哪些Python代码运行效率低下的地方，调用C语言实现的程序将会加速代码运行。除此之外，加速Python代码的方法还有，利用 numba 的 jit 或 njit 装饰器来加速数学计算比较多或循环比较多的地方。

<!--more-->

# 编写 C 代码

打开文本编辑器，敲入如下代码，并命名为 x_maths.c

```c
#include <stdio.h>
#include <math.h>

int square(int i) {
	return pow(i, 3);
}
```

# 编译为共享库文件

利用C编译器将上面的 x_maths.c 编译为共享库文件（shared library）x_maths.so，也可以写成其他名字，但必须以 .so 为扩展名

```shell
cc -fPIC -shared -o x_maths.so x_maths.c
```

注意，我这里在 Ubuntu 18.04 上运行

# Python 调用 C 函数

在 Python 程序中，从共享文件创建一个 ctypes.CDLL 实例，最后，使用 {CDLL_instance}.{function_name}（{function_parameters}） 格式调用 C 函数。我这里在 Jupyter lab 里编写代码如下

```python
from ctypes import *

so_file = "./x_maths.so"
my_function = CDLL(so_file)
print(type(my_function))
print(my_function.square(100))
```

运行结果如下

```python
<class 'ctypes.CDLL'>
1000000
```

# 结论

1. ctype 简单，且是 Python 标准库之一
2. ctypes 是低级实现，易于实现，但缺乏自动化，对于复杂任务将会变得较为繁琐