---
title: jupyterlab安装plotly
author: Jinzhong Xu
mathjax: true
date: 2020-06-22 15:32:57
tags:
- jupyterlab
- plotly
categories: technology
---

Plotly是一家技术计算公司，总部位于魁北克省的蒙特利尔市，致力于开发在线数据分析和可视化工具。Plotly为个人和协作提供了在线图形，分析和统计工具，以及用于Python，R，MATLAB，Perl，Julia，Arduino和REST的科学图形库。本篇介绍的就是其 Python 画图库plotly，常常在jupyterlab使用plotly进行离线画图时总是出现图形不显示的问题。因为plotly画出的图形比matplotlib更加好看且有交互性，同时，matplotlib画的图形也可以使用plotly转化为同态交互图。所以，希望能够使用plotly画图，并能够在常用软件jupyterlab中使用。

<!--more-->

# 安装plotly

推荐使用pip安装最新的plotly，

```shell
pip install plotly
```

# jupyterlab使用plotly

在jupyterlab中使用plotly需要安装一些支持扩展，其中 jupyterlab-plotly@ 需要根据最新的plotly版本对应，参考网址[Python\Getting Started with Plotly](https://plotly.com/python/getting-started/?utm_source=mailchimp-jan-2015&utm_medium=email&utm_campaign=generalemail-jan2015&utm_term=bubble-chart)
```shell
pip install ipywidgets
```

```shell
conda install nodejs

# JupyterLab renderer support
jupyter labextension install jupyterlab-plotly@4.8.1
```

在  jupyterlab-plotly@4.8.1中，需要编译一会，可能会消耗2G多内存，建议内存小的用户创建交换分区。

```shell
# 查看安装的jupyterlab扩展
jupyter labextension list
```

# 使用plotly画图

按照上面方法按照后，需要先 log out jupyterlab，然后，在登录进去使用plotly才能正常显示图形。

更多jupyterlab使用plotly画图，请参考[官网](https://plotly.com/python/getting-started/)

# 无法安装扩展解决办法

因为 build jupyterlab 扩展时，需要使用 npm 安装一些插件，所以 npm 按照插件成功与否会直接影响 jupyterlab 扩展 build 的成败。下面介绍如何安装 npm 和配置国内源

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

