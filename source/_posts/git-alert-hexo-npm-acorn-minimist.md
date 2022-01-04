---
title: 修复GitHub提示acorn和minimist因版本低而易受攻击警告
author: Jinzhong Xu
mathjax: true
date: 2020-03-18 22:18:03
tags:
- acorn
- minimist
- hexo
- npm
- git
categories: technology
---

Hexo + Github 搭建博客，在需要两地（单位与家）同时写博文时，选择将blog源码上传到GitHub，但是，某时当使用 **git push** 上传源码到GitHub，出现 *acorn & minimist 容易受攻击*。

下面是自己摸索的解决办法：

<!--more-->

# Mac端

在家里Mac终端执行如下命令

```bash
cd ~/github/JBlog
npm install acorn@latest
npm install minimist@latest
```

如果版本较低，也可以使用如下命令更新npm

```bash
npm install -g npm
```

如下命令可以修复问题，但是我这里好像没什么用处，不过作为笔记还是记录下来

```bash
npm audit fix
```

# Windows sublinux - Ubuntu 端

在Ubuntu终端执行如下命令

```bash
cd github/JBlog
npm install acorn@latest
npm install minimist@latest
```

# 提交到GitHub

在Mac端更新低版本的插件后，就可以使用如下命令上传到GitHub了

首先，先生成和部署博文

```bash
hexo clean && hexo g && hexo d
```

其次，上传GitHub上

```bash
git status
git add .
git commit -m "correct bugs for alerts"
git push
```

发现已经没有了警告信息

最后，在Ubuntu端

```bash
git pull
```

