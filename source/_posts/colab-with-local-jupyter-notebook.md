---
title: Colab 使用本地 Jupyter Notebook 内核运行时
mathjax: true
author: Jinzhong Xu
date: 2020-09-01 21:44:06
tags:
- colab
- jupyter
categories: 
- technology
- python
---

众所周知，Colab 是一个美观、工具齐全、编写 Python 代码方便的网页版 notebook，结合本地运行时能够提高代码开发效率。注意本篇使用的 Jupyterlab 版本小于3.

<!--more-->

设置前注意事项：

- 调用任意命令（例如“`rm -rf /`”）
- 访问本地文件系统
- 在计算机上运行恶意内容

# 使用本地 Kernel

如果想使用 colab + 本地运行时，请按照如下步骤设置：

1. 安装 Jupyter

   建议先安装 Miniconda

   ```bash
   pip install jupyterlab
   # or
   conda install -c conda-forge jupyterlab
   # or 
   mamba install -c conda-forge jupyterlab
   ```

   安装后，建议[生成配置文件](https://jupyter-notebook.readthedocs.io/en/stable/config.html)

   ```bash
   jupyter notebook --generate-config
   ```

   对于 [JupyterLab 版本大于3 的](https://jupyter-server.readthedocs.io/en/latest/other/full-config.html), 推荐使用

   ```python
jupyter server --generate-config
   ```
   
   

2. 安装 jupyter_http_over_ws

   ```bash
   pip install jupyter_http_over_ws
   
   # 启用 jupyter_http_over_ws
   jupyter serverextension enable --py jupyter_http_over_ws
   ```

   

3. 启动服务器并进行身份验证

   ```bash
   jupyter notebook \
     --NotebookApp.allow_origin='https://colab.research.google.com' \
     --port=8888 \
     --NotebookApp.port_retries=0
     
   # 后面的三个配置都可以在 ～/.jupyter/jupyter_notebook_config.py 中进行设置
   # 这样就不用每次启动 jupyter notebook 时手动配置，设置后启动方式如下
   # 如果没有文件 jupyter_notebook_config.py，可以使用命令 jupyter notebook --generate-config 生成
   nohup jupyter notebook > /dev/null 2>&1 &
     
   # 查看 jupyter notebook 的 token 可使用如下代码
   jupyter notebook list
   
   # 关闭运行的 jupyter notebook，使用如下命令
   jupyter notebook stop 8888
   ```

   

4. 连接到本地运行时

   在 Colaboratory 中，点击“连接”按钮，然后选择“连接到本地代码执行程序…”。在随即显示的对话框中输入上一步中的网址，然后点击“连接”按钮。完成此操作后，您应该就已经连接到本地运行时了。

5. 卸载

   您可以通过运行以下命令停用和移除 jupyter_http_over_ws jupyter 扩展程序：

   ```bash
   jupyter serverextension disable --py jupyter_http_over_ws
   pip uninstall jupyter_http_over_ws
   ```

# 使用远程服务器的 Kernel

如果想要使用远程主机或 vps 的 jupyter notebook，可以先使用终端将远程主机的 jupyter notebook 的 8888 端口映射到本地 localhost 的 8888 端口，方法如下：

```bash
ssh -L 8888:localhost:8888 jinzhongxu@3.2.1.0
# 也可以使用如下代码，不打开远程终端
ssh -f -N -L 8888:localhost:8888 jinzhong@3.2.1.0
```

