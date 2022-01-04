---
title: JupyterLab 中 Pandas 打印行列省略问题
mathjax: true
author: Jinzhong Xu
date: 2021-10-20 09:58:11
tags:
- jupyter
- jupyterlab
- python
- pandas
categories:
- technology
- python
---

在 JupyterLab 中书写 Python 代码非常的方便，交互性强，并且能够书写 Markdown 文档等。但是，使用其处理 Pandas 的 DataFrame 对象时，当表格数据行或列太大时，打印时总是显示不全，在中间出现一些省略号，这使得想要查看所有内容时不是太方便，下面给出一种方法，能够显示所有的行和列。

<!--more-->

# 解决方案

在 JupyterLab 的第一个单元格内，添加如下内容：

```python
import pandas as pd

# 显示所有列
pd.set_option('display.max_columns', None)
# 显示所有行
pd.set_option('display.max_rows', None)
# 设置 value 的显示长度为 100，默认为 50
pd.set_option('max_colwidth',100)
```

在 JupyterLab 中使用如下命令，可以查看所有可自定义参数

```python
pd.set_option?
```



# 参考链接

1. [pandas中关于DataFrame行，列显示不完全（省略）的解决办法](https://blog.csdn.net/weekdawn/article/details/81389865)

