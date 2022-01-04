---
title: PyCharm 远程解释器配置
author: Jinzhong Xu
mathjax: true
date: 2020-08-10 15:33:11
tags:
- python
- pycharm
- ssh
categories: 
- technology
- python
---

PyCharm 是一款编写 Python 代码的优秀 IDE，特别是其代码提示功能和自动检查代码功能，当我们想要利用远程服务器的强大计算能力和方便的本地 PyCharm 开发工具时，需要配置 PyCharm 远程解释器，下面进行介绍

<!--more-->

# PyCharm 专业版安装

想使用 PyCharm 远程解释器功能，必须使用专业版，JET BRAINS 官网上可以免费使用30天专业版 PyCharm，到期后，可以购买，价格大概时2000元左右。

# 远程解释器配置

顺序如下

File -> Settings -> Project -> Python Interpreter -> 右上角添加 -> SSH Interpreter -> New server configuration -> next -> 配置密码等

# 上传本地代码

配置好远程解释器后，需要将本地新建的工程上传到远程服务器的某一个目录下，才能够运行，不然会出现找不到文件或目录，具体配置如下

Tools -> Deployment -> Configuration -> + -> SFTP -> Connection -> 填写自己的内容，一般不用修改 -> Mappings -> Local path(表示本地代码工程目录) -> Deployment path(远程服务器运行时代码工程目录) -> ok

# 运行代码

运行前，一般都需要将本地代码的修改更正到远程服务器上，才能够在远程服务器上运行代码，快捷键是

```bash
ctrl + shift + alt + x
```

如果觉得每次运行都要上传比较麻烦，可以如下设置，自动上传：

1. 在 Tools -> Deployment -> Configuration 里设置 默认服务器，打对号，也可右键设置
2. 在 Tools -> Deployment -> Options -> Upload changed files automatically to the default server -> 选择 Always

上传代码后，就可以运行了，快捷键是

```bash
ctrl + shift + f10
```

# 设置自动同步

当前面已经设置自动同步 (Always)，但是却没有自动同步或上传代码到服务器，可能是因为 Deployment 中没有设置默认服务器，设置后应该就可以了。

# 参考链接

1. [Pycharm设置了自动上传文件，为什么还是没有自动上传文件](https://blog.csdn.net/myt2000/article/details/103288535)
