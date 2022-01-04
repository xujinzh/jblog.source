---
title: 利用 GitHub 和 HEXO 随时随地书写博客
author: Jinzhong Xu
date: 2020-03-03 21:42:36
tags:
- hexo
- github
- mac
categories: technology
---

当使用 GitHub + HEXO + Markdown 部署个人博客时，总是希望能够在家和在单位都能够随时随地写博客，记录当时的工作或灵感。本篇文章介绍如何完成家庭和单位无缝衔接的去自由书写博客。如何搭建博客请参考我的另一篇文章： [利用 Hexo and Github 搭建个人博客](https://xujinzh.github.io/2019/12/20/%E4%BD%BF%E7%94%A8GitHub%20%E4%B8%8E%20Hexo%20%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2/) .

<!--more-->

# 在 GitHub 上新建一个仓库

在 GitHub 上创建一个仓库，用以保持和同步博客源文件（包括博客文章、主题等内容）

比如：

```shell
git@github.com:xujinzh/JBlog.git
```



# 将本地博客同步到 GitHub

如果你当前的博客源文件是在单位，那么，我们将该文件夹同步上传到 GitHub 上。步骤如下：

```bash
cd JBlog
git init
git remote add origin git@github.com:xujinzh/JBlog.git
git add .
git commit -m "release 1.0.0"
git push -u origin master
```



# 将 GitHub 仓库克隆到本地

回到家，将 GitHub 上的仓库克隆到家里 Mac 上。注意，该Mac上首先需要安装 hexo，步骤如下：

```bash
brew update
brew install node
sudo npm install hexo-cli -g
```

克隆仓库到本地Mac上：

```bash
git clone git@github.com:xujinzh/JBlog.git /Users/jinzhongxu/github/JBlog
```

此时，运行 hexo -v 会发生如下错误：

```bash
ERROR Local hexo not found in ....
ERROR Try running: 'npm install hexo --save'
```

这是由于.gitignore 中缺少 node_modules 文件夹，没有更新上去。解决方法如下：

```bash
cd JBlog
npm cache clean --force
npm install -g npm
npm install

# 查看博客显示，本地模式 http://localhost:4000
hexo server
```

或者

```bash
npm install --force
```

到此，就可以正常使用了。

# 写文章

在家写文章

```bash
hexo new mac-test
```

然后打开 mac-test.md 写文章，然后，部署文章到网址

```bash
hexo clean && hexo generate && hexo deploy
```

之后，将更新推送到 GitHub 仓库

```bash
git status
git add .
git commit -m "new article"
git push
```

如果是第一次 push，请使用

```bash
git push -u origin master
```

它会记住你的提交分支情况，这样以后就可以直接 git push 了。

回到办公室，首先从 GitHub 仓库拉去最新

```bash
git pull
```

然后，开始写文章，记住发布完文章后，记得 push 到 GitHub 仓库。



# 参考链接

1. [管理远程仓库](https://docs.github.com/cn/get-started/getting-started-with-git/managing-remote-repositories)

