---
title: hexo博客评论系统gitalk
author: Jinzhong Xu
date: 2020-03-14 10:25:47
tags:
- hexo
- gitalk
categories: technology
---

Hexo结合GitHub进行博客系统搭建，能够免费的畅享GitHub给我们带来的无限量服务。下面假设

1. hexo博客已经搭建好了
2. 主页地址是：https://xujinzh.github.io/
3. 并且在GitHub上创建了gitalk仓库：https://github.com/xujinzh/gitalk-comments.git 

<!--more-->

# 注册新应用

打开网址：https://github.com/settings/applications/new

- 在**Application name**一栏填写应用名称，可随意填写，但必须填写
- **Homepage URL**一栏填写博客主页，必填，我这里是：https://xujinzh.github.io/
- Application description一栏是对该应用的描述信息，可以选填
- **Authorization callback URL**一栏是应用程序的回调URL，也是博客主页，必填，我这里是：https://xujinzh.github.io/

点击注册后，会跳转到应用信息页，显示有该应用的用户**Client ID**和**Client Secret**，这两个参数在下面配置项中将用到，请保持打开该页面，方便进行一下的配置。

# 配置gitalk

## 新方法

推荐使用最新配置的方法，只需要配置主题里面的 `_config.xml` 或 主目录下的  `_config.next.yml`

```yaml
# Gitalk
# For more information: https://gitalk.github.io
gitalk:
  enable: true
  github_id: xujinzh # GitHub repo owner
  repo: xujinzh.github.io # Repository name to store issues
  client_id: 6**5 # GitHub Application Client ID
  client_secret: 9**5 # GitHub Application Client Secret
  admin_user: xujinzh # GitHub repo owner and collaborators, only these guys can initialize gitHub issues
  distraction_free_mode: true # Facebook-like distraction free mode
  # When the official proxy is not available, you can change it to your own proxy address
  proxy: https://cors-anywhere.azm.workers.dev/https://github.com/login/oauth/access_token # This is official proxy address
  # Gitalk's display language depends on user's browser or system environment
  # If you want everyone visiting your site to see a uniform language, you can set a force language value
  # Available values: en | es-ES | fr | ru | zh-CN | zh-TW
  language: en
```

参考：

1. [Hexo 集成 Gitalk 评论系统](https://www.jianshu.com/p/02fc71f3633f)

## 旧方法

这里**“~”**表示博客主目录

1. 在目录**~\themes\next\layout\_third-party\comments**下创建`gitalk.swig` 文件，并插入如下内容

   ```swift
   {% if page.comments && theme.gitalk.enable %}
     <link rel="stylesheet" href="https://unpkg.com/gitalk/dist/gitalk.css">
   
     <script src="https://unpkg.com/gitalk/dist/gitalk.min.js"></script>
      <script type="text/javascript">
           var gitalk = new Gitalk({
             clientID: '{{ theme.gitalk.ClientID }}',
             clientSecret: '{{ theme.gitalk.ClientSecret }}',
             repo: '{{ theme.gitalk.repo }}',
             owner: '{{ theme.gitalk.githubID }}',
             admin: ['{{ theme.gitalk.adminUser }}'],
             id: location.pathname,
             distractionFreeMode: '{{ theme.gitalk.distractionFreeMode }}'
           })
           gitalk.render('gitalk-container')           
          </script>
   {% endif %}
   ```

2. 修改**~\themes\next\layout\_partials**下的`comments.swig` 文件，添加如下内容的**第5、6行**。

   ```swift
     {% elseif theme.valine.appid and theme.valine.appkey %}
       <div class="comments" id="comments">
       </div>
   
     {% elseif theme.gitalk.enable %}
   	<div id="gitalk-container"></div>
   
     {% endif %}
   
   {% endif %}
   ```

   

3. 修改**~\themes\next\layout\_third-party\comments**下的`index.swig`文件，在最后一行添加如下内容

   ```swift
   {% include 'gitalk.swig' %}
   ```

4. 在目录**~\themes\next\source\css\_common\components\third-party**下新建文件`gitalk.styl`，添加如下内容

   ```swift
   .gt-header a, .gt-comments a, .gt-popup a
     border-bottom: none;
   .gt-container .gt-popup .gt-action.is--active:before
     top: 0.7em;
   ```

5. 修改目录**~\themes\next\source\css\_common\components\third-party**下的`third-party.styl`，再最后一行添加如下内容

   ```swift
   @import "gitalk";
   ```

6. 修改目录**~\themes\next**下的`_config.yml`文件，添加如下内容

   ```swift
   gitalk:
     enable: true
     githubID: xujinzh
     repo: gitalk-comments
     ClientID: Client ID
     ClientSecret: Client Secret
     adminUser: xujinzh  #此项为初始化评论账号，每一篇文章都需要该用户初始化评论后其他人才可以评论
     distractionFreeMode: true
   ```

   该部分可参考：[使用gittalk实现hexo博客评论功能](https://cjjkkk.github.io/gitalk/).

   # busuanzi网站统计

   不蒜子的官网：http://busuanzi.ibruce.info/

   按照官网教程，需要把以下代码添加到文件**~\themes\next\layout\_partials\footer.swig**最后一行，
   
   ```swift
   <script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"></script>
   
   <span id="busuanzi_container_site_pv">本站总访问量<span id="busuanzi_value_site_pv"></span>次</span>
   <span id="busuanzi_container_site_uv">本站访客数<span id="busuanzi_value_site_uv"></span>人次
   </span>
   ```
   
   修改文件：**~\themes\next\_config.yml**的如下内容
   
   ```swift
   busuanzi_count:
     # count values only if the other configs are false
     enable: true
   
     # custom uv span for the whole site
     site_uv: true
     site_uv_header: <i class="fa fa-user"></i>
     site_uv_footer:
   
     # custom pv span for the whole site
     site_pv: true
     site_pv_header: <i class="fa fa-eye"></i>
     site_pv_footer:
   
     # custom pv span for one page only
     page_pv: true
     page_pv_header: <i class="fa fa-file-o"></i>
     page_pv_footer:
   ```
   
   注意，这里**site_pv**和**site_uv** 设置为true时和上面的**本站总访问量**和**本站访客数** 重复，只需设置一处即可。我使用下着，所以，文件**~\themes\next\layout\_partials\footer.swig**最后一行，我只添加了如下内容：
   
   ```swift
   <script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"></script>
   ```
   
   