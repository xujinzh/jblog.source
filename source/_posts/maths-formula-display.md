---
title: 数学公式显示
author: Jinzhong Xu
mathjax: true
date: 2020-03-17 14:28:18
tags:
- maths
categories: technology
---

Hexo + Github + Next搭建博客后，有时候需要写一些带有公式的博文。而默认情况下是不能显示数学公式的。这里介绍一种如何在博文上显示数学公式的方法。

<!--more-->

# Typora 本地数学公式编辑

在本地下文章想显示数学公式，推荐使用typora这个软件，可以非常简单的配置后显示数学公式。

## 行内公式

首先设置行内公式编辑功能

文件---偏好设置---Markdown---Markdown扩展语法---内联公式

设置后重启typora

行内公式编辑方法，直接在需要编写数学公式的地方写如下类似内容

```markdown
$\sin(x) = x - \frac{x^3}{3!} + \cdots$ 
```

即，将公式写在**$**包括的里面就行，跟Latex语法一样。

## 居中显示公式

居中显示公式跟Latex语言一样，首先输入**$$**，然后回车，即可进入编辑模式。

或者使用快捷键：Ctrl + Shift + M

或者点击段落---公式块

# 网页显示数学公式

## 开启mathjax

进入目录 **~\themes\next**，编辑**_config.yml** ，修改如下内容

```yaml
# MathJax Support
mathjax:
  enable: true
  per_page: true
  cdn: //cdn.bootcss.com/mathjax/2.7.1/latest.js?config=TeX-AMS-MML_HTMLorMML
```

## post模板添加mathjax

进入目录**~\scaffolds** ，编辑**post.md**，添加**mathjax: true** ，类似于如下内容

```markdown
title: {{ title }}
author: Jinzhong Xu
date: {{ date }}
mathjax: true
tags:
categories:
```

这样，使用命令hexo new maths-formula创建新博文时就会自动加载上mathjax，使得数学公式正常显示。

# 其他

如果不想设置那么复杂，可以直接在想要写的博文前面添加如下内容，使得本博文可以显示数学公式

```html
<head>
    <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
    <script type="text/x-mathjax-config">
        MathJax.Hub.Config({
            tex2jax: {
            skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
            inlineMath: [['$','$']]
            }
        });
    </script>
</head>
```