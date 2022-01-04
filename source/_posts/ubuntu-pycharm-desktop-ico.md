---
title: Ubuntu上设置Pycharm快捷启动
date: 2020-02-22 12:18:32
tags:
- ubuntu
- pycharm
categories: technology
---

在使用 Ubuntu 进行 python 编程时，Pycharm 是一个非常方便的开发工具，但是，首次从官网下载并安装 Pycharm时，必须使用命令行运行 Pycharm。这在下次使用或启动 Pycharm 时非常不方便。最好能够在状态栏上或启动界面上能够保留或搜索到 Pycharm 图标。如何设置呢，按照如下步骤即可实现。

<!--more-->

# 1. 首先在命令行第一次启动Pycharm

```bash
./pycharm.sh
```

# 2. 将Pycharm添加到桌面

1. 在 Pycharm 中找到 Tools
2. 在 Tools 中点击 Create Desktop Entry

到此，即实现了启动界面上搜索到 Pycharm 图标。

# 3. 将Pycharm添加到状态栏

1. 从启动界面，搜索 Pycharm 图标，并点击启动
2. 在状态栏点击右键，选择 Add to Favorates

到此，即实现了在状态栏上保留 Pycharm 启动图标

如果喜欢在终端运行Pycharm来启动，那么，可以将 pycharm.sh 连接到环境变量可以搜索的地方，方便启动，具体的

```bash
sudo ln -s /home/jayzonxu/Documents/pycharm-community-2019.3.3/bin/pycharm.sh /usr/bin/pycharm
```

在终端中，输入 pycharm，即可启动