---
title: git 基础知识
date: 2020-01-13 09:07:22
tags:
- git
- github
categories: technology
---

git 是一种分布式版本控制软件，最初由林纳斯·托瓦兹于2005年为了更好的管理Linux内核开发而设计。可以保存文件的更新记录，可以恢复以前的状态，显示编辑前后的差异。当同时上传新文件覆盖旧文件时，如果发生冲突会发出警告。

<!--more-->

# 数据库

git数据库分为远程数据库和本地数据库，如果想要公开本地数据库中的内容，可以上传到远程数据库。GitHub就是远程数据库的代码托管软件，可以免费公开也可以付费私享，团队协作开发。

本地数据库可以通过在本地创建全新的数据库，也可以从远程数据库克隆拉取。

当提交本地代码或文件修改时，应认真填写修改内容的提交信息，一般可以用”提交修改内容的摘要+修改理由“问模板提交信息。

git版本控制软件下，实际的工作目录称为工作树，数据库和工作树之间有索引，索引是为了向数据库提交作准备的区域。Git在执行提交的时候，不是直接将工作树的状态保存到数据库，而是将设置在中间索引区域的状态保存到数据库。因此，要提交文件，首先需要把文件加入到索引区域中。

# git 安装

从官网下载相应版本进行安装。官网地址：[git-scm.com](https://git-scm.com/downloads)，安装完成后，使用如下命令测试git是否安装成功

```bash
git --version
```

# git 配置

在安装完git后，一般需要先对git进行配置，才能后续正常使用，配置文件在家目录下面的 **.gitconfig** 隐藏文件里。在命令行中配置git，会自动将git配置信息写入该文件中。

- 配置用户名和邮箱

  ```bash
  git config --global user.name "jinzhongxu"
  
  git config --global user.email "xujinzhong027@outlook.com"
  ```

  也可以将 **--global** 去掉，这样就只是针对当前数据库配置，而不是本机配置。

- git命令彩色显示

  ```bash
  git config --global color.ui auto
  ```

- git命令起别名，如给命令**checkout**起别名**co**

  ```bash
  git config --global alias.co checkout
  ```

# git 创建本地数据库

## 创建本地数据库

使用如下命令创建名为bottle的数据库

```bash
mkdir bottle
cd bottle/
git init
```

## 提交文件到仓库

在本地bottle数据库目录下编辑文件hello.java，然后提交到本地数据库

```bash
git status # 查看状态
git add hello.java # 添加文件到索引
git commit -m "add hello.java for test" # 提交文件
git log # 查看
```

如果有多个文件，可以利用如下命令把所有文件添加到索引

```bash
git add .
```

或者

```bash
git add hello.java ck.java
```

## 查看提交的文件

使用如下命令查看提交的文件信息详情

```bash
git log
```

或者

```bash
git log --decorate
```

# git 连接本地数据库与远程数据库

在GitHub上注册个人账户，创建新的数据库，名字可以自定义。

在GitHub官网上创建的仓库一般都是裸仓库，即使用如下命令创建的仓库，一般都会以.git结尾，这也是为什么GitHub官网上创建的仓库是.git结尾。

```bash
git init --bare monkey.git
```

这种方式创建的仓库与缺少--bare方式创建的仓库区别有

1. 没有.git目录，但是正常的所有.git目录下面的文件都在monkey目录下；
2. 不能进行git命令操作；
3. 一般作为远端服务器代码仓库；
4. 可以克隆下来后使用push提交代码，而不使用--bare创建的仓库不能接收push来的代码；

更多相关内容可参考：[Git 本地仓库和裸仓库]([https://moelove.info/2016/12/04/Git-%E6%9C%AC%E5%9C%B0%E4%BB%93%E5%BA%93%E5%92%8C%E8%A3%B8%E4%BB%93%E5%BA%93/](https://moelove.info/2016/12/04/Git-本地仓库和裸仓库/))

这里，我们首先创建本地仓库，然后添加GitHub远程仓库到本地仓库，其次pull远程仓库到本地仓库，最后将本地仓库的增加的代码push到远程仓库

## 实例化本地仓库

```bash
git init bottle
```

查看配置信息

```bash
git config --list
```

配置git用户信息

```bash
git config user.name "jinzhongxu"
git config user.email "xujinzhong027@outlook.com"
```

## 添加远程仓库

```bash
git remote add origin https://github.com/xujinzh/monkey.git
```

## 拉取远程仓库或者更新本地库

```bash
git pull origin master:master
```

上面代码中，origin表示远程仓库，第一个master表示远程仓库的master分支，第二个master表示本地分支，可以写成其他分支，如果分支不存在会自动创建。**另外，该命令也是从远程数据库把最新变更内容拉取到本地的方法，以更新别的开发者提交的最新代码。** 省略数据库名称的话，会在名为origin的数据库进行pull。使用log指令来确认历史记录是否已更新。

该步也可以使用fetch命令代替

```bash
git fetch origin master
git merge FETCH_HEAD
```

## 提交本地仓库到远程仓库

```bash
vim test.txt
echo "write in local" | cat >> test.txt
git status
git add test.txt
git commit -m "add test.txt for test"
git push -u origin master
```

**注意**：也可以直接使用clone来完成上面的步骤，代码如下

```bash
git clone https://github.com/xujinzh/monkey.git neck
```

然后再进入 neck 目录配置 git 用户信息，之后就可以添加代码，push 代码了。该方法是最简单的创建数据库的方法。并且，在克隆的数据库推送时，可以省略远程数据库和分支的名称，如下简单命令

```bash
git push
```

在执行pull之后，进行下一次push之前，如果其他人进行了推送内容到远程数据库的话，那么你的push将被拒绝。这是因为，如果远程数据库和本地数据库的同一个地方都发生了修改的情况下，因为无法自动判断要选用哪一个修改，所以就会发生冲突。Git会在发生冲突的地方修改文件的内容，需要我们手动修正冲突。如下命令以文本形式显示更新记录的流程图。

```bash
git log --graph --oneline
```

## 修改提交时密码连接为SSH直接提交

使用如下命令查看远程克隆方式

```bash
git remote -v
```

```shell
origin  https://github.com/xujinzh/ph-novelty-detection.git (fetch)
origin  https://github.com/xujinzh/ph-novelty-detection.git (push)
```

如果已经添加本地机器的SSH公钥到GitHub了，那么直接，修改为SSH远程连接方式、

```bash
git remote set-url origin git@github.com:xujinzh/ph-novelty-detection.git
```

再次使用

```bash
git remove -v
```

 查看

```shell
origin  git@github.com:xujinzh/ph-novelty-detection.git (fetch)
origin  git@github.com:xujinzh/ph-novelty-detection.git (push)
```

这样，再次push代码时，就不需要输入用户名和密码了。

更多git相关知识请参考：

- [git - 简易指南](https://www.bootcss.com/p/git-guide/)

- [猴子都能懂的GIT入门](https://backlog.com/git-tutorial/cn/)

- [GIT CHEAT SHEET](https://www.runoob.com/manual/github-git-cheat-sheet.pdf)