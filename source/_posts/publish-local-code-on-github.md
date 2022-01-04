---
title: 把本地代码发布到 GitHub 上
mathjax: true
author: Jinzhong Xu
date: 2020-10-13 12:54:32
tags:
- github
- python
categories:
- technology
- python
---

本地写好的代码如何发布到 GitHub 上呢？这里给出一个方法。

<!--more-->

# 在 GitHub 上创建一个新的仓库

1. 填写自己喜欢的仓库名
2. 填写自己需要的仓库介绍，简短介绍仓库
3. 选择初始化仓库
4. 最好初始化 README file
5. 点选 .gitignore
6. 选择自己需要的代码版权协议

# 克隆仓库到本地

最好为 GitHub 添加 SSH 认证，这样在提交修改的代码时不需要重复输入用户名和密码，方法参考我的另一篇博文 [使用SSH连接GitHub](https://xu-jinzhong.gitee.io/2020/01/08/ssh-git/)

克隆代码方法如下（这里以 SSH 为例）

```bash
git clone git@github.com:xujinzh/CSK.git
```

# 把本地代码放到克隆的文件夹下

如我这里本地开发的代码是 

```bash
cf-csk
```

经过上面克隆后，代码放在

```bash
CSK
```

拷贝 cf-csk 文件到 CSK 里

```bash
cp -r cf-csk/* CSK/
```

# 提交代码到 GitHub

一般需要完善 README.md 文件以便更好的介绍该仓库，修改后，进行提交

```bash
git status
git add .
git commit -m "add csk tackers"
git push -u origin main:main
```

