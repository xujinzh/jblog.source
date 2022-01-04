---
title: 手动更新 oh-my-zsh
mathjax: true
author: Jinzhong Xu
date: 2021-10-08 15:13:49
tags:
- shell
- zsh
categories:
- technology
- linux
---

我比较喜欢使用 zsh，特别是搭配 [Oh My ZSH](https://ohmyz.sh/)，它有很多方便的功能以及各种主题。但它不会自动更新，特别是 Mac 上很少关闭 Terminal 导致oh-my-zsh 不是最新的。下面介绍一种手动更新 oh-my-zsh 的方法。

<!--more-->

# 手动更新

使用如下命令

```bash
omz update
```

或者

```bash
cd ~/.oh-my-zsh/tools
zsh upgrade.sh
```

# 参考链接

1. [Manually update oh-my-zsh](https://blog.digital-craftsman.de/manually-update-oh-my-zsh/)
2. [How to Manually Update Oh-My-ZSH](https://futurestud.io/tutorials/how-to-manually-update-oh-my-zsh)

