---
title: JupyterLab 中单元格同时能够输出多行的方法
mathjax: true
author: Jinzhong Xu
date: 2021-10-20 10:10:28
tags:
- python
- jupyter
- jupyterlab
categories:
- technology
- python
---

在 JupyterLab 中书写 Python 代码非常的方便，交互性强，并且能够书写 Markdown 文档等。但是，使用其进行多行打印输出时，要么使用 print 函数，要么把需要输出的参数都写在最后一行，这大大降低了其交互性。下面给出一种解决方法。

<!--more-->

# 解决方案 1

每次在 JupyterLab 的第一行添加如下代码

```python
from IPython.core.interactiveshell import InteractiveShell

InteractiveShell.ast_node_interactivity = "all"
```

# 解决方案2

在配置文件中修改，这样不用每次在 JupyterLab 首行都添加代码

1. 终端输入如下命令，创建 ipython_config.py 文件

   ```bash
   vim ~/.ipython/profile_default/ipython_config.py
   ```

   

2. 在文件中写入如下内容

   ```bash
   c = get_config()
   c.InteractiveShell.ast_node_interactivity = "all"
   ```

   

3. 使其生效

   ```bash
   source ~/.ipython/profile_default/ipython_config.py
   ```

   

# 参考链接

1. [Jupyter cell同时输出多行](https://blog.csdn.net/enter89/article/details/90633718)

