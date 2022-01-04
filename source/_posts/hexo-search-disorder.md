---
title: 关于 HEXO 博客搜索功能不稳定问题
mathjax: true
author: Jinzhong Xu
date: 2021-11-10 16:35:41
tags:
- hexo
- search
categories:
- technology
---

Hexo + GitHub (or VPS) + Markdown 搭配完成博文撰写与发布，记录个人每天学习到的知识以及产生的灵感，是非常方便的。但是，最近发现我的博客出现搜索功能紊乱的问题，有些博文明明存在却搜索不到。因此，经过长时间的探索发现一种解决方法。特此记录下来。

<!--more-->

# 问题描述

在登录到博客网站后，想要通过关键字搜索某篇博文，搜索不到，但是确信博文是存在的。经过一页一页翻找，确实找到了博文。断定搜索功能失效。

尝试，在本地重新生成和部署

```bash
cd blog
hexo clean
hexo generate 
hexo deploy
```

再重新打开博文进行搜索，发现可以使用。但是，写完博文后再次生成部署，发现搜索功能又失效。

# 定位问题

通过网站直接访问搜索网页： https://xujinzh.github.io/search.xml

发现在顶部有红色错误提示，第$i$行和第$j$列，字符编码错误。

在本地打开 `public/search.xml` 的第 $i$ 行和第 $j$ 列，看看它属于哪篇博文。

在博文中检查是否有不合法的字符出现。清除掉它们。

# 重新生成和部署

清理完所有非法字符后，建议先清除，然后再重新生成和部署

```bash
cd blog
# 先清除
hexo clean
hexo g -d
```

之后，在博客上发现搜索功能恢复。重试多次清除、生成和部署后，都能够准确搜索。

# 参考链接

1. [Hexo本地搜索失效](https://blog.csdn.net/Strong997/article/details/104887353)

