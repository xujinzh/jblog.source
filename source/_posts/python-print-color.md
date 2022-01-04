---
title: Python 打印颜色设置
mathjax: true
author: Jinzhong Xu
date: 2021-04-25 16:02:19
tags:
- python
- print
- color
categories:
- technology
- python
---

使用 Python 的 print 方法可以打印输出字符串，通过如下方法可以设置打印字符串的字体颜色和背景颜色。

<!--more-->

# 字体颜色

```python
print("\033[1;30m 字体颜色：白色\033[0m")
print("\033[1;31m 字体颜色：红色\033[0m")
print("\033[1;32m 字体颜色：深黄色\033[0m")
print("\033[1;33m 字体颜色：浅黄色\033[0m")
print("\033[1;34m 字体颜色：蓝色\033[0m")
print("\033[1;35m 字体颜色：淡紫色\033[0m")
print("\033[1;36m 字体颜色：青色\033[0m")
print("\033[1;37m 字体颜色：灰色\033[0m")
print("\033[1;38m 字体颜色：浅灰色\033[0m")
```

# 背景颜色

```python
print("背景颜色：白色   \033[1;40m    \033[0m")
print("背景颜色：红色   \033[1;41m    \033[0m")
print("背景颜色：深黄色 \033[1;42m    \033[0m")
print("背景颜色：浅黄色 \033[1;43m    \033[0m")
print("背景颜色：蓝色   \033[1;44m    \033[0m")
print("背景颜色：淡紫色 \033[1;45m    \033[0m")
print("背景颜色：青色   \033[1;46m    \033[0m")
print("背景颜色：灰色   \033[1;47m    \033[0m")
```

# Windows CMD 显示颜色方法

```python
import os
import platform
# 如果平台系统是 'Linux'，则不需要此设置
if platform.system() == 'Windows':
    os.system('color FF')
```



# 参考链接

1. [python print字体颜色 print背景颜色](https://blog.csdn.net/ever_peng/article/details/91492491) 