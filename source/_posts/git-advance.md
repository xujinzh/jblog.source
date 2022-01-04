---
title: git 知识汇总补充
date: 2020-01-13 19:39:56
tags:
- git
- github
categories: technology
---

Git的分支功能可以支持同时进行多个功能的开发和版本管理。

<!--more-->

## 查看分支情况

- 查看当前分支和本地分支

```bash
git branch
```

- 查看远程分支

```bash
git branch -r
```

- 查看所有分支

```bash
git branch -a 
```

## 创建本地分支

- 直接创建本地分支

```bash
git branch issue3
```

- 切换分支

```bash
git checkout issue3
```

- 创建并切换本地分支

```bash
git checkout -b issue3
```

## 合并分支

使用如下命令合并分支，如果目前是在issue3分支，首先需要切换到master分支，然后再合并issue3分支到master分支

```bash
git checkout master
git merge issue3
```

除了merge命令外，还有一种命令可以合并分支，那就是rebase，更多内容可以查看：[分支的合并](https://backlog.com/git-tutorial/cn/stepup/stepup1_4.html)

## 删除分支

```bash
git branch -d issue3
```

## 删除远程分支

```shell
git push origin --delete add-license-1
```

<font color='dd0000'>注意：删除远程分支不使用git branch -d，而是使用git push origin --delete命令</font>

但是，当远程仓库没有该分支时，或者在本地使用git branch -r能够看到远程仓库名，但远程仓库却已经不存在时，使用上面删除远程分支，会出现如下错误：

```shell
error: unable to delete 'xxx': remote ref does not exist 
```

这时候需要我们更新本地仓库的远程仓库列表信息，命令如下：

```shell
git fetch --prune

# 更新后，会列出远程仓库origin已经不存在的分支
// From github.com:xujinzh/JBlog
//  - [deleted]         (none)     -> origin/add-license-1
//  - [deleted]         (none)     -> origin/dependabot/npm_and_yarn/acorn-7.1.1
```

之后，再使用命令 git branch -r 查看，就会发现最新的远程仓库列表信息

## 添加标签

首先运行如下命令，查看commit id，

```bash
git log 
```

运行如下命令添加标签

```bash
git tag 1.0.0 c584a15bd2
```

其中，1.0.0为标签，版本号，c584a15bd2为commit id，只要唯一即可。

## 查看标签

```bash
git tag -n
```

## 删除标签

```bash
git tag -d <tagname>
```

## 提交本地分支issue3到远程分支dis

```bash
git push -u origin issue3:dis
```

如果远程分支不存在，则自动创建一个分支

## 删除远程分支dis

```bash
git push -u origin :dis
```

## 自建git托管服务器

这里以Ubuntu为例

首先，需要有一个vps服务器，假设IP地址是1.14.1.14

其次，在服务器上增加用户和组，比如添加 jinzhongxu 用户

```bash
adduser jinzhongxu
```

然后，安装git命令

```bash
sudo apt update && sudo apt install git
```

最后，建立远程仓库

```bash
git init --bare monkey.git
```

把远程仓库克隆到本地

```bash
git clone jinzhongxu@1.14.1.14:/home/jinzhongxu/monkey.git
```

## 远程仓库

当从远程仓库克隆后，git默认远程仓库名为origin，并将远程的master分支与本地的master分支对应。查看远程仓库名称使用如下命令

```shell
git remote
```

or

```shell
git remote -v
```

将显示更多信息，不仅有远程仓库名称，还有fetch和push的远程仓库地址。当没有push权限时，将不显示push地址。

## 多人协作开发提交

当克隆远程仓库到本地，一般是master分支，产品版本分支，该分支是主分支，时刻与远程仓库同步；除此之外，开发人员在本地还会创建dev分支、bug分支和feature分支。其中dev分支是开发分支，团队开发人员开发分支，重大版本迭代时merge到主分支，一般也需要时刻与远程dev分支同步；bug分支是本地修复bug的分支，一般不进行远程同步；feature分支同步取决于团队需要。该小节可参考：[多人协作](https://www.liaoxuefeng.com/wiki/896043488029600/900375748016320)

1. leader或首位开发者在本地master分支外使用如下命令创建了dev分支，并提交到远程origin仓库

   ```shell
   # 在本地创建dev分支，并切换到dev分支，进行开发
   git checkout -b dev
   
   # 推送本地dev分支到远程仓库origin，并同时在远程仓库origin创建dev分支
   git push origin dev
   ```

2. 第二位开发人员首先克隆远程开发仓库到本地，然后在本地增加远程origin/dev分支到本地dev分支

   ```shell
   # 克隆远程origin仓库到本地机器
   git clone jinzhongxu@1.14.1.14:/home/jinzhongxu/monkey.git
   
   # 查看分支情况，特别是远程，发现远程仓库origin除了有master分支，还有分支origin/dev
   git branch -a
   //* master
   //  remotes/origin/HEAD -> origin/master
   //  remotes/origin/dev
   //  remotes/origin/master
   
   # 将远程仓库origin的dev分支增加到本地，并切换到本地dev分支
   git checkout -b dev origin/dev
   
   # 查看本地分支信息
   git branch
   //* dev
   //  master
   
   # 该开发人员进行了本地开发，并在dev分支增加部分代码，然后提交到远程仓库origin的dev分支
   git status
   git add .
   git commit -m "repair bug"
   git status
   git push origin dev:dev
   ```

3. 第三位开发人员也同样和第二位开发人员一样，下载了远程仓库origin的master分支和dev分支，并同时进行开发

   ```shell
   # 克隆远程origin仓库到本地机器
   git clone jinzhongxu@1.14.1.14:/home/jinzhongxu/monkey.git
   
   # 查看分支情况，特别是远程，发现远程仓库origin除了有master分支，还有分支origin/dev
   git branch -a
   //* master
   //  remotes/origin/HEAD -> origin/master
   //  remotes/origin/dev
   //  remotes/origin/master
   
   # 将远程仓库origin的dev分支增加到本地，并切换到本地dev分支
   git checkout -b dev origin/dev
   
   # 查看本地分支信息
   git branch
   //* dev
   //  master
   
   # 该开发人员进行了本地开发，并在dev分支增加部分代码，然后提交到远程仓库origin的dev分支
   git status
   git add .
   git commit -m "repair bug"
   git status
   
   # 但是，使用如下命令在提交时出现问题
   git push origin dev:dev
   // To 1.14.1.14:/home/jinzhongxu/monkey.git
   //  ! [rejected]        dev -> dev (fetch first)
   // error: failed to push some refs to 'jinzhongxu@1.14.1.14:/home/jinzhongxu/monkey.git'
   // hint: Updates were rejected because the remote contains work that you do
   // hint: not have locally. This is usually caused by another repository pushing
   // hint: to the same ref. You may want to first integrate the remote changes
   // hint: (e.g., 'git pull ...') before pushing again.
   // hint: See the 'Note about fast-forwards' in 'git push --help' for details.
   
   # 上面的错误提示很明白，那就是团队中已经有人提交的最新更新到远程dev分支，需要先git pull，然后再git push，使用如下命令解决
   git pull # 如果失败，可能的原因是没有指定本地dev分支与远程origin/dev分支的链接，可使用右边命令设置dev和origin/dev的链接：git branch --set-upstream-to=origin/dev dev
   # 成功后，出现如下提示，就是需要手动修改冲突，然后，再git push
   // remote: Counting objects: 5, done.
   // remote: Compressing objects: 100% (3/3), done.
   // remote: Total 3 (delta 1), reused 0 (delta 0)
   // Unpacking objects: 100% (3/3), done.
   // From 1.14.1.14:/home/jinzhongxu/monkey
   //    4461cc8..7056c16  dev        -> origin/dev
   // Auto-merging main.py
   // CONFLICT (content): Merge conflict in main.py
   // Automatic merge failed; fix conflicts and then commit the result.
   
   git status
   git add main.py
   git commit -m "repair bugs for xlabel and ylabel"
   git push origin dev
   ```

4. 前面三个过程，特别是2和3，是公司开发时常遇到，并且经常这样的迭代进行。特别是3，我们要经常使用，要熟练掌握。



## 更改远程仓库的 URL

当远程仓库 IP 地址更改、SSH 和 HTTPS 切换等其他导致远程仓库的 URL 地址发生改变时，需要切换地址，才能够正确提交本地修改的代码等内容上传到远程仓库。

更改远程仓库的 URL 需要使用命令：

```bash
git remote set-url
```

`git remote set-url` 命令使用两个参数：

- 现有远程仓库的名称。 例如，`源仓库`或`上游仓库`是两种常见选择。

- 远程仓库的新 URL。 例如：

  - 如果您要更新为使用 HTTPS，您的 URL 可能如下所示：

    ```shell
    https://github.com/USERNAME/REPOSITORY.git
    ```

  - 如果您要更新为使用 SSH，您的 URL 可能如下所示：

    ```shell
    git@github.com:USERNAME/REPOSITORY.git
    ```

### [将远程 URL 从 SSH 切换到 HTTPS](https://docs.github.com/cn/free-pro-team@latest/github/using-git/changing-a-remotes-url#将远程-url-从-ssh-切换到-https)

1. 打开 Terminal（终端）。

2. 将当前工作目录更改为您的本地仓库。

3. 列出现有远程仓库以获取要更改的远程仓库的名称。

   ```shell
   $ git remote -v
   > origin  git@github.com:USERNAME/REPOSITORY.git (fetch)
   > origin  git@github.com:USERNAME/REPOSITORY.git (push)
   ```

4. 使用

    

   ```
   git remote set-url
   ```

    

   命令将远程的 URL 从 SSH 更改为 HTTPS。

   ```shell
   $ git remote set-url origin https://github.com/USERNAME/REPOSITORY.git
   ```

5. 验证远程 URL 是否已更改。

   ```shell
   $ git remote -v
   # Verify new remote URL
   > origin  https://github.com/USERNAME/REPOSITORY.git (fetch)
   > origin  https://github.com/USERNAME/REPOSITORY.git (push)
   ```

下次对远程仓库执行 `git fetch`、`git pull` 或 `git push` 操作时，您需要提供 GitHub 用户名和密码。 When Git prompts you for your password, enter your personal access token (PAT) instead. Password-based authentication for Git is deprecated, and using a PAT is more secure. For more information, see "[Creating a personal access token](https://docs.github.com/cn/free-pro-team@latest/github/authenticating-to-github/creating-a-personal-access-token)."

You can [use a credential helper](https://docs.github.com/cn/free-pro-team@latest/github/using-git/caching-your-github-credentials-in-git) so Git will remember your GitHub username and personal access token every time it talks to GitHub.

### [将远程 URL 从 HTTPS 切换到 SSH](https://docs.github.com/cn/free-pro-team@latest/github/using-git/changing-a-remotes-url#将远程-url-从-https-切换到-ssh)

1. 打开 Terminal（终端）。

2. 将当前工作目录更改为您的本地仓库。

3. 列出现有远程仓库以获取要更改的远程仓库的名称。

   ```shell
   $ git remote -v
   > origin  https://github.com/USERNAME/REPOSITORY.git (fetch)
   > origin  https://github.com/USERNAME/REPOSITORY.git (push)
   ```

4. 使用

    

   ```
   git remote set-url
   ```

    

   命令将远程的 URL 从 HTTPS 更改为 SSH。

   ```shell
   $ git remote set-url origin git@github.com:USERNAME/REPOSITORY.git
   ```

5. 验证远程 URL 是否已更改。

   ```shell
   $ git remote -v
   # Verify new remote URL
   > origin  git@github.com:USERNAME/REPOSITORY.git (fetch)
   > origin  git@github.com:USERNAME/REPOSITORY.git (push)
   ```

### 参考链接

[更改远程仓库的 URL](https://docs.github.com/cn/free-pro-team@latest/github/using-git/changing-a-remotes-url)

