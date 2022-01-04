---
title: 在 Ubuntu 上挂载 ISO 文件
mathjax: true
author: Jinzhong Xu
date: 2021-07-27 10:24:16
tags:
- ubuntu
- iso
- mount
categories:
- technology
- ubuntu
---

我这里需要安装 MATLAB，但是下载的软件是 ISO 文件，无法直接读取，需要挂在到 Ubuntu 上才能读取，因此，使用如下方法挂载和卸载 ISO 文件

<!--more-->

# 创建挂在点文件夹

```bash
sudo mkdir /media/iso
```

# 挂载 ISO 文件到挂载点

```bash
sudo mount -o loop /home/jinzhongxu/Matlab98R2020a_Lin64.iso /media/jinzhongxu
```

# 卸载

```bash
sudo umount /media/iso
```

# 参考链接

1. [How to mount an ISO file?](https://askubuntu.com/questions/164227/how-to-mount-an-iso-file)

