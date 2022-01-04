---
title: Pycharm 自动文件代码注释说明
author: Jinzhong Xu
mathjax: true
date: 2020-07-31 12:43:24
tags:
- pycharm
- python
categories: technology
---

PyCharm 是一款非常优秀的python代码开发工具，功能强大，特别是代码提示功能，非常好用。在我们开发python代码时，常常需要给代码文件添加说明注释，比如，在什么时候创建的代码、有谁开发、作者信息等等，下面给出一个个人喜好的格式

<!--more-->

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Author   : Jinzhong Xu
# @Contact  : jinzhongxu@csu.ac.cn
# @Time     : ${DATE} ${TIME}
# @File     : ${NAME}.py
# @Software : ${PRODUCT_NAME}

```

如何在PyCharm中设置呢？

以Windows上为例：

1. 打开PyCharm -> File -> Settings -> Editor -> File and Code Templates -> Python Script 

2. 然后添加上面的格式代码
3. 点击 Apply -> OK

