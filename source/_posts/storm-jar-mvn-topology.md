---
title: 使用Maven打包jar包并提交到storm拓扑上运行
date: 2020-01-19 20:05:35
tags:
- java
- maven
- storm
categories: technology
---

使用maven编写Java代码并打包jar包是非常方便的，这篇文章介绍使用maven打包storm应用程序代码并提交到storm集群上的拓扑上运行。

<!--more-->

# 使用idea和maven创建一个storm工程

1. 点击Project

2. 选择Maven

3. 点击Next

4. 输入工程名、GroupId、ArtifactId等，假设这里输入工程名为：com.monkey

5. 默认打开一个pom.xml文件，在里面输入一下配置信息

   1> 打开官网：[Storm Core](https://mvnrepository.com/artifact/org.apache.storm/storm-core)

   选择版本，拷贝如下信息到pom.xml中，注意，需要手动创建\<dependencies>*\</dependencies>，并把下面的内容替换 * 号，另外，如果是本地测试，需要将provided修改为compile，如果是打成jar包并提交storm集群上，请使用provided

   ```shell
   <!-- https://mvnrepository.com/artifact/org.apache.storm/storm-core -->
   <dependency>
       <groupId>org.apache.storm</groupId>
       <artifactId>storm-core</artifactId>
       <version>1.1.1</version>
       <scope>provided</scope>
   </dependency>
   ```

   2> 打开官网：[Apache Maven Shade Plugin](https://maven.apache.org/plugins/maven-shade-plugin/usage.html) ，拷贝如下的配置文件，注意，your.main.class替换为你自己的主函数类名，该插件进行maven打包最同意，直接进入工程目录，然后输入命令 mvn package进行打包，它会将依赖的所有jar打包，包含可能同名的jar包；而使用Apache Maven Assembly Plugin 打包，会覆盖同名的jar包，对于那些不同目录下同名的jar包覆盖可能会造成运行时错误，如hdfs，配置信息，下面给出，打包命令 mvn package assembly:single；对于不需要依赖的程序，可以直接在工程目录下运行命令 mvn package 进行打包

   ```shell
    <build>
           <plugins>
               <plugin>
                   <groupId>org.apache.maven.plugins</groupId>
                   <artifactId>maven-shade-plugin</artifactId>
                   <version>3.2.1</version>
                   <configuration>
                       <transformers>
                           <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                               <mainClass>WordCountTopology</mainClass>
                           </transformer>
                       </transformers>
                   </configuration>
                   <executions>
                       <execution>
                           <phase>package</phase>
                           <goals>
                               <goal>shade</goal>
                           </goals>
                       </execution>
                   </executions>
               </plugin>
           </plugins>
       </build>
   ```

   

6. 写代码

   注意，在构建拓扑时，如果既想在本地调试运行（pom.xml 设置成compile），又想能够提交到远程集群运行（pom.xml设置成provided），构建拓扑类需如下写代码：

   ```java
   Config config = new Config();
   // config.put("wordcount", "wordcount");
   
   try {
       if (args != null && args.length > 0) {
           System.out.println("remote run"); // 远程集群运行
           StormSubmitter.submitTopology(args[0], config, builder.createTopology());
       } else {
           System.out.println("local run"); // 本地运行
           LocalCluster cluster = new LocalCluster();
   
           cluster.submitTopology(TOPOLOGY_NAME, config, builder.createTopology());
           Thread.sleep(10000);
           cluster.killTopology(TOPOLOGY_NAME);
           cluster.shutdown();
       }
   } 
   ```

# 使用idea和maven打包jar包

打包工程到jar包，进入工程目录下，输入命令

```bash
mvn clean
mvn package
```

在./target目录下有**com.monkey-\*.jar**和 original-com.monkey-\*.jar两个jar包，第一个才是我们需要的jar包

# 提交jar包到storm拓扑上 

将刚刚打包的jar包提交到storm的拓扑上，使用如下命令(这里以工程目录下提交，并且storm安装在用户目录下)

```bash
~/storm/bin/storm jar ./target/com.monkey-\*.jar WordCountTopology test
```

参数解释：“~/storm/bin/storm” 表示storm命令，“jar”表示storm命令的参数，运行jar包命令，“./target/com.monkey-\*.jar”表示待提交运行的jar包，“WordCountTopology”表示工程项目的主类入口，注意和上面pom.xml配置参数对应一致，“test"表示提交到集群拓扑上显示的Topology_name

后续，可以通过网页，如localhost:8080打开storm ui，进入topology summary进行管理，如kill等。也可以通过命令行的方式管理，如

```bash
storm -h
```

表示打开storm命令帮助文件，里面有storm命令行介绍及参数列表

```bash
storm list
```

表示查看正在运行的拓扑信息

```bash
storm kill test
```

表示kill掉拓扑名为test的拓扑，更多命令请参考：[命令行操作](https://www.bookstack.cn/read/Storm-Documents/Manual-zh-Command-Line-Client.md)

注意，可以通过配置项来修改拓扑的运行，配置信息的优先级依次为：

defaults.yaml < storm.yaml < 拓扑配置 < 内置型组件信息配置 < 外置型组件信息配置。

# 其他maven打包方法

上面介绍了使用maven的shade插件进行打包，其实还有两种比较简单的打jar包方法，但是，都没有shade插件打包的通用。

## 1. 直接使用maven打包

当工程代码不需要依赖外部jar包时，同时在pom.xml文件中，指定了程序的主入口，就可以在工程目录下，使用命令

```bash
mvn clean
mvn package 
```

进行打包

## 2. 使用maven assembly 插件打包

对于大多数带有额外jar包的程序或工程，都可以使用assembly进行打包，只要额外jar包中不出现同名的不同功能的jar包，需要在pom.xml文件中，写入如下配置信息，注意修改主类入口函数名

```bash
<plugin>
    <artifactId>maven-assembly-plugin</artifactId>
    <configuration>
        <descriptorRefs>
            <descriptorRef>jar-with-dependencies</descriptorRef>
        </descriptorRefs>
        <archive>
            <manifest>
                <mainClass>your.main.class</mainClass>
            </manifest>
        </archive>
    </configuration>
</plugin>
```

进入工程目录，输入如下命令进行打包

```bash
mvn clean
mvn package assembly:single
```

需要的jar包是带有依赖的那个

## 3.使用maven shade插件打包

该方法是推荐的方法，对于任何情况的工程都可以使用，而且打包命令简单。请参考上面的pom.xml配置文件信息，并进入工程输入命令

```bash
mvn clean
mvn package
```

进行打包

本节内容参考了如下文章：

1、[Storm 入门的Demo教程](https://www.cnblogs.com/xuwujing/p/8584684.html)

2、[storm学习之六-使用Maven 生成jar包多种方式](https://www.cnblogs.com/200911/p/5085343.html)