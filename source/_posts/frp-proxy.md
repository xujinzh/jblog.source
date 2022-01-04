---
title: 反向代理工具 frp
date: 2020-02-17 10:49:30
tags:
- ubuntu
- frp
categories: technology
---

当想在家或手机端使用局域网内的高性能服务器时，因为网络的限制而不能直接连接局域网内的服务器，所以可以使用一个具有公网IP的远程服务器来作为中间连接服务器，进行反向代理来达到目的。

<!--more-->

首先，假设局域网服务器是 Ubuntu 系统的 PowerEdge，远程服务器是一个 Debian 系统的 AWS 服务器 JX，其次，需要从 Github 下载反向代理软件 [frp](https://github.com/fatedier/frp) 

## 在 JX 上，下载 frp，并解压，修改配置文件，最后运行服务

```bash
wget https://github.com/fatedier/frp/releases/download/v0.31.2/frp_0.31.2_linux_amd64.tar.gz
tar -xzf frp*
rm frp*gz
mv frp* frp
cd frp
vim frps.ini
```

添加如下内容，并运行如下命令

```shell
[common]
bind_port = 7000
vhost_http_port = 8080
```

```bash
nohup ./frps -c frps.ini > log &
```

或者配置开机自启，方法如下

```bash
sudo -i
vim /etc/systemd/system/frps.service

# 添加如下代码；使用 local-fs.target 似乎可预防掉线
[Unit]
Description=frps daemon
After=local-fs.target
#After=network.target

[Service]
Type=simple
ExecStart=/home/jinzhongxu/.frp/frps -c /home/jinzhongxu/.frp/frps.ini
User=jinzhongxu
Group=jinzhongxu
WorkingDirectory=/home/jinzhongxu/
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target

# 查看状态并开启、设置开机自启
systemctl status frps.service
systemctl start frps.service
systemctl status frps.service
systemctl enable frps.service
```



## 在 PowerEdge 上，同样下载 frp，并解压，修改配置文件，最后启动局域网服务

```bash
wget https://github.com/fatedier/frp/releases/download/v0.31.2/frp_0.31.2_linux_amd64.tar.gz
tar -xzf frp*
rm frp*gz
mv frp* frp
cd frp
vim frpc.ini
```

添加如下代码

```shell
[common]
server_addr = www.jx.com
server_port = 7000

[ssh]
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = 6000

[web]
type = http
local_port = 8080
custom_domains = www.jx.com
```

运行如下代码，开启局域网服务

```bash
nohup ./frpc -c frpc.ini > log &
```

## 在 WSL 上，同样下载 frp，并解压，修改配置文件，最后启动局域网服务

```bash
wget https://github.com/fatedier/frp/releases/download/v0.31.2/frp_0.31.2_linux_amd64.tar.gz
tar -xzf frp*
rm frp*gz
mv frp* frp
cd frp
vim frpc.ini
```

添加如下代码

```shell
[common]
server_addr = www.jx.com
server_port = 7000

[ssh2]
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = 6001
```

至此，就可以在家，通过命令以 JX 为中继远程连接局域网的服务器 PowerEdge/WSL 了，需要注意的是，远程服务器端一定要打开端口：7000， 6000， 6001， 8080等

```bash
ssh www.jx.com -p 6000
ssh www.jx.com -p 6001
```

也可以打开网址，

www.jx.com:8080

来访问局域网 8080 端口的服务