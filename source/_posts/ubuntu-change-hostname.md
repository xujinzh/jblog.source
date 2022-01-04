---
title: Debian or Ubuntu 修改主机名
date: 2019-12-20 20:46:07
tags:
- ubuntu
- debian
- hostname
categories: technology
---

<p>首先，运行命令查看主机名</p>
<!--more-->

```bash
hostnamectl
```

<blockquote class="wp-block-quote"><p>Static hostname: debian-virtualbox<br>          Icon name: computer-vm<br>            Chassis: vm<br>         Machine ID: aaaaaaaaaaaaaaaaaaaaaaaa<br>            Boot ID: bbbbbbbbbbbbbbbbbbbb<br>     Virtualization: oracle<br>   Operating System: Debian GNU/Linux 10 (buster)<br>             Kernel: Linux 4.19.0-6-amd64<br>       Architecture: x86-64</p></blockquote>
<p>其次，运行如下命令修改主机名</p>
```bash 
hostnamectl set-hostname debian
```

```bash
vim /etc/hosts
```

<p>修改 /etc/hosts 中的 debian-virtualbox 主机名为 debian，重启一个 terminal，发现已经修改成功。</p>

