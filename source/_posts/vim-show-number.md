---
title: vim 显示行号
author: Jinzhong Xu
date: 2020-03-06 11:24:46
tags:
- vim
- ubuntu
categories: technology
---

Vim 是Ubuntu系统及其他Linux系统比较常用的文件编辑工具，但是，默认系统上不会显示文本的行号，如何设置显示行号，可以按照如下的步骤进行，这里以Ubuntu18.04为例进行。

<!--more-->

# 拷贝vim配置文件到家目录

```bash
cp /etc/vim/vimrc ~/.vimrc
```

# 修改vim配置文件

```bash
vim ~/.vimrc
```

在末尾增加一行

```shell
set number
```

# 重新用vim打开文件

```bash
vim test 
```

即可发现已经显示行号