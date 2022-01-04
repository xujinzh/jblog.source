---
title: 在 Windows10 上配置 Java JDK 14 开发环境
mathjax: true
author: Jinzhong Xu
date: 2021-03-30 14:53:23
tags:
- windows10
- java
categories:
- technology
- java
---

Java 是一种广泛使用的计算机编程语言，拥有跨平台、面向对象、泛型编程的特性，广泛应用于企业级 Web 应用开发和移动应用开发。任职于 Sun 微系统的詹姆斯·高斯林等人于1990年代初开发Java语言的雏形，最初被命名为 Oak，目标设置在家用电器等小型系统的编程语言，应用在电视机、电话、闹钟、烤面包机等家用电器的控制和通信。随着1990年代互联网的发展，Sun 公司看见 Oak 在互联网上应用的前景，于是改造了 Oak，于1995年5月以 Java 的名称正式发布。后来 Sun 公司被甲骨文公司并购，Java 也随之成为甲骨文公司的产品。Java 伴随着互联网的迅猛发展而发展，逐渐成为重要的网络编程语言。Java 编程语言的风格十分接近 C++ 语言。继承了 C++ 语言面向对象技术的核心，舍弃了容易引起错误的指针，以引用取代；移除了 C++ 中的运算符重载和多重继承特性，用接口取代；增加垃圾回收器功能。Java 不同于一般的编译语言或解释型语言。它首先将源代码编译成字节码，再依赖各种不同平台上的虚拟机来解释执行字节码，从而具有“一次编写，到处运行”的跨平台特性。现时，移动操作系统 Android 大部分的代码采用 Java 编程语言编程。

总之，Java 编程语言是个简单、面向对象、分布式、解释性、健壮、安全与系统无关、可移植、高性能、多线程和动态的语言。下面介绍如何在 Windows10 上配置 Java JDK 14 的环境变量。

<!--more-->

# 下载 Java JDK 14

## JDK

打开官网： https://www.oracle.com/java/technologies/javase/jdk14-archive-downloads.html

下载需要的 Windows 版本。

## MAVEN

在使用 Java 编写程序时，最习惯使用 Maven 进行工程构建与管理，因此，这里给出下载 Maven 的地址：https://maven.apache.org/download.cgi#

# 安装 Java JDK 14

## JDK

直接点击安装，默认安装即可。在安装后，只有 jdk 而没有 JRE，这是与 Java 8 不同的。并且，在 lib 里面也没有 tools.jar

## MAVEN

把 Maven 解压，复制到需要的目录下，如我这里是： C:\Program Files\apache-maven-3.6.3

# 配置 Java 环境

## JDK

右键点击此电脑---》属性---》高级系统设置---》环境变量---》系统变量---》新建---》变量名写 “JAVA_HOME”---》变量值写Java JDK 14安装目录，我这里是“C:\Program Files\jdk-14.0.2” ---》确定

系统变量---》Path---》编辑---》新建---》%JAVA_HOME%\bin---》确定---》确定---》确定

## MAVEN

Maven 的配置同 Java JDK

# 测试环境变量

## Java

win + r 打开运行，输入 `cmd`，打开命令行提示符，输入`java --version` 如下

```bash
C:\Users\jinzhongxu>java --version
java 14.0.2 2020-07-14
Java(TM) SE Runtime Environment (build 14.0.2+12-46)
Java HotSpot(TM) 64-Bit Server VM (build 14.0.2+12-46, mixed mode, sharing)
```

## Maven

win + r 打开运行，输入`cmd`，打开命令行提示符，输入 `mvn -v` 如下

```bash
C:\Users\jinzhongxu>mvn -v
Apache Maven 3.6.3 (cecedd343002696d0abb50b32b541b8a6ba2883f)
Maven home: C:\Program Files\apache-maven-3.6.3\bin\..
Java version: 14.0.2, vendor: Oracle Corporation, runtime: C:\Program Files\jdk-14.0.2
Default locale: zh_CN, platform encoding: GBK
OS name: "windows 10", version: "10.0", arch: "amd64", family: "windows"
```

# Maven 镜像配置

因 Maven 默认镜像在国内下载限制，因此，建议更改为阿里云的镜像，方法如下：（建议修改 **settings.xml** 前先复制保持一份）

打开文件 **C:\Program Files\apache-maven-3.6.3\conf\settings.xml**，编辑为如下：

需要说明的是：建议把修改好的 settings.xml 拷贝到 C:\Users\jinzhongxu\.m2 目录下一份。

```xml
<!-- mirrors
   | This is a list of mirrors to be used in downloading artifacts from remote repositories.
   |
   | It works like this: a POM may declare a repository to use in resolving certain artifacts.
   | However, this repository may have problems with heavy traffic at times, so people have mirrored
   | it to several places.
   |
   | That repository definition will have a unique id, so we can create a mirror reference for that
   | repository, to be used as an alternate download site. The mirror site will be the preferred
   | server for that repository.
   |-->
<mirrors>
    <!-- mirror
     | Specifies a repository mirror site to use instead of a given repository. The repository that
     | this mirror serves has an ID that matches the mirrorOf element of this mirror. IDs are used
     | for inheritance and direct lookup purposes, and must be unique across the set of mirrors.
     |
    <mirror>
      <id>nexus-osc</id>
      <mirrorOf>*</mirrorOf>
      <name>Nexus osc</name>
      <url>http://maven.aliyun.com/nexus/content/repositories/central</url>
    </mirror>
     -->
    <mirror>
        <id>nexus-osc</id>
        <mirrorOf>*</mirrorOf>
        <name>Nexus osc</name>
        <url>http://maven.aliyun.com/nexus/content/repositories/central</url>
    </mirror>
</mirrors>


<profiles>
    <!-- profile
     | Specifies a set of introductions to the build process, to be activated using one or more of the
     | mechanisms described above. For inheritance purposes, and to activate profiles via <activatedProfiles/>
     | or the command line, profiles have to have an ID that is unique.
     |
     | An encouraged best practice for profile identification is to use a consistent naming convention
     | for profiles, such as 'env-dev', 'env-test', 'env-production', 'user-jdcasey', 'user-brett', etc.
     | This will make it more intuitive to understand what the set of introduced profiles is attempting
     | to accomplish, particularly when you only have a list of profile id's for debug.
     |
     | This profile example uses the JDK version to trigger activation, and provides a JDK-specific repo.
    <profile>
      <id>jdk-1.4</id>

      <activation>
        <jdk>1.4</jdk>
      </activation>

      <repositories>
        <repository>
          <id>jdk14</id>
          <name>Repository for JDK 1.4 builds</name>
          <url>http://www.myhost.com/maven/jdk14</url>
          <layout>default</layout>
          <snapshotPolicy>always</snapshotPolicy>
        </repository>
      </repositories>
    </profile>
    -->

    <!--
     | Here is another profile, activated by the system property 'target-env' with a value of 'dev',
     | which provides a specific path to the Tomcat instance. To use this, your plugin configuration
     | might hypothetically look like:
     |
     | ...
     | <plugin>
     |   <groupId>org.myco.myplugins</groupId>
     |   <artifactId>myplugin</artifactId>
     |
     |   <configuration>
     |     <tomcatLocation>${tomcatPath}</tomcatLocation>
     |   </configuration>
     | </plugin>
     | ...
     |
     | NOTE: If you just wanted to inject this configuration whenever someone set 'target-env' to
     |       anything, you could just leave off the <value/> inside the activation-property.
     |
    <profile>
      <id>env-dev</id>

      <activation>
        <property>
          <name>target-env</name>
          <value>dev</value>
        </property>
      </activation>

      <properties>
        <tomcatPath>/path/to/tomcat/instance</tomcatPath>
      </properties>
    </profile>
    -->
	<profile>
      <id>development</id>
      <activation>
        <jdk>14</jdk>
        <activeByDefault>true</activeByDefault>
      </activation>
      <properties>
        <maven.compiler.source>14</maven.compiler.source>
        <maven.compiler.target>14</maven.compiler.target>
        <maven.compiler.compilerVersion>14</maven.compiler.compilerVersion>
      </properties>
    </profile>

  </profiles>
```

# 参考链接

1. [永久解决 Intellij idea 报错：Error : java 不支持发行版本5](https://blog.csdn.net/qq_42583206/article/details/108375173)

   