---
title: Ubuntu 包管理工具 snap
date: 2019-12-20 20:51:41
tags:
- ubuntu
- snap
categories: technology
---

<p>在 Ubuntu 系统上，命令行利用 apt 作为包管理工具已经是非常不错的，但是，有些包，比如 youtube-dl 却不能很好的安装和更新，但是，通过 snap 我们可以非常完美的解决这类包的安装与管理。</p>
<p>利用 apt 安装 snap，命令如下</p>
<!--more-->

```bash
sudo apt install snapd
```

<p>查找需要安装的包</p>
```bash
snap find search_text
```

<p>安装包</p>
```bash
sudo snap install package
```

<p>查看安装了哪些包</p>
```bash
snap list
```

<p>查看安装的包的历史</p>
```bash
snap changes
```

<p>更新安装的包</p>
```bash
sudo snap refresh package
```

<p>也可以查看更新列表</p>
```bash
sudo snap refresh --list
```

<p>返回包的更新到上一个版本</p>
```bash
sudo snap revert package
```

<p>卸载包</p>
```bash
sudo snap remove package
```

<p>通过 snap下载离线 app</p>
```bash
snap download package_name
```

<p>安装app</p>
```bash
snap ack package_name
snap install package_name
```

<p>如果忘记 sudo，导致命令不能执行，可以运行如下命令</p>
```bash
sudo !!
```

<p>或者，添加 sudo，重新运行上一个命令</p>
