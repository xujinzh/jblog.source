---
title: 利用 git 把本地代码保存到远程仓库
mathjax: true
author: Jinzhong Xu
date: 2021-10-21 13:52:08
tags:
- git
- github
- gitee
categories:
- technology
---

现如今大多数开发者都会将本地开发的代码托管到远程仓库，那如何将本地新创建的工程代码保存到远程服务器的新仓库呢？下面给出实现过程。

<!--more-->

# 创建远程仓库

以 github 为例。

登录账户，在界面右上交点击加号，选择 New repository，输入仓库名称，如 test，其他都不选。点击 Create repository

# 本地代码托管

在本地，将代码首先初始化 git，然后添加远程仓库地址，上传即可。

```bash
cd hello-test
git init
git remote add origin git@github.com:xujinzh/hello-test.git
git add .
git commit -m "release 1.0.0"
git branch -M main
git push -u origin main
```



