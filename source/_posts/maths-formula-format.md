---
title: Github Pages 数学公式显示
date: 2019-12-22 13:44:47
tags:
- maths
- markdown
categories: technology
---

使用GitHub Pages发布博文时，数学公式不能正常显示，有一种方法可以解决，具体的是，添加如下代码到markdown文件的开头，这样发布的博文就可以正常显示了。

<!--more-->

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

