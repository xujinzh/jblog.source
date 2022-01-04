---
title: Linux 终端显示配置
date: 2019-12-20 20:05:18
tags:
- linux
- terminal
- shell
categories: technology
---

Linux 终端配置成比较突出的颜色，有助于快速定位和美化终端，下面是配置个人比较喜欢的终端显示的方法，以Ubuntu为例：

<!--more-->

## 1, 打开终端配置文件

```bash
vim ~/.bashrc
```
## 2, 把一下代码追加到文件中

```bash
export PS1="\[\033[36m\]\u\[\033[m\]@\[\033[32m\]\h:\[\033[33;1m\]\w\[\033[m\]\$ "
export CLICOLOR=1
export LSCOLORS=ExFxBxDxCxegedabagacad
```
## 3, 保存文件，并运行如下代码使设置生效

```bash
source ~/.bashrc
```

