---
title: Jupyterlab Code Formatter 的快捷键 Keyboard Shortcuts 设置
mathjax: true
author: Jinzhong Xu
date: 2021-11-11 09:19:51
tags:
- jupyter
- python
categories:
- technology
- python
---

Jupyterlab Code Formatter 能够在 notebook 中对代码进行格式化，安装成功后，只需要点击代码格式化按钮。那么如何像在 pycharm 中一样使用快捷键对代码进行格式化呢，本篇记录一下。

<!--more-->

# 安装格式化包

1. 安装插件

   ```bash
   pip install jupyterlab_code_formatter
   ```

2. 安装格式化支持

   ```bash
   pip install black isort
   ```

   

3. 重启 Jupyterlab

# 设置快捷键

打开 Jupyterlab，点击 settings -> Advanced Settings Editor -> Keyboard Shortcuts

在 User Preferences 中输入

```bash
    "shortcuts": [
        {
            "command": "jupyterlab_code_formatter:black",
            "keys": [
                "Ctrl Alt L"
            ],
            "selector": ".jp-Notebook.jp-mod-editMode"
        }
    ]
```

我这里设置的快捷键是 `Ctrl Alt L`

# 参考链接

1. [jupyterlab-code-formatter Prerequisites and Installation Steps](https://jupyterlab-code-formatter.readthedocs.io/en/latest/installation.html#installation-step-1-installing-the-plugin-itself)

