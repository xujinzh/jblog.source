---
title: 为 Conda 设置可用源
author: Jinzhong Xu
mathjax: true
date: 2020-05-27 16:35:01
tags:
- python
- conda
categories: technology
---

Python 是一个非常优秀的编程语言，特别是在数据科学和人工智能领域。对于从事人工智能，尤其是深度学习方向研究的科研人员、学生及其他爱好者来说，Python 是一款神器，这主要是归功于 Python 的各种功能包，如 Numpy, Sympy, Matplotlib, Pandas, Scikit-learn, Tensorflow, Pytorch 等，那么如何有效的管理（安装、删除、更新升级、降级到特定版本等）这些包非常的重要。

<!--more-->

Python 常用的包管理工具有 Pip 和 Conda 等。

Pip 是标准的 Python 包管理系统，用于安装和管理由 Python 编写的软件包，大多数包能够从默认的包及其依赖项源（PyPI, [Python Package Index](https://pypi.org/)）中找到。

Conda 是一个多功能软件包管理软件，它不仅可以管理 Python 包，也可以管理 R 语言包等，因实际使用中发现 Pip 安装包是总是出现不兼容，安装包失败问题，所以建议使用 Conda 来管理 Python 包。同时，建议使用 Miniconda 或 Ananconda 来安装 Python 和常用依赖包。这会比从 Python 官网安装更加方便。

使用 Conda 来管理 Python 包出现的一个问题就是在国内安装时容易出现 HTTP ERROR，那是因为它的源在国外服务器上，容易导致连接超时问题。一种有效的解决方法就是为 Conda 添加国内源，目前，比较常用的国内源包括[清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/) 和 中国科学技术大学开源软件镜像，在 Anaconda 一项中发现后者其实是指向前者的，所以这里以清华大学开源软件镜像源为例，通过实践测试来一步一步配置 Conda 国内镜像源，使得 Conda 管理安装 Python 包顺利进行。

# 国内源添加方法

## 查看 Conda 配置项

```sh
conda config --show
```

可以观察 "channels:" 一项，默认为 "- defaults"

## 添加清华源

```shell
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
conda config --set show_channel_urls yes

# 或者使用如下方法：
# https://tuna.moe/oh-my-tuna/
# https://github.com/tuna/oh-my-tuna
wget https://tuna.moe/oh-my-tuna/oh-my-tuna.py
# For yourself
python oh-my-tuna.py
# ...or for everyone!
sudo python oh-my-tuna.py --global
# Get some help
python oh-my-tuna.py -h
```

推荐添加 "main" 和 "cloud/conda-forge"，不推荐 "free"（如下），因为 "main" 比 "free" 更新更完善更全，"cloud/conda-forge" 对应 Anaconda 的 conda-forge  ，包含一些 conda 中没有的第三方软件包。但是，实际测试发现清华源里的 conda-forge 更新慢于 Anaconda.

```shell
conda config --remove channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
```



## 删除默认源

添加完清华源后，可以使用如下命令查看

```shell
vim ~/.condarc
```

此时，发现"defaults" 仍然存在，而且是在最后一个，当使用 Conda 安装更新 Python 包时，发现仍然出现 HTTP ERROR，因为没有删除 "defaults" 的缘故。删除方法有两种，一种是直接在文件 "~/.condara" 中删除，一种是使用如下命令

```shell
conda config --remove channels defaults
```

也可以使用命令

```shell
conda config --remove-key channels
```

删除全部源。

同样，以上删除默认源的方法也适合与清华源的删除。

## 使用 Conda

```shell
conda update --all
```

# 配置代理使用默认源

虽然切换到国内源比较快，但是，国内源更新教迟缓一些，对于某些包不能及时更新，这时，可以配置代理，使用默认源

```shell
vim ~/.condarc
# 添加或修改成如下内容
channels:
  - https://repo.anaconda.com/pkgs/main
  - conda-forge
show_channel_urls: true
proxy_servers:
  http: socks5://127.0.0.1:1080
  https: socks5://127.0.0.1:1080
```

此时，可以直接使用conda的默认源安装最新的包了。

# 参考链接

1. [Using default repositories](https://docs.anaconda.com/anaconda/user-guide/tasks/using-repositories/)
2. [Configure conda for use behind a proxy server (proxy_servers)](https://docs.conda.io/projects/conda/en/latest/user-guide/configuration/use-condarc.html#config-proxy)