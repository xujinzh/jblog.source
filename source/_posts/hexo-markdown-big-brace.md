---
title: Hexo 大括号渲染
mathjax: true
author: Jinzhong Xu
date: 2021-11-19 19:28:39
tags:
- hexo
categories:
- technology
---

Hexo 生成 markdown 撰写的博文中含有公式大括号 $\lbrace$ 和 $\rbrace$ 时，发布网站上大括号消失。本篇给出可能的原因，和解决方法。

<!--more-->

# 可能原因

在使用公式编辑大括号时，采用了如下的形式

```markdown
$ E = \min_{v \in V} \{u \in V | w(u,v) > 0\} $
```

$$
E = \min_{v \in V} \{u \in V | w(u,v) > 0\}
$$

# 解决方法

把

```markdown
\{ \}
```

替换为

```markdown
\lbrace \rbrace
```

结果示例

```markdown
$ E = \min_{v \in V} \lbrace u \in V | w(u,v) > 0\rbrace $
```

$$
E = \min_{v \in V} \lbrace u \in V | w(u,v) > 0\rbrace
$$

# 参考链接

1. [Hexo Next主题Mathjax 大括号渲染错误](https://lanlan2017.github.io/blog/14ee6880/)
