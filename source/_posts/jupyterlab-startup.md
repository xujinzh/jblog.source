---
title: Python jupyter lab 开机自启动
date: 2019-12-20 19:58:24
tags:
- python
- jupyterlab
- ubuntu
categories: technology
---

## generate config 

```bash
jupyter notebook --generate-config
echo "c.NotebookApp.ip = '0.0.0.0'" >> ~/.jupyter/jupyter_notebook_config.py
```

## activate service 开机自启

```bash
vim /etc/systemd/system/jupyter.service
```
添加如下代码，并保存

<!--more-->

```bash
[Unit]
Description=Jupyter Notebook
After=network.target
[Service]
Type=simple
ExecStart=/home/jinzhongxu/miniconda3/bin/jupyter-lab --config=/home/jinzhongxu/.jupyter/jupyter_notebook_config.py --no-browser
User=jinzhongxu
Group=jinzhongxu
WorkingDirectory=/home/jinzhongxu/
Restart=always
RestartSec=10
[Install]
WantedBy=multi-user.target
```

## enable jupyter

```bash
systemctl enable jupyter
systemctl start jupyter
```

其他程序或服务，也可以使用上面的方法，需要注意两点：

1. 系统版本不能太低，如 Ubuntu 14.04等，因为该系统没有 systemd
2. jupyter.service 中的 ExecStart 后面的命令不能使用 ~jinzhongxu，必须使用绝对路径

一个例子：

```bash
vim /etc/systemd/system/frpc.service
```

```shell
[Unit]
Description=frpc daemon
After=network.target

[Service]
Type=simple
ExecStart=/home/jinzhongxu/.frp.local/frpc -c /home/jinzhongxu/.frp.local/frpc.ini
#ExecStart=/home/jinzhongxu/.frp.local/frp # 这种方法不可行，其中 frp 即 frp.sh，里面就是
# nohup /home/jinzhongxu/.frp.local/frpc -c /home/jinzhongxu/.frp.local/frpc.ini > log &
User=jinzhongxu
Group=jinzhongxu
WorkingDirectory=/home/jinzhongxu/.frp.local/
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl start frpc.service
sudo systemctl enable frpc.service
```

