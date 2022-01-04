---
title: Windows10 子系统开机自启动SSH等服务
author: Jinzhong Xu
mathjax: true
date: 2020-07-20 15:03:43
tags:
- windows
- wsl
categories: technology
---

WSL 是 Windows10 Linux 子系统，它可以让Windows10用户无需安装虚拟机就可以使用Linux系统，非常的方便。但是，默认WSL不开启sshd服务，因此，会降低使用的便捷性，这里给出如何开启该服务，以及如何设置开机自启动其他服务，如 frp 等

<!--more-->

1. 在 Windows10 上，使用 **windows + r** 键，调出运行，输入 **shell:startup** 进入开机启动项文件夹

2. 新建文件：**wsl.vbs**，名字自定义，但必须使用 vbs 作为扩展名

3. 添加如下内容：（可以使用 notepad++ 打开）

   ```vbscript
   Set ws = CreateObject("Wscript.Shell")
   ws.run "wsl -d Ubuntu-18.04 -u root /etc/init.d/ssh start", vbhide
   ws.run "wsl -d Ubuntu-18.04 -u root ~jinzhongxu/.frp.local/frpc -c ~jinzhongxu/.frp.local/frpc.ini", vbhide
   ws.run "wsl -d Ubuntu-18.04 -u root /etc/init.d/zabbix-agent start", vbhide
   ws.run "wsl -d Ubuntu-18.04 -u root cd ~jinzhongxu/.jan && ./jan", vbhide
   ```

4. 其中的 "Ubuntu-18.04" 为默认的 WSL 名称，可以使用 cmd 命令: 

   ```powershell
   wsl -l
   // 结果如下：
   适用于 Linux 的 Windows 子系统:
   Ubuntu-18.04 (默认)
   ```

扩展内容：可以在cmd中直接输入 **wsl** 进入Linux子系统shell