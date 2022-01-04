---
title: Mac Book Pro 更改 bash 为 zsh
date: 2019.12.21
tags:
- mbp
- shell
categories: technology
---

```bash
chsh -s /bin/zsh
# 有时 Centos 上没有自动安装工具 chsh，可使用如下方法安装
sudo yum install util-linux-user
```

安装 oh-my-zsh

```bash
sh -c "$(wget -O- https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

