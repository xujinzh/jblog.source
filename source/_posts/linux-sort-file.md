---
title: Linux 文件按大小或时间排序
date: 2019-12-20 20:06:51
tags:
- linux
- ubuntu
- ls
categories: 
- technology
- linux
---


## 1, 查看 Linux 某一个文件夹下所有文件，并按从新到旧的时间排序：

<!--more-->

```bash
ls -tl
```


## 2, 从旧到新的时间排序：


```bash
ls -tlr
```


## 3, 查看 Linux 某一个文件夹下所有文件，并按文件大小从大到小排序:


```bash
# 对于子文件夹，则只有文件夹的大小，不会计算整个子文件夹下所有文件的大小
ls -Sl
ls -Slh
# 同时会计算文件夹下子文件夹所有文件的大小，参与排序
du -s * | sort -nr
```


## 4, 从小到大排序：


```bash
ls -Slr
ls -Slhr
du -s * | sort -n
```


## 5, 查看最大的前几个文件


```bash
du -s * | sort -nr | head # 默认前10个
ls -Slh | head -n 3  # 最大的3个
du -s * | sort -n | tail
```


## 6, 查看最小的几个个文件：


```bash
du -s * | sort -nr | tail # 默认前10个
ls -Slrh | head -n 3 # 最小的3个
du -s * | sort -n | head
```

## 7, 查看根目录下所有文件的大小

```bash
# 发现哪些文件夹占用磁盘比较多
sudo du -h --max-depth=1 /
df -h
# 不进入文件夹查看文件夹下文件的大小
du -h --max-depth=1 archive

# 递归查看文件夹下子文件夹的大小
du -h archive
# 查看某一个文件夹下所有文件大小和
du -h --max-depth=0 archive
du -sh archive
```

