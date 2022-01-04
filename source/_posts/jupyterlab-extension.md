---
title: jupyterlab 扩展
date: 2019-12-31 19:53:54
tags:
- python
- jupyterlab
categories: technology
---

# jupyterlab 代码折叠 - **[Collapsible_Headings](https://github.com/aquirdTurtle/Collapsible_Headings)**

按照 Github 上扩展安装步骤进行安装

```bash
jupyter labextension install @aquirdturtle/collapsible_headings
```

效果如图

<!--more-->

![](https://raw.githubusercontent.com/aquirdTurtle/Collapsible_Headings/master/Demo2.gif)

# jupyterlab 目录 - [TOC](https://github.com/jupyterlab/jupyterlab-toc)

按照 Github 上扩展安装步骤进行安装

<font color='dd00dd'>注意：如果 jupyterlab >= 3.0.0，则自带该插件功能，不需再次安装</font>

```bash
jupyter labextension install @jupyterlab/toc
```

# jupyterlab 代码自动补全 - [Kite](https://github.com/kiteco/jupyterlab-kite)

按照 Github 上扩展安装步骤进行安装

## 安装 Kite Engine

**macOS Instructions**

1. Download the [installer](https://kite.com/download) and open the downloaded `.dmg` file.
2. Drag the Kite icon into the `Applications` folder.
3. Run `Kite.app` to start the Kite Engine.

**Windows Instructions**

1. Download the [installer](https://kite.com/download) and run the downloaded `.exe` file.
2. The installer should run the Kite Engine automatically after installation is complete.

**Linux Instructions**

1. Run `bash -c "$(wget -q -O - https://linux.kite.com/dls/linux/current)"` from the terminal.

2. The installer should run the Kite Engine automatically after installation is complete.

   

## 安装 Kite Extension for JupyterLab

```bash
pip install jupyter-kite
jupyter labextension install "@kiteco/jupyterlab-kite"
```

然后，重启 jupyterlab

# Jupyterlab 代码格式化 - [code formatter](https://github.com/ryantam626/jupyterlab_code_formatter/blob/master/docs/installation.rst)

按照如下步骤安装

<font color='dd00dd'>对于 jupyterlab < 3.0.0, 按照如下方式安装</font>

```bash
jupyter labextension install @ryantam626/jupyterlab_code_formatter
pip install jupyterlab_code_formatter black isort
jupyter serverextension enable --py jupyterlab_code_formatter
```

<font color='dd00dd'>对于 jupyterlab >= 3.0.0，按照如下方式安装</font>

```bash
pip install jupyterlab_code_formatter
jupyter server extension enable jupyterlab_code_formatter
# if you installed the plugin with pip and using --user, you need to add it here as well
# jupyter server extension enable --user jupyterlab_code_formatter
pip install black isort
```

然后，重启 jupyterlab

# 可能遇到的问题

## 无法卸载扩展

如果使用

```bash
jupyter labextension list
```

出现

installed 和 uninstalled 相同的条目，可以移除 build_config.json 来解决。

## 无法安装扩展

因为build jupyterlab 扩展时，需要使用 npm 安装一些插件，所以 npm 按照插件成功与否会直接影响 jupyterlab 扩展 build 的成败。下面介绍如何安装 npm 和配置国内源

```bash
sudo apt update 
sudo apt install nodejs
# 配置淘宝源
npm config set registry https://registry.npm.taobao.org
# 或者使用工具 nrm 管理源，首先安装 nrm
npm install -g nrm
# 查看可用源
nrm ls
# 切换国内源
nrm use taobao
```

这样，再次使用

```bash
jupyter lab build
```

时，就能够 build 成功了。

<font color='dd0000'>注意：在 jupyterlab 3.0.0 以后，部分插件安装不需要 nodejs，可以直接使用 pip 安装，如 @ryantam626/jupyterlab_code_formatter </font>

## 参考链接：

[npm 切换镜像站点](https://www.runoob.com/w3cnote/npm-switch-repo.html)

[NPM 国内慢的问题解决](https://www.runoob.com/w3cnote/npm-slow-use-cnpm.html)