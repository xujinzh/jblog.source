---
title: 开启bbr
date: 2019-12-20 19:23:30
tags:
- ubuntu
- bbr
categories: technology
---

有时候网速太慢，可以使用bbr加速，使用如下命令

```bash
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
sysctl -p
sysctl net.ipv4.tcp_available_congestion_control
lsmod | grep bbr
```

