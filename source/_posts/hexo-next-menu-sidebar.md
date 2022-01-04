---
title: Next主题菜单栏链接显示乱码问题
author: Jinzhong Xu
date: 2020-03-15 09:43:42
tags:
- hexo
- next
- menu
categories: technology
---

Hexo + Next + Github搭配使用搭建博客是非常流行和方便的，不需要购买服务器和域名，充分发挥GitHub的作用。但是，当我搭建博客时也遇到过关于Next主题的Menu菜单和边框栏设置的问题。不过最后总算解决了，这里记录下来提供给有需要的小伙伴也为自己留个记忆，以备后用。

<!--more-->

# Next的菜单图标显示

1. 打开主题配置文件，**～/themes/next/_config.yml** , 把相应注释去掉，这里选择你要展示的页面。如下：

```swift
menu:
 home: /|| home
 about: /about/|| user
 tags: /tags/|| tags
 categories: /categories/|| th
 archives: /archives/|| archive
 \#schedule: /schedule/ || calendar
 \#sitemap: /sitemap.xml || sitemap
 \#commonweal: /404/ || heartbeat
```

注意，**about, tags, categories**三个页面需要通过**hexo new page "about"、hexo new page "tags"、hexo new page "categories"** 等命令新建，在**～/sources/**下面会出现相应的文件夹，打开文件夹里面的index命名的markdown文件，添加相应的类型，如**type: "about"**，**tags** 文件夹下的markdown文件添加**type: "tags"**，**categories** 文件夹下的markdown文件添加**type: "categories"**。**archives** 页面不需要新建，注视掉上面相应行就好。

另外，注意将原来的 **/ | |** 之间的空格去掉，不然会出现点击页面标签在url地址后多出**%20** .类似于，https://xujinzh.github.io/%20. 

2. 修改如下内容，打开菜单图标功能

```swift
# Enable/Disable menu icons.
menu_icons:
 enable: true
```

# Next的侧边栏图标链接显示

当设置完上面的内容后，菜单图标和链接都可以使用了，但是，在侧边栏地方会出现一些小问题，就是**posts**，点击该链接会打不开网址，出现乱码，这里需要修改**～/themes/next/layout/_macro/sidebar.swig** 里面如下内容：

```swift
<a href="{{ url_for(theme.menu.archives.split('||')[0]) | trim }}">
```

即，将**archives**后面的小括号 **‘)’**，往后面提到 **[0]**后面。

吃水不忘挖井人，参考链接：[解决Hexo博客导航栏链接URL乱码问题](https://blog.csdn.net/fullbug/article/details/103844424)

