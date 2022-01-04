---
title: Ubuntu卸载cuda
date: 2020-02-20 12:46:25
tags:
- cuda
- ubuntu
categories: technology
---

当使用Ubuntu运行深度学习模型时，有时候会出现一些错误，有可能是因为cuda版本不匹配或较旧导致的，所以需要我们卸载Ubuntu系统安装的cuda驱动，重新安装，下面一些代码是以Ubuntu18.04为例来卸载cuda

<!--more-->

```bash
sudo apt --purge remove cuda
sudo apt autoremove
sudo apt --purge remove cuda*
```

有时，还需要进入路径/usr/local，并删除文件夹cuda

```bash
rm -rf /usr/local/cuda*
```

至此，就完全卸载了cuda。