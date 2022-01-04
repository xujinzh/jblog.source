---
title: 设置 Jupyter Cell Proxy
mathjax: true
author: Jinzhong Xu
date: 2021-08-31 21:18:01
tags:
- python
- jupyter
- proxy
categories:
- technology
- python
---

Jupyter Notebook/Lab 提供了交互式的 Python 等编程范式，下面介绍一种设置 Proxy 的方法，方便 Python 代码的运行。

<!--more-->

只需要在 Jupyter Notebook/Lab 第一个 Cell 上添加如下代码即可：

```python
# 导入模块
import os
# 设置代理
proxy = 'http://127.0.0.1:1080'
os.environ['http_proxy'] = proxy
os.environ['HTTP_PROXY'] = proxy
os.environ['https_proxy'] = proxy
os.environ['HTTPS_PROXY'] = proxy
```

