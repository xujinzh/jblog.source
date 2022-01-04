---
title: 开源许可协议
author: Jinzhong Xu
mathjax: true
date: 2020-04-30 10:47:57
tags:
- license
- github
categories: technology
---

开源许可协议有很多，常见的如：MIT, BSD, Apache, LGPL, GPL，它们之间的关系可以参见阮一峰的[如何选择开源许可证？](https://www.ruanyifeng.com/blog/2011/05/how_to_choose_free_software_licenses.html)

![](https://raw.githubusercontent.com/xujinzh/archive/master/images/other/open-source-license.png)

<!--more-->

该图是乌克兰程序员 Paul Bagwell 制作的分析图，阮一峰将其转化为中文版。此外，也可以参考GitHub提供的网站[选择一个开源软件协议](http://choosealicense.online/)，英文版有[Choose an open source license](https://choosealicense.com/)获取更多信息。下面对某些开源许可协议（证）进行介绍。

# 没有License

没有 Licence 默认是被版权保护。想让大家使用，就需要选择一个合适的开源许可协议。

# MIT

MIT 是最宽泛、最自由，没有任何限制。可以售卖、可以使用作者名促销等。

# BSD

相对自由，与 Apache 差不多，教 MIT 严格一些，不允许使用作者名促销，需要保护版权。

# GPL

最严格，基于 GPL 协议的代码而开发的代码，以及修改的代码，都必须遵守 GPL 协议发布。GPL 也包含不同的版本，对权力进行适当的放开，其中 GPL  v3 是最严格的、最激进的，它也是 GPL 之父 Richard Stallman 不断在宣传和推进的开源许可协议。

# GUN

GNU 是 Lesser General Public License ，GNU 是一种较宽松通用公共许可证 。

# 给 GitHub 代码仓库添加开源许可协议

## 新创建一个仓库的情况

可以直接在创建仓库时选择自己喜欢的开源许可协议即可。

## 旧仓库增加开源许可协议

1. 进入 GitHub 该仓库主页，点击 “ Create new file ”; 
2. 输入文件名 ” LICENSE ", 然后点击右侧 " Choose a license template " ;
3. 选择协议后，点击右侧的 “ Review and submit ”;
4. 在 “ Commit  new file ” 下 输入 “ Create LICENSE ” 或者你喜欢的描述，点击 “ Commit directly to the master branch. ” 前面的圆点，再点击 “ Commit new file ” 提交即可。  

