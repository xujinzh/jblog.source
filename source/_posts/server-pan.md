---
title: 在服务器上搭建个人网盘
date: 2019.12.21
tags:
- linux
- pan
categories: technology
---

本篇通过 <a href="https://filebrowser.xyz/installation">filebrowser</a> 在服务器上搭建个人网盘.

首先，在终端运行一下代码：

<!--more-->

```bash
curl -fsSL https://filebrowser.xyz/get.sh | bash
```

进行网盘系统安装。

其次，进入你需要建立网盘的目录，运行 filebrowser 命令

或者直接运行 

```bash
filebrowser -`r` /path/to/your/files
```



这时，网盘运行中服务器127.0.0.1:8080端口。

最后，本地远程连接服务器，并映射服务器端口到本地端口8008，命令如下:

```bash
ssh -L8008:localhost:8080 root@xxx.xxx.xxx.xxx
```

这样就可以在本地浏览器输入localhost:8008打开个人网盘，记得修改密码。

注意点：平常想要本地打开网盘，可以直接在本地终端一行命令完成，如下:

```bash
ssh -L8008:localhost:8080 root@xxx.xxx.xxx.xxx 'filebrowser'
```