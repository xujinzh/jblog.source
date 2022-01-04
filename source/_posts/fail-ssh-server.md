---
title: Failed to connect to changelogs.ubuntu.com.meta.release.lts
date: 2019-12-20 20:03:29
tags:
- ssh
- debian
categories: technology
---

当出现 Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. 可以在终端中运行如下两行代码来解决。
```bash
sudo rm /var/lib/ubuntu-release-upgrader/release-upgrade-available
sudo /etc/update-motd.d/91-release-upgrade
```

