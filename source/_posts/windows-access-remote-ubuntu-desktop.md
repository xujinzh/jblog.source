---
title: Windows 远程连接 Ubuntu 桌面
mathjax: true
author: Jinzhong Xu
date: 2021-07-02 14:30:20
tags:
- xrdp
- windows
- ubuntu
- remote
categories:
- technology
- windows
---

想从 Windows 10 远程桌面访问 Ubuntu，通过使用 xrdp 来达成这一目的。

<!--more-->

# Ubuntu 上安装软件

安装远程桌面协议 (RDP) 服务器 xrdp

```bash
sudo apt install xrdp
```

运行远程桌面共享服务器xrdp

```bash
sudo systemctl enable --now xrdp
```

为传入流量打开防火墙端口 3389

```bash
sudo ufw allow from any to any port 3389 proto tcp
```

# Windows上远程连接

移至 Windows 10 主机并打开远程桌面连接客户端。使用搜索框搜索远程关键字，然后单击打开按钮。

输入 Ubuntu 的 IP 地址和端口号 3389，验证连接

# 可能问题

在启动到 Xrdp 远程桌面协议 (RDP) 服务器的远程连接后，我有时会收到黑屏。虽然我不确定如何完全解决这个问题，但在建立远程连接之前从 Ubuntu 桌面注销至少暂时解决了这个问题。

# 参考链接

1. [Ubuntu 20.04 Remote Desktop Access from Windows 10](https://linuxconfig.org/ubuntu-20-04-remote-desktop-access-from-windows-10)

