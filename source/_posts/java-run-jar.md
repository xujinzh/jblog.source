---
title: 使用 Java 命令运行 jar 包
mathjax: true
author: Jinzhong Xu
date: 2021-03-30 15:51:29
tags:
- java
- jar
categories:
- technology
- java
---

Java 使用 Maven 等 打包好程序为 Jar 后，如何运行，这里根据 Jar 包中是否有主类 （main class）入口，来分两种方法运行。

<!--more-->

# Maven 打包 Java 代码

这种方法打包的 JAR 包默认不包含主类入口

```bash
# 在工程代码主目录下
mvn clean package
# clean 是清除之前打包的残留
# package 是打包
```

# 利用 Artifacts 打包带有主类入口的 Jar 包

File---》Project Structure---》Artifacts---》+---》JAR--->from module with dependencies--->Main Class--->copy to the output directory and link via manifest--->Directory for META-INF/MANIFEST.MF 选择 resources文件夹---》ok--->ok--->Build--->Build Artifacts--->Build

当不选主类或者 Directory for META-INF/MANIFEST.MF 选择默认的 java 文件夹时，都不会包含主类入口。

# Jar 包中有主类入口

```bash
java -jar target\java-learn-1.0-SNAPSHOT.jar
```

# Jar 包中没有主类入口

```bash
java -cp target\java-learn-1.0-SNAPSHOT.jar com.mathscv.App
# 这里 com.mathscv 为包名或者 groupId，App 为主类
```

# 参考链接

1. [java命令如何运行jar包](https://blog.csdn.net/SweetTool/article/details/73826121)

   