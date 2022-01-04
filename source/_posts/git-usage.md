---
title: git 简单使用命令
date: 2020-01-08 10:17:47
tags:
- github
- git
categories: technology
---

使用GitHub来保存本地代码，既能够防止代码丢失，又能够与Coders交流互动，那么如何使用git命令提交本地代码呢，下面介绍一下流程.

<!--more-->

# 远程GitHub已经建有一个仓库

如果远程GitHub上已经建有一个仓库，或者通过网页建成一个仓库，使用如下步骤进行

## 克隆远程仓库

网页上打开仓库首页，找到右上角的Clone or download

### 使用SSH克隆仓库

```bash
git clone git@github.com:user-name/your-repo-name.git
```

该方法需要事先将本地用户目录下的公钥拷贝到本人GitHub网站的设置中的SSH keys中，具体方法如下：

拷贝所有内容到GitHub主页---右上角Settings---左侧SSH and GPG keys---SSH keys---New SSH key---随便写个名字在title栏，并粘贴刚刚拷贝的内容到key栏中---Add SSH key

测试SSH连接情况：

在终端中输入一下命令

```bash
ssh -T git@github.com
```

如果出现 Hi your-user-name! You've successfully authenticated, but GitHub does not provide shell access. 证明正确通过SSH连接GitHub

### 使用https克隆仓库

```bash
 git clone https://github.com/user-name/your-repo-name.git
```

## 设置本地代码仓库提交者的用户名和用户邮箱

该节参考GitHub官网：[在 Git 中设置用户名](https://help.github.com/cn/github/using-git/setting-your-username-in-git)

### [为计算机上的*每个*仓库设置 Git 用户名](https://help.github.com/cn/github/using-git/setting-your-username-in-git#)

1. 打开 Git Bash。

2. 设置 Git 用户名：

   ```shell
   $ git config --global user.name "Mona Lisa"
   ```

3. 确认您正确设置了 Git 用户名：

   ```shell
   $ git config --global user.name
   > Mona Lisa
   ```

### [为一个仓库设置 Git 用户名](https://help.github.com/cn/github/using-git/setting-your-username-in-git#setting-your-git-username-for-a-single-repository)

1. 打开 Git Bash。

2. 将当前工作目录更改为您想要在其中配置与 Git 提交关联的名称的本地仓库。

3. 设置 Git 用户名：

   ```shell
   $ git config user.name "Mona Lisa"
   ```

4. 确认您正确设置了 Git 用户名：

   ```shell
   $ git config user.name
   > Mona Lisa
   ```

## push本地代码到远程GitHub仓库

 进入本地代码库文件夹目录

```shell
git status
git add .
git commit -m "test push"
git push
```

git push[命令](https://www.yiibai.com/git/git_push.html) 

```shell
# git push 使用方法
git push <远程主机名> <本地分支名>:<远程分支名>
git push [--all | --mirror | --tags] [--follow-tags] [--atomic] [-n | --dry-run] [--receive-pack=<git-receive-pack>]
       [--repo=<repository>] [-f | --force] [-d | --delete] [--prune] [-v | --verbose]
       [-u | --set-upstream] [--push-option=<string>]
       [--[no-]signed|--sign=(true|false|if-asked)]
       [--force-with-lease[=<refname>[:<expect>]]]
       [--no-verify] [<repository> [<refspec>…]]

# 将本地master主分支推送到远程仓库origin的master主分支。当master不存在时，新建远程master主分支
git push origin master

# 删除远程仓库origin的master主分支
git push origin :master
git push origin --delete master

# 本地分支和远程分支存在链接/追踪关系，可省略本地分支和远程分支名
git push origin

# 本地分支只有一个链接/追踪分支，可省略远程仓库名
git push

# 本地仓库与多个远程仓库存在链接/追踪关系，可使用-u（--set-upstream）指定默认远程仓库名，这样后面可以不加参数使用git push
git push -u origin master

# 不带任何参数的git push，默认只推送当前分支，这叫做simple方式。此外，还有一种matching方式，会推送所有有对应的远程分支的本地分支。Git 2.0版本之前，默认采用matching方法，现在改为默认采用simple方式。如果要修改这个设置，可以采用git config命令

git config --global push.default matching
git config --global push.default simple

# 不管是否存在对应的远程分支，将本地的所有分支都推送到远程仓库，这时需要使用–all选项
git push --all origin 
```



## pull远程仓库

当远程仓库更新比本地仓库快时，需要从远程仓库更新本地仓库到最新，命令如下

```bash
git pull
```

# 远程GitHub没有仓库

当远程GitHub个人网站下没有需要的仓库时，需要首先在网页下创建远程仓库，如仓库名为cvRepo

在本地创建同名仓库

```bash
mkdir cvRepo
```

进入仓库目录，并初始化本地仓库

```bash
cd cvRepo
git init
```

git 本地代码到远程仓库

```bash
git add .
git commit -m "cv codes"
git remote add origin git@github.com:user-name/your-repo-name.git
git push -u origin master
```

# 修改git默认编辑器为vim

```bash
git config --global core.edit vim
```

查看是否成功

```bash
git config --list | grep vim
```

更多git使用技巧请参考GitHub官网的[使用 Git](https://help.github.com/cn/github/using-git)

