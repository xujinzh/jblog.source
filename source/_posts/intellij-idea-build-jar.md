---
title: 使用intellij idea 编译项目到jar包
date: 2020-01-02 20:49:05
tags:
- java
- program
categories: technology
---

当我使用 Java 开发 Flink 时，遇到了一个问题，那就是如何使用 Intellij Idea 编译项目成jar包。经过调研，发现有两种方法来编译项目：

<!--more-->

## 使用Intellij Idea编译

1. 点击 File

2. 点击 Project Structure

3. 点击 Artifacts

4. 点击 “+” 号

5. 点击 JAR

6. 点击 From modules with dependencies

7. 选择 Main Class 

8. 点击 Apply && ok

9. 点击 Build

10. 点击 Build Artifacts

11. 点击 Build

    该方法可参考[使用Intellij Idea打包java为可执行jar包](https://blog.csdn.net/xuemengrui12/article/details/74984731)

## 使用Maven编译

在项目工作目录下，运行如下命令

mvn clean package

以上两种方法，都可以编译 Java 项目成jar包，然后就可以提交到 Flink 集群或web下运行。