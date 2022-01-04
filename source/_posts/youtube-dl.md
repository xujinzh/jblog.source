---
title: youtube-dl 下载视频方法
author: Jinzhong Xu
mathjax: true
date: 2020-07-11 10:54:31
tags:
- youtube-dl
- video
categories: technology
---

youtube-dl 能够方便的下载网页视频，虽然不是所有网站视频都可以下载，但已经能够覆盖很多网站。这里介绍一下如何使用youtube-dl有效下载视频。

<!--more-->

## 安装youtube-dl

```shell
pip install youtube-dl --upgrade
```

## 配置代理

如果使用了代理，可以使用如下配置

```shell
# 打开本地配置文件
vim .bashrc
# 添加如下内容
alias youtube = "youtube-dl --no-check-certificate --proxy socks5://127.0.0.1:1080"
```

## 列出有哪些格式视频

```shell
youtube-dl --list-formats https://www.yyy.com/video/BV1TW411g7Tf/
# 或者
youtube-dl -F https://www.yyy.com/video/BV1TW411g7Tf/
```

## 下载指定格式视频和音频

```shell
youtube-dl -f22+140 https://www.yyy.com/video/BV1TW411g7Tf/
```

## 下载所有格式视频

```shell
youtube-dl --all-formats https://www.yyy.com/video/BV1TW411g7Tf/
```

## 下载视频和字幕

```shell
youtube-dl --write-sub --all-subs https://www.yyy.com/video/BV1TW411g7Tf/
```

## 下载最高清的视频

```shell
youtube-dl -f bestvideo+bestaudio https://www.yyy.com/video/BV1TW411g7Tf/
```

## 下载多个视频

```shell
# 首先将视频地址写入文件中，注意，如果是一个视频集合，这里只需要输入第一个视频的链接，就可以下载整部视频
cat https://www.yyy.com/video/BV1TW411g7Tf/ >> video.txt

# 然后，从文件中获取链接下载视频
youtube-dl -a video.txt
```

## 下载多个最高清带有字幕的视频

```shell
youtube-dl -f bestvideo+bestaudio --write-sub --all-subs -a video.txt
```

