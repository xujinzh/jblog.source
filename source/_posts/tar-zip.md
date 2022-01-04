---
title: Linux 压缩与解压缩命令 tar and zip
date: 2019-12-20 19:56:37
tags:
- ubuntu
- tar
- zip
categories: technology
---

# tar.gz
## text文件/目录

```bash
tar -czvf text.tar.gz text   %压缩text成text.tar.gz
tar -xzvf text.tar.gz           %解压缩text.tar.gz
```
note: c为压缩, x为解压缩, z为gz格式, v为显示解、压缩过程,  f: 使用档案名字，切记，这个参数是最后一个参数，后面只能接档案名。

<!--more-->

# tar.bz2
## text文件/目录

```bash
tar -cjvf text.tar.bz2 text       %压缩text成text.tar.bz2
tar -xjvf text.tar.bz2           %解压缩text.tar.bz2
```
note: c为压缩, x为解压缩, j为bz2格式, v为显示解、压缩过程,  f: 使用档案名字，切记，这个参数是最后一个参数，后面只能接档案名。

# zip
## text文件/目录

```bash
zip text.zip text     %压缩text为zip格式
unzip text.zip        %解压缩text.zip为text
```

# tgz
`tar zxvf backups.tgz`
or
`gunzip -c backups.tgz | tar xvf -`