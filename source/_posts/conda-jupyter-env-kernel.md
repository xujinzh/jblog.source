---
title: Conda 配置其他虚拟环境 jupyter kernel
author: Jinzhong Xu
mathjax: true
date: 2020-03-25 21:32:19
tags:
- conda
- python
- kernel
- jupyter
categories: 
- technology
- python
---

Miniconda 是一个 python 简约发行版，集成了 python、pip、conda 等，利用 conda 进行 python 软件包管理非常方便。另外，也可以为特定的项目创建特定的运行环境，再结合 jupyter 等能够非常方便切换各种独立环境。下面给出如何利用 conda 创建特定环境。

<!--more-->

# 安装 Miniconda

从官网下载 miniconda 并安装

```bash
# 安装最新版 miniconda
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
chmod +x Miniconda3*
./Miniconda3*
```

# 安装特定环境

创建新的环境，命名为 toplayer，安装 python 版本 3.8

```bash
# 如果安装 miniconda 时没有初始化，需要初始化，重新登陆
conda init
# 建议安装 jupyterlab，这样在虚拟环境中就可以不用安装了
pip install jupyterlab
```

创建虚拟环境

```bash
# 安装虚拟环境
conda create -n toplayer python=3.8
# 激活虚拟环境
conda activate toplayer
# 退出虚拟环境
conda deactivate
```

<font size=3 color=cyan>删除虚拟环境</font>

```
# 删除虚拟环境
conda remove -n toplayer --all
```

注意，conda 版本要求 4.6+

列出所有安装的环境

```bash
# 在主环境下
conda env list
# 或者
conda info -e
```

# 安装  jupyter kernel

在虚拟环境安装需要的软件

```bash
# 首先激活虚拟环境
conda activate toplayer

# 安装需要软件
conda install ipython jupyterlab
# 如果在 base 主环境中已经安装了 jupyterlab，那么可以只需要安装 ipykernel
pip install ipykernel

# 在虚拟环境安装 jupyter kernel，并命名
python -m ipykernel install --user --name toplayer --display-name "Python3(toplayer)"

# 在主环境中打开 jupyterlab
conda deactivate
# 注意这里尝试退出虚拟环境，并进入到主环境，主环境以 base 显示，如果没有显示任何环境名，可以使用如下命令进入主环境
conda activate
# 打开 jupyterlab
jupyterlab
```

此时，刷新 jupyter notebook 就可以从 kernel 中看到名为 Python3(toplayer) 的 kernel

#  卸载不想要的 jupyter kernel

查看有哪些 jupyter kernel

```bash
jupyter kernelspec list
```

显示结果如下（示例）

```bash
Available kernels:
  perslay     /home/jinzhongxu/.local/share/jupyter/kernels/perslay
  toplayer    /home/jinzhongxu/.local/share/jupyter/kernels/toplayer
  python3     /usr/local/miniconda/share/jupyter/kernels/python3
  c           /usr/local/share/jupyter/kernels/c
```

删除不需要的 kernel

```bash
jupyter kernelspec uninstall toplayer
jupyter kernelspec uninstall perslay
jupyter kernelspec uninstall c
```

# 把包版本号写入文件

```bash
python -m pip freeze > requirements.txt
```

# 从环境文件安装特定版本的包

```bash
python -m pip install -r requirements.txt
```

# 参考链接

1. [Adding An Environment to Jupyter Notebooks](http://echrislynch.com/2019/02/01/adding-an-environment-to-jupyter-notebooks/)

2. [用conda创建python虚拟环境](https://blog.csdn.net/lyy14011305/article/details/59500819)

3. [pip freeze](https://pip.pypa.io/en/stable/cli/pip_freeze/)

   