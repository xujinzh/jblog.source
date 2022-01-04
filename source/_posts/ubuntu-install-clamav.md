---
title: Ubuntu 安装杀毒软件 clamav
date: 2019-12-20 20:39:15
tags:
- ubuntu
- clamav
categories: technology
---

<p>有些时候，基于某些特殊要求需要给 Ubuntu 安装杀毒软件，这里介绍一个非常有效的杀毒软件：clamav，它还有 GUI 界面 clamtk.</p>
<p>命令行安装杀毒软件：</p>
<!--more-->

```bash
sudo apt update
sudo apt upgrade
sudo install clamav clamtk
```

<p>使用 clamav 扫描某个文件夹下的所有文件，并把扫描结果写到 antivirus.history文件中：</p>
```bash
sudo clamscan -r /home/jayzonxu/Downloads | tee antivirus.history
```

<p>如果不喜欢使用命令行，也可以使用 GUI 界面的 clamtk;</p>
<p>更多有关 clamav 请参看其中文<a rel="noreferrer noopener" aria-label="wiki（在新窗口打开）" href="https://wiki.ubuntu.org.cn/ClamAV" target="_blank">wiki</a>.</p>
