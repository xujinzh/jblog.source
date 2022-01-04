---
title: Python为pip使用其他源来安装包
date: 2020-02-19 10:37:24
tags:
- python
- pip
categories: technology
---

Python 是一种非常优秀的编程语言，特别是在人工智能中研发中使用很多，Python包索引（缩写为PyPI，也称为Cheese Shop）是Python的官方第三方软件存储库。其使用pip命令来管理软件安装。

<!--more-->

有时候使用pip安装软件时，会提示链接超时，这时可以采用切换源的方式来安装软件，切换到国内的源能够很快速的成功安装这些软件。

下面列出可用的源

1. https://pypi.tuna.tsinghua.edu.cn/simple 清华源（推荐，该镜像每 5 分钟同步一次。）
2. https://mirrors.aliyun.com/pypi/simple 阿里云源
3. https://pypi.doubanio.com/simple 豆瓣源
4. https://mirrors.163.com/pypi/simple 网易源 （每24小时更新一次）
5. https://mirrors.cloud.tencent.com/pypi/simple 腾讯云源
6. https://mirrors.bfsu.edu.cn/pypi/web/simple 中科大源

# 使用方法

## 临时使用国内源

```bash
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple some-package
```

## 永久使用设置默认源

### 命令行设置默认源

```bash
pip install pip -U
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

如果您到 pip 默认源的网络连接较差，临时使用本镜像站来升级 pip：

```markdown
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pip -U
```

### 手动修改配置文件

Linux 系统配置如下

```bash
# 修改 pip.conf 文件 (没有就创建一个)
vim ~/.config/pip/pip.conf

# 添加如下内容
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```

Mac 系统配置如下

```bash
# 修改 pip.conf 文件
vim ~/Library/Application Support/pip/pip.conf

# 如果没有上面的目录, 在如下目录创建 pip.conf 文件
vim ~/.config/pip/pip.conf

# 添加如下内容
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```

Windows 系统配置如下

```bash
# 修改 pip.conf 文件
%APPDATA%\pip\pip.ini

# 添加如下内容
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```

<font size=3 color=cyan>修改文件后，执行命令发生错误</font>

```bash
# 使用非 HTTPS 加密源，在执行命令发生错误，在命令最后加上 `--trusted-host pypi.douban.com`
pip install django -i http://pypi.douban.com/simple --trusted-host pypi.douban.com
# 建议使用 HTTPS 加密源
```

### 使用包管理工具设置

```bash
pip install pip-setting
pip-setting -s qinghua
```

# 设置代理

```shell
pip install pysocks
# 安装文件里列出的软件
pip install -r requirements.txt --proxy='socks5://127.0.0.1:1080'
# 安装某一个软件
pip install torch --proxy socks5://127.0.0.1:1080
```

# 更新所有包

```bash
pip list --outdate --format=freeze | grep -v '^\-e' | cut -d = -f 1 | xargs -n1 pip install -U
```

# 常用  pip 命令

```bash
# 列出 pip 安装的包
pip list
# 列出过时的包
pip list -o
pip list --outdate
# 显示指定的包
pip show numpy scipy
# 升级指定的包
pip install --upgrade numpy scipy
pip install -U numpy scipy
# 升级到指定版本的包
pip install -U numpy==1.21.1
# 安装指定版本的包
pip install numpy==1.21.1
# 卸载包
pip uninstall numpy
```



# 参考链接

1. [How To Update All Python Packages](https://www.activestate.com/resources/quick-reads/how-to-update-all-python-packages/)

2. [网易pypi镜像使用帮助](https://mirrors.163.com/.help/pypi.html)

3. [清华镜像使用帮助](https://mirrors.tuna.tsinghua.edu.cn/help/pypi/)

4. [更换（Pypi）pip源到国内镜像](https://developer.aliyun.com/article/652884)

   
