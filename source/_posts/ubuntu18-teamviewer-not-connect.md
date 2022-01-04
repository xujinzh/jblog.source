---
title: 重启Ubuntu后Teamviewer无法连接问题
date: 2020-02-22 11:01:47
tags:
- ubuntu
- teamviewer
categories: technology
---

在使用 teamviewer 连接 Ubuntu18.04 桌面系统时，总是出现重启 Ubuntu 系统后，通过 teamviewer 连接不上的问题。但是，当登录上 Ubuntu 系统后，发现可以用 teamviewer 连接。（After reboot, I cannot connect via Teamviewer. Following manual login, Teamviewer is able to connect at the login screen until the next reboot.）

<!--more-->

这是因为，teamviewer 服务需要用户登陆系统后才能打开，因此，可以通过修改配置让 teamviewer 服务自动打开，这样就可以再重启 Ubuntu 系统后，直接用 teamviewer 连接。具体的

```bash
sudo vim /etc/gdm3/custom.conf
```

把

```shell
WaylandEnable=false
```

注销掉

然后，重启系统

```bash
sudo reboot
```

重启 Ubuntu 系统后，就可以使用 teamviewer 进行连接。