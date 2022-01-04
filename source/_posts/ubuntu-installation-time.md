---
title: 如何查询 Ubuntu 系统安装时间
date: 2019-12-20 20:34:25
tags:
- debian
- ubuntu
categories: technology
---

<p>为了某些目的，有时候我们需要知道 Ubuntu 系统的安装时间，那么如何通过terminal 查询系统安装时间呢，这里给出两种查询方法：</p>
<p>第一种方法：运行如下命令直接查询日志文件获取系统安装时间：</p>
<ul><li>ls -ld /var/log/installer</li></ul>
<p>第二种方法：</p>
<p>1、首先，运行如下命令查询系统有哪些分区</p>
<ul><li>ls /dev | grep sd</li></ul>
<p>2、然后，运行如下命令查询系统盘格式化时间，就是系统安装时间</p>
<ul><li>sudo dumpe2fs /dev/sda | grep 'Filesystem created'</li></ul>
<p>在第一步种，可能有些是 /dev/sdb，但大多数是 /dev/sda</p>
