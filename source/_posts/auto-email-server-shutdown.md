---
title: 服务器关机自动发送通知邮件
mathjax: true
author: Jinzhong Xu
date: 2021-11-29 15:09:53
tags:
- python
- linux
- ubuntu
- mail
categories:
- technology
- python
---

我这里有个需求，就是服务器会因为不知道的原因（排除断电）导致关机，这种情况下，如果不能及时知道何时关机将会导致不能及时进行重启，并进行安全检查。这里通过撰写 Python 代码并设置服务，监测服务器是否关机并自动邮件通知。本篇以 Ubuntu 为例。

<!--more-->

# 编写 python 代码

编写 /home/jinzhongxu/shutdown_msg.py 模块

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Author  : Jinzhong Xu
# @Contact : jinzhongxu@csu.ac.cn
# @Time    : 2021/11/23 17:45
# @File    : shutdown_msg.py
# @Software: PyCharm

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

# ubuntu设置关机时自动执行任务 https://blog.csdn.net/xiaohu50/article/details/79268538


if __name__ == '__main__':
    subject = "服务器关机"
    content = "服务器关机了"
    mail(subject=subject, text=content)
```

# 设置守护程序

创建服务

```bash
sudo vim /etc/systemd/system/mailshutdown.service
```

添加如下内容

```bash
[Unit]
Description=Run command at shutdown
# 假设要执行的命令依赖网络
Requires=network.target
DefaultDependencies=no
Conflicts=reboot.target
Before=shutdown.target

[Service]
Type=oneshot
RemainAfterExit=true
ExecStart=/bin/true
ExecStop=/usr/local/miniconda/bin/python /home/jinzhongxu/shutdown_msg.py

[Install]
WantedBy=multi-user.target
```

设置开机启动

```bash
sudo systemctl start mailshutdown.service
sudo systemctl enable mailshutdown.service
```

这样，当某人使用

```bash
sudo shutdown -h now
```

等命令关机时，将会收到邮件通知。

# 参考链接

1. [ubuntu设置关机时自动执行任务](https://blog.csdn.net/xiaohu50/article/details/79268538)
2. [服务器开机或IP改变自动发送通知邮件](https://xujinzh.github.io/2021/11/06/auto-email-server-ip)
