---
title: 在 Jupyter 中使用 Matlab
mathjax: true
author: Jinzhong Xu
date: 2021-07-27 13:22:38
tags:
- matlab
- jupyter
categories:
- technology
- python
---

Jupyter 为创建交互式脚本提供了一个强大且广泛的平台，在这里介绍一种在 Jupyter 中调用 Matlab 的方法。假设已经安装了 Miniconda 或者 Anaconda， Matlab 版本是 2020a，安装在路径：`/usr/local/MATLAB` 下。

<!--more-->

# 创建虚拟环境

因为 Matlab 2020a 对用的 Python 版本最高为 3.7，所以这里安装对于版本，虚拟环境名字命名为 jmatlab

```bash
conda create -vv -n jmatlab python=3.7 jupyter
```

# 激活 jmatlab 环境

```bash
conda activate jmatlab
```

# 安装 jupyterlab 和 matlab 内核

```bash
pip install jupyterlab
pip install matlab_kernel
python -m matlab_kernel install
```

# 查看安装的内核

```bash
jupyter kernelspec list
```

您应该在可用内核列表中看到 Matlab.

# 安装 Matlab 引擎

```bash
cd /usr/local/MATLAB/extern/engines/python
python setup.py install
```

这将允许从 Python 会话中调用 Matlab 引擎。

# 重启 jupyterlab

```bash
sudo systemctl restart jupyterhub.service
```

# 参考链接

1. [Using Jupyter Notebooks and JupyterLab with Matlab](http://www.jmlilly.net/jupyter-matlab)

   
