---
title: Hexo 生成错误问题
author: Jinzhong Xu
date: 2020-08-28 14:28:33
tags:
- hexo
- blog
categories: technology
---

Hexo 写完博文后，使用如下命令生成静态页面

```shell
hexo generate
```

出现如下错误

```html
Error: expected end of comment, got end of file
```

解决方案：

<!--more-->

1. 使用命令找到出错的文档是哪一个

   ```shell
   hexo generate --debug
   ```

   

2. 检查文档中是否出现

   ```html
   {#
   ```

   

3. 尝试去除上面的 '{' 和 '#'

参考文献

[起风了](https://www.dyxmq.cn/cloud/hexo/the-hexo-error-expected-end-of-comment.html)

