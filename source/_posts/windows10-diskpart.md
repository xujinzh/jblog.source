---
title: 安装 windows10 时出现无法创建分区
date: 2019-12-20 20:26:18
tags:
- windows
- disk
categories: technology
---

在安装 windows10 时出现无法创建分区，一般是因为两个问题，一个是启动引导项未设置成 UEFI ；另一个原因是磁盘未格式化 GPT 格式。下面分别给出解决两者的方法。（UEFI 对应 GPT；Legacy 对应 MBR）

第一个问题解决方法，设置引导项为 UEFI

<!--more-->

重启系统，长按 F2 进入 bios，选择 UEFI 作为引导项

第二个问题解决方法，格式化磁盘为 GPT

在系统安装界面，使用快捷键 SHIFT + F10 进入CMD，输入

```bash
diskpart
list disk
select disk 0
clean
convert GPT
```

至此，完成了磁盘0格式化为 GPT 格式。

这时候，再重新安装系统就可以了。推广参考 <a rel="noreferrer noopener" aria-label="csdn（在新窗口打开）" href="https://blog.csdn.net/abcdad/article/details/80273646" target="_blank">csdn</a> 

经实际验证，安装Windows10系统时，使用UEFI+GPT；安装Ubuntu系统时，使用Legacy+MBR。