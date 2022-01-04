---
title: 服务器开机或IP改变自动发送通知邮件
mathjax: true
author: Jinzhong Xu
date: 2021-11-06 17:06:26
tags:
- python
- linux
- ubuntu
- mail
categories:
- technology
- python
---

我这里有个需求，就是服务器会因为断电等原因关机、重启，导致 IP 地址更改，无法通过 SSH 连接，也不能及时知道电脑何时开机。这里通过撰写 Python  代码，自动检测 IP、开机自动邮件通知。本篇以 Ubuntu 为例。

<!--more-->

# 编写 Python 代码

在目录 `/home/jinzhongxu/PythonProjects` 下编写 Python 模块 `send_message.py`

因为在 Ubuntu 系统下，当电脑重启时，目录 `/tmp` 下的文件会清空

在 Mail 函数中，`sender` 为 qq 邮箱，即发送给你通知信息的邮箱；`recipients` 为接收信息的邮箱，可以通过该登录该邮箱查看收到的通知信息，如 IP 更改、服务器重启等；`password` 为 qq 邮箱的授权码，`smtp_server`、`port` 等都需要到 QQ 邮箱进行认证获取；`subject` 为邮件主题；`text` 为邮件内容；`attachment` 为附件，比如图像、文件等。

## 简单通知信息

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Author  : Jinzhong Xu
# @Contact : jinzhongxu@csu.ac.cn
# @Time    : 2021/11/5 19:02
# @File    : send_message.py
# @Software: PyCharm

import json
import os
import socket
import base64
import os
import smtplib
from email import encoders
from email.header import Header
from email.mime.base import MIMEBase
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
from email.utils import formataddr, parseaddr


def get_temp_ip(current_ip):
    """
    检查是否 IP 改变和是否是重启状态
    :param current_ip: 当前 IP 地址
    :return: 是否发送邮件、当前 IP 地址、电脑是否是重启
    """
    reboot = False
    temp_ip_json_path = "/tmp/ip.json"
    if (not os.path.exists(temp_ip_json_path)) or (
            not os.path.getsize(temp_ip_json_path)
    ):
        reboot = True
        print("No {}, dump it.".format(temp_ip_json_path))
        with open(temp_ip_json_path, "w") as jo:
            json.dump(current_ip, jo)
            return True, current_ip, reboot

    else:
        with open(temp_ip_json_path, "r") as jo:
            origin_ip = json.load(jo)
        if origin_ip == current_ip:
            print("Current ip {} do not change, no need to send".format(current_ip))
            return False, current_ip, reboot
        else:
            print(
                "The ip updated from {} to {}, update it.".format(origin_ip, current_ip)
            )
            os.remove(temp_ip_json_path)
            with open(temp_ip_json_path, "w") as jo:
                json.dump(current_ip, jo)
                return True, current_ip, reboot


def get_global_ip():
    """
    获取电脑的外网 IP 地址
    :return: 外网 IP
    """
    try:
        s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
        s.connect(("8.8.8.8", 80))
        ip = s.getsockname()[0]
    finally:
        s.close()

    return ip


def get_status():
    """
    获取电脑状态，包括是否 IP 改变、是否重启
    :return: 是否发送通知邮件
    """
    global_ips = get_global_ip()
    whether_to_send, send_ip, reboot = get_temp_ip(global_ips)
    send_ip = json.dumps(send_ip)
    return whether_to_send, send_ip, reboot


def _format_addr(s):
    name, addr = parseaddr(s)
    return formataddr((Header(name, "utf-8").encode(), addr))


def mail(
        sender="xxx@qq.com",
        password="xxxZHyyy",
        recipients=("jinzhongxu@csu.ac.cn", ),
        smtp_server="smtp.qq.com",
        port=465,
        subject="服务器 IP 改变",
        text="",
        attachment=("",),
):
    msg = MIMEMultipart()
    msg["From"] = _format_addr("JinzhongXu-Pythoner <%s>" % sender)
    msg["To"] = _format_addr("管理员 <%s>" % ", ".join(list(recipients)))
    msg["Subject"] = Header(subject, "utf-8").encode()
    # 邮件正文是MIMEText:
    msg.attach(MIMEText(text, "plain", "utf-8"))

    attachment = list(attachment)
    if attachment != [""]:
        for i, file_path in enumerate(attachment):
            with open(file_path, "rb") as f:
                # 设置附件的MIME和文件名:
                file_dir, file_name = os.path.split(os.path.abspath(file_path))
                filename_extension = file_name.split(".")
                mime = MIMEBase("file", filename_extension[-1], filename=file_name)
                # 加上必要的头信息:
                mime.add_header("Content-Disposition", "attachment", filename=file_name)
                mime.add_header("Content-ID", f"<{i}>")
                mime.add_header("X-Attachment-Id", f"{i}")
                # 把附件的内容读进来:
                mime.set_payload(f.read())
                # 用Base64编码:
                encoders.encode_base64(mime)
                # 添加到MIMEMultipart:
                msg.attach(mime)

    server = smtplib.SMTP_SSL(smtp_server, port)
    # 控制打印日志
    # server.set_debuglevel(2)
    try:
        server.login(
            sender,
            base64.b64decode(password.encode(), altchars=None, validate=False).decode(),
        )
        server.sendmail(sender, list(recipients), msg.as_string())
        logs = f"{sender} 给 {'; '.join(recipients)} 的邮件发送成功"
    except smtplib.SMTPException:
        logs = "Error: 无法发送邮件"
    finally:
        server.quit()
    return logs


if __name__ == "__main__":
    whether_to_send, global_ips, reboot = get_status()
    if whether_to_send and reboot:
        mail(subject="服务器重启成功", text=f"重启后的IP：{global_ips}")
    elif whether_to_send and (not reboot):
        mail(text=f"新的IP：{global_ips}")
    else:
        print("wait and no send")

```

## 详细通知信息

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Author  : Jinzhong Xu
# @Contact : jinzhongxu@csu.ac.cn
# @Time    : 2021/11/12 18:11
# @File    : send_message.py
# @Software: PyCharm

import socket
import base64
import os
import smtplib
from email import encoders
from email.header import Header
from email.mime.base import MIMEBase
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
from email.utils import formataddr, parseaddr

"""
把 IP 地址存储在某个固定的开机不被清空的文件 fixed_ip_file 中；
把 IP 地址存储在某个固定的开机就被清空的文件中，比如 /tmp 文件下的文件 tmp_ip_file；

如果没有则表示刚开机，检测 IP 并写入到文件 tmp_ip_file 中，同时检查当前 IP 是否与 fixed_ip_file 中存储的 IP 是否相同：
    0. 如果 fixed_ip_file 不存在，则把当前 IP 写入该文件，并通知："电脑重启成功，IP 文件丢失，当前 IP 地址是 XXX"；
    1. 如果存在但不同，则发送邮件通知："电脑重启成功，IP 地址改变。旧 IP: xxx，新 IP: yyy"，最后更新 fixed_ip_file 中的 IP；
    2. 如果相同，则发送邮件通知："电脑重启成功，IP 未发生改变，IP 地址是 xxx"；
    
如果有那么表示不是刚开机，并检查当前 IP，如果与文件 tmp_ip_file 中不同，则更新，如果相同则不用重新。
同时检查当前 IP 是否与 fixed_ip_file 中存储的 IP 是否相同：
    0. 如果 fixed_ip_file 不存在，则把当前 IP 写入该文件，并通知："电脑正常运行中，IP 文件丢失，当前 IP 地址是 XXX"；
    1. 如果存在但不同，则发送邮件通知："电脑正常运行中，IP 地址改变。旧 IP: xxx，新 IP: yyy"，最后更新 fixed_ip_file 中的 IP；
    2. 如果相同，则不发送邮件通知； 
    
1. 需要编写检测外网 IP 地址的函数，返回外网 IP 地址
2. 需要编写发送邮件的函数，发送通知
3. 需要指定两个文件地址
4. 需要编写检测电脑状态和 IP 地址状态的函数，检测是否发送邮件、邮件内容
"""


def computer_status():
    # 指定两个文件，存储 IP 地址
    fixed_ip_file = "~/Documents/computerinfo/ip.txt"
    tmp_ip_file = "/tmp/ip.txt"
    fixed_ip_file = os.path.expanduser(fixed_ip_file)
    print(fixed_ip_file)
    send_email = False
    ip = get_global_ip()
    # 如果 /tmp 下没有文件 tmp_ip_file，那么表示电脑刚开机
    if not os.path.exists(tmp_ip_file):  # 此处有误报电脑重启的可能，如认为的删除 tmp_ip_file 文件。本程序默认该中情况不发生。
        send_email = True  # 则一定要发送通知邮件
        with open(tmp_ip_file, 'w') as ip_file:
            ip_file.write(ip)

        # 如果 fixed_ip_file 不存在，则重新创建并写入当前 IP 地址；发送通知邮件
        if not os.path.exists(fixed_ip_file):
            with open(fixed_ip_file, 'w') as ip_file:
                ip_file.write(ip)
            subject = "电脑重启成功，IP 文件丢失"
            content = f"电脑重启成功，IP 文件 {fixed_ip_file} 丢失，当前 IP 地址是 {ip}"
            return send_email, content, subject
        # 如果 fixed_ip_file 存在
        else:
            with open(fixed_ip_file, 'r') as ip_file:
                origin_ip = ip_file.readline()
            # 如果当前 IP 与原 IP 不同，则发送通知邮件
            if ip != origin_ip:
                subject = f"电脑重启成功，IP 地址改变为 {ip}"
                content = f"电脑重启成功，IP 地址改变。旧 IP: {origin_ip}，新 IP: {ip}"
                with open(fixed_ip_file, 'w') as ip_file:
                    ip_file.write(ip)
                return send_email, content, subject
            else:  # 如果当前 IP 与原 IP 相同，则发送通知邮件
                subject = "电脑重启成功，IP 地址未改变"
                content = f"电脑重启成功，IP 未发生改变，IP 地址是 {ip}"
                return send_email, content, subject
    # 如果 /tmp 下有文件 tmp_ip_file，那么表示电脑正常运行
    else:
        with open(tmp_ip_file, 'r') as ip_file:
            origin_ip = ip_file.readline()
        # 如果当前 IP 与 tmp_ip_file 中不一致，则更新 tmp_ip_file 中的 IP
        if ip != origin_ip:
            with open(tmp_ip_file, 'w') as ip_file:
                ip_file.write(ip)
        # 如果 fixed_ip_file 不存在
        if not os.path.exists(fixed_ip_file):
            with open(fixed_ip_file, 'w') as ip_file:
                ip_file.write(ip)
            send_email = True
            subject = f"电脑正常运行中，但 IP 文件丢失"
            content = f"电脑正常运行中，但 IP 文件 {fixed_ip_file} 丢失，当前 IP 地址是 {ip}"
            return send_email, content, subject
        else:  # 如果 fixed_ip_file 存在
            with open(fixed_ip_file, 'r') as ip_file:
                origin_ip = ip_file.readline()
            # 如果当前 IP 与 fixed_ip_file 的不相同
            if ip != origin_ip:
                send_email = True
                subject = f"电脑正常运行中，IP 地址改变为 {ip}"
                content = f"电脑正常运行中，IP 地址改变。旧 IP: {origin_ip}，新 IP: {ip}"
                with open(fixed_ip_file, 'w') as ip_file:
                    ip_file.write(ip)
                return send_email, content, subject
            else:  # 如果当前 IP 与 fixed_ip_file 的相同，则不发生通知邮件；这是大多数正常情况
                subject = "电脑正常运行中，IP 地址未改变"
                content = f"电脑正常运行中，IP 地址未改变，仍然是 {ip}"
                return send_email, content, subject


def get_global_ip():
    """
    获取电脑的外网 IP 地址
    :return: 外网 IP
    """
    s = None
    try:
        s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
        s.connect(("8.8.8.8", 80))
        ip = s.getsockname()[0]
    finally:
        s.close()

    return ip


def _format_addr(s):
    name, addr = parseaddr(s)
    return formataddr((Header(name, "utf-8").encode(), addr))


def mail(
        sender="xxx@qq.com",
        password="xxxZHyyy",
        recipients=("jinzhongxu@csu.ac.cn", "otheruser@163.com"),
        smtp_server="smtp.qq.com",
        port=465,
        subject="服务器 IP 地址改变",
        text="",
        attachment=("",),
):
    msg = MIMEMultipart()
    msg["From"] = _format_addr("JinzhongXu-Pythoner <%s>" % sender)
    msg["To"] = _format_addr("管理员 <%s>" % ", ".join(list(recipients)))
    msg["Subject"] = Header(subject, "utf-8").encode()
    # 邮件正文是MIMEText:
    msg.attach(MIMEText(text, "plain", "utf-8"))

    attachment = list(attachment)
    if attachment != [""]:
        for i, file_path in enumerate(attachment):
            with open(file_path, "rb") as f:
                # 设置附件的MIME和文件名:
                file_dir, file_name = os.path.split(os.path.abspath(file_path))
                filename_extension = file_name.split(".")
                mime = MIMEBase("file", filename_extension[-1], filename=file_name)
                # 加上必要的头信息:
                mime.add_header("Content-Disposition", "attachment", filename=file_name)
                mime.add_header("Content-ID", f"<{i}>")
                mime.add_header("X-Attachment-Id", f"{i}")
                # 把附件的内容读进来:
                mime.set_payload(f.read())
                # 用Base64编码:
                encoders.encode_base64(mime)
                # 添加到MIMEMultipart:
                msg.attach(mime)

    server = smtplib.SMTP_SSL(smtp_server, port)
    # 控制打印日志
    # server.set_debuglevel(2)
    try:
        server.login(
            sender,
            base64.b64decode(password.encode(), altchars=None, validate=False).decode(),
        )
        server.sendmail(sender, list(recipients), msg.as_string())
        logs = f"{sender} 给 {'; '.join(recipients)} 的邮件发送成功"
    except smtplib.SMTPException:
        logs = "Error: 无法发送邮件"
    finally:
        server.quit()
    return logs


if __name__ == "__main__":
    send_email, content, subject = computer_status()
    if send_email:
        mail(subject=subject, text=content)
    else:
        pass
```



# 配置定时检查

```bash
crontab -e
# 添加如下内容，每分钟进行 IP 检查和是否是重启状态
*/1 * * * * /usr/local/miniconda/bin/python /home/jinzhongxu/PythonProjects/send_message.py
```

# 参考链接

1. [linux服务器开机发送本机ip地址到指定邮箱](https://zhuanlan.zhihu.com/p/338190964)
2. [Python 获取本机内网IP](https://www.cnblogs.com/lianshuiwuyi/p/11636876.html)
3. [Linux临时目录/tmp与/var/tmp](https://zhuanlan.zhihu.com/p/352642228)

