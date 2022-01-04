---
title: 使用SSH连接GitHub
date: 2020-01-08 09:46:12
tags:
- github
- ssh
categories: technology
---

在本地开发代码时，需要push本地代码到GitHub仓库时，总是需要输入用户名和秘密，这导致了开发的效率降低，也不安全。那有一种方式可以快速高效且安全的连接到GitHub仓库，并提交代码，那就是使用SSH连接GitHub仓库，下面给出如何从手动输入秘密切换到使用SSH连接GitHub仓库。

<!--more-->

# 添加SSH公钥到GitHub账号下

假设使用的是Ubuntu系统，将用户下的公钥拷贝粘贴到GitHub账号下

```bash
cat ~/.ssh/id_rsa.pub
```

拷贝所有内容到GitHub主页---右上角Settings---左侧SSH and GPG keys---SSH keys---New SSH key---随便写个名字在title栏，并粘贴刚刚拷贝的内容到key栏中---Add SSH key

# 测试能否正确通过SSH连接GitHub

在终端中输入一下命令

```bash
ssh -T git@github.com
```

如果出现 Hi your-user-name! You've successfully authenticated, but GitHub does not provide shell access. 证明正确通过SSH连接GitHub

# 添加本地仓库

```bash
git remote rm origin

git remote add origin git@github.com:user-name/your-repo-name.git
```

# push 本地代码到仓库

正确完成以上步骤后，第一次push本地代码，需要使用如下命令

```bash
git push --set-upstream origin master
```

以后，就可以直接使用如下命令进行提交代码到远程仓库

```bash
git push
```

