---
title: 利用 Hexo 和 Github/gitee/云服务器搭建个人博客
date: 2019.12.20
tags:
- hexo
- blog
- github
categories: technology
---

使用 Hexo 搭建静态网站，结合 Git 命令和 Markdown 编辑文章，将会使得个人记录灵感、撰写笔记非常自然。

<!--more-->

# 安装 Node.js + Hexo

此部分参考 [GitHub+Hexo 搭建个人网站详细教程](https://zhuanlan.zhihu.com/p/26625249) 

## 安装 nodejs 和 npm

hexo 的安装需要 nodejs（即 javascript 的一种运行环境，对 Google V8 引擎进行的封装。同时也是一个服务器端的 javascript 的解释器）。而 npm 是 nodejs 的包管理器（package manager）。

需要注意的是，安装 nodejs 后，npm 自动安装。

```shell
# 以Ubuntu 18.04 为例
sudo apt update
sudo apt install nodejs

# 检查是否安装成功
node --version
npm --version
```

## 安装 hexo

```bash
# 使用 npm 命令安装 Hexo，可以认为 hexo 是 nodejs 的一个包
# 全局安装
npm install -g hexo-cli

# 或者局部安装 hexo 包
npm install hexo
```

# 利用 hexo 创建本地博客

可以参考 hexo 官网：[建站](https://hexo.io/zh-cn/docs/setup.html) 

中文网站：[hexo.io](https://hexo.io/zh-cn/) 

```bash
hexo init blog.source
cd blog.source
npm install
hexo server 
# 此时，打开本地浏览器，输入 http://localhost:4000 就可以打开博客网页，不过现在是部署在本地的，可以利用 Git 上传到代码托管仓库或者私人云服务器上，继续下面将进行介绍

# 新建完成后，blog.source 文件夹的目录如下：

.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```
# 更换成 next 主题

如果你不喜欢默认的 landscape 主题，可以下载自己喜欢的主题。使用下面的命令，默认是 clone 到 blog.source\themes 目录下，我这里下载 [next 主题](https://github.com/theme-next/hexo-theme-next) 

```bash
cd blog.source
git clone https://github.com/next-theme/hexo-theme-next.git
```

下载后，需要下面的修改配置，更改为 [next](https://github.com/next-theme/hexo-theme-next) 主题

# 配置博客

修改 _config.yml 文件部分内容，并保存

- 1、 更改站点信息

```shell
# Site
title: J Blog
subtitle: ''
description: '个人技术网址：人工智能和数学'
keywords:
author: Jinzhong Xu
language: en
timezone: 'Asia/Shanghai'
```
- 2、 更改URL

```shell
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: https://xujinzh.github.io
root: /
permalink: :year/:month/:day/:title/
```
- 3 、修改主题

```shell
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next
```
- 4、更改部署

```bash
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: 'git'
  repo: 
  	github: git@github.com:xujinzh/xujinzh.github.io.git,master
  	gitee: git@gitee.com:xu-jinzhong/xu-jinzhong.git,master
  	hexo: git@1.1.1.1:/home/git/hexo.git,master

# 这里添加了两个代码托管网址，分别是国外的 github 和国内的 gitee，而且使用的是 SSH，所以建议首先将本地电脑的 SSH 公钥添加到自己的 github 和 gitee 上，并分别创建仓库。这样每次部署时，都会在两个仓库生成静态网站。需要注意的是 gitee 需要进到网站仓库（下面仓库名）里，找到服务--> Gitee Pages 手动更新网站，自动更新需要收费。

# 个人云服务器配置请参考下面介绍

# github 上创建仓库：
仓库名：xujinzh.github.io
其他默认。注意为了保持一级域名:https://xu-jinzhong.gitee.io/, 仓库名需使用github用户名xujinzh
# gitee 上创建仓库：
仓库名：xu-jinzhong
其他默认：注意为了保持一级域名:https://xujinzh.github.io/, 仓库名需使用gitee用户名xu-jinzhong

# 想要使用 git 更新部署，还需要安装下面的 git 部署命令
```

# 安装 git 部署命令

```bash
npm install hexo-deployer-git --save

# 安装成功后，就可以上传到代码托管网站上了，如GitHub, gitee，个人云服务器（需要下面的配置）
hexo clean && hexo generate && hexo deploy
# 其中 hexo g 可以代替 hexo generate; hexo d 可以代替 hexo deploy

# 当然，也可以先在本地查看网页情况，没有问题再部署到远程仓库上，方法如下
hexo clean && hexo generate && hexo server

# 其中 hexo s 可以代替 hexo server
# 成功后，会在地址：http://localhost:4000 访问到本地博客网页
```

# 安装搜索插件

搜索插件很重要，因为当网站上书写的博文比较多时，想要找到之前的某篇文章，最好使用搜索

```shell
npm install hexo-generator-searchdb --save
```
在文件 _config.yml 中添加如下代码
```shell
# Local search
# Dependencies: https://github.com/flashlab/hexo-generator-search

search:
  path: search.xml
  field: post
  format: html
  limit: 10000
  
```
进一步修改如下代码
```shell
local_search:
  enable: true
  # If auto, trigger search by changing input.
  # If manual, trigger search by pressing enter key or search button.
  trigger: auto
  # Show top n results per article, show all results by setting to -1
  top_n_per_article: 1
  # Unescape html strings to the readable one.
  unescape: false
  # Preload the search data when the page loads.
  preload: true
```
记得保存文件 \themes\next\_config.yml，该节可参考 [hexo - Next 主题添加搜索功能]([https://yashuning.github.io/2018/06/29/hexo-Next-%E4%B8%BB%E9%A2%98%E6%B7%BB%E5%8A%A0%E6%90%9C%E7%B4%A2%E5%8A%9F%E8%83%BD/#comments](https://yashuning.github.io/2018/06/29/hexo-Next-主题添加搜索功能/#comments)) 

# 添加页面

```shell
hexo new page categories
hexo new page tags
hexo new page about
```
编辑 \source\tags\index.md，categories 类似， about 直接在 markdown 里增加需要的内容就好，不需要修改文件头部信息
```shell
---
title: tags
date: 2019-12-20 15:59:36
type: "tags"
---
```

其他标签类似。更多个性化设置请参考 [Hexo-NexT 主题个性优化](https://blog.guanqr.com/study/blog/hexo-theme-next-customization/#) 

# 个人偏好设置

此处是在 themes/next 主题下的配置文件中修改

- 1、主题设置

  ```shell
  # ---------------------------------------------------------------
  # Scheme Settings
  # ---------------------------------------------------------------
  
  # Schemes
  #scheme: Muse
  #scheme: Mist
  scheme: Pisces
  #scheme: Gemini
  ```

  

- 2、菜单设置

  ```shell
  menu:
    home: / || fa fa-home
    about: /about/ || fa fa-user
    tags: /tags/ || fa fa-tags
    categories: /categories/ || fa fa-th
    archives: /archives/ || fa fa-archive
    #schedule: /schedule/ || fa fa-calendar
    #sitemap: /sitemap.xml || fa fa-sitemap
    #commonweal: /404/ || fa fa-heartbeat
  
  ```

# 云服务器部署博客

当不习惯使用或者其他原因（比如仓库是公开的）不喜欢使用代码托管网站时，可以自己搭建代码托管服务器

## 购买服务器

此部分以个人喜好，目前云服务器厂家特别多，建议购买大厂的，一般20G左右的硬盘足够写博文了，考虑到后期的访问量，最好1G内存吧，系统可以选择 Ubuntu、CentOS 等，本文以 Ubuntu为例

## 静态服务器 nginx

nginx 是一款静态 http 服务软件，可以将服务器上的静态资源（如 HTML、图片）通过 http 协议展现到网页上。

安装 nginx

```bash
sudo apt update
sudo apt install nginx
nginx -v
```

此时，如果将云服务器的外网 IP 输入浏览器上访问，能够看到 nginx 安装成功的欢迎页

## 代码版本控制系统 Git

使用 Git 进行版本控制将是非常方便的，能够通过 Git 更新博文

安装 Git

```bash
sudo apt update
sudo apt install git
git --version
```

配置 Git

```bash
git config --global user.name "jinzhongxu"
git config --global user.email "jinzhongxu@outlook.com"
```

## 配置静态网页

一般 nginx 的静态网页内容放在云服务器的 /var/www/html 目录下，我们需要将托管到 GitHub/gitee 上的博客克隆到该目录下，因目录 /var/www/html 所有者是 root，所以，需要切换到 root 用户

```bash
sudo -i
cd /var/www/html
rm index.nginx-debian.html
git clone https://github.com/j/j.github.io.git
cp -r j.github.io/* .
```

此时再次访问网页，发现已经是自己克隆的博文了

如果不想将博文放到目录 /var/www/html 下，可以修改配置

```bash
cd /etc/nginx/sites-available
vim default

# 修改如下的目录为你喜欢的目录，不过注意的是，因为要别人通过网页查看博文但又不能随便修改博文，所以需要设置存放博文的目录权限
root /var/www/html;

# 如 /var/www/html 的默认权限
 /var/www/
total 12
drwxr-xr-x  3 root root 4096 Sep  1 02:05 ./
drwxr-xr-x 14 root root 4096 Sep  1 02:05 ../
drwxr-xr-x 13 root root 4096 Sep  1 02:19 html/

# 修改 html 文件夹所有者和所有组为 git， git
chown -R git:git /var/www/html
```

## 利用 Git 发表新博文

创建 git 用户(root身份创建新用户)

```bash
sudo -i
adduser git
# 如果是 CentOS 用户，记得使用 passwd git 设置 git 用户的密码
usermod -aG sudo git
```

创建裸仓

```bash
# 切换到 git 用户
su git
# 配置 ssh， 将本地的公钥 id_rsa.pub 写入云服务器的 authorized_keys
ssh-keygen -t rsa
 
 
# 切换到本地机器，
# 这个命令是在本地机器上使用，如本地的 wsl
ssh-copy-id git@1.1.1.1


# 切换到远程云服务器，生成裸仓
cd
git init --bare hexo.git
```

## 配置仓库

```bash
cd hexo.git/hooks/
vim post-receive

# 添加如下内容
#!/bin/sh
git --work-tree=/var/www/html --git-dir=/home/git/hexo.git checkout -f

# 添加文件 post-receive 的执行权限
chmod +x post-receive
```

## 配置本地博客

如果上面配置博客时，没有添加云服务器仓库，这里添加最后一行，配置后显示如下，根据自己情况替换我的仓库名

```shell
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: 'git'
  repo: 
  	github: git@github.com:xujinzh/xujinzh.github.io.git,master
  	gitee: git@gitee.com:xu-jinzhong/xu-jinzhong.git,master
  	hexo: git@1.1.1.1:/home/git/hexo.git,master
```



# 遇到的问题

## 在 post 的下一个中显示为 \<i class="fa fa-angle-right">\<i>

解决方案：

修改 /themes/next/layout/_partials/pagination.swig

```html
{%- if page.prev or page.next %}
 <nav class="pagination">
   {{
     paginator({
       prev_text: '<i class="fa fa-angle-left" aria-label="'+__('accessibility.prev_page')+'"></i>',
       next_text: '<i class="fa fa-angle-right" aria-label="'+__('accessibility.next_page')+'"></i>',
       mid_size: 1,
       escape: false
     })
   }}
 </nav>
{%- endif %}
```

