---
title: Maven 笔记2
tag: Maven
categories:
  - [编程工具,项目管理,Maven] 
  - [后端,Java,Java Web]
---

<h1>Maven-笔记2</h1>

[toc]



# 一、Maven 简介

<u>**Maven**</u> （妹文）可以翻译成“专家”。是 Apache 的一个开源项目，

用于Java平台下的**<u>项目“构建”</u>**、**<u>依赖管理</u>**、**<u>项目信息管理</u>**。



## 1.1、“项目构建” 的概念



编译、运行单元测试、生成文档、打包、部署。这一整套流程就是 **<u>构建（build）</u>**。





## 1.2、“项目构建” 的工具



常见的项目构建工具：

> - `Ant`：最早的构建工具，基于IDE。
> - `Maven：`Java 编写，通过 **<u>项目对象模型</u>** 管理项目，第一个支持从网络下载的概念，XML 作为配置文件。
> - `Gradle`：安卓的管理工具，采用 <u>**DSL**</u>格式 作为配置文件格式。







## 1.3、Maven 的四大特性





### 1.3.1、依赖管理



Maven 用于管理 jar 包。

一个 jar 包的依赖可以通过 <u>**groupId、artifactId、version**</u> 组成的 <u>**坐标（coordination）**</u>来标识。

一个 Maven 项目，本身必须具有 <u>**“gav”坐标**</u>，打包方式可以是 <u>**jar 包**</u>，也可以是 <u>**war 包**</u>。





示例：

```xml
<!--
格式：
        <dependancy0>
            <groupId>公司域名（所属的项目）</groupId>
            <artifactId>项目名（模块名）</artifactId>
            <version>版本号</version>
        </dependancy0> 
-->

        <dependancy>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.1.0</version>
        </dependancy>

```





### 1.3.2、多模块构建



Maven 的POM.xml 配置文件可以有继承的关系。

在使用继承关系时，需要先定义一个 parent POM 作为 一组 module 的聚合 POM。

在  parent POM 中，使用 `<modules>` 来注册 一组子模块，并且父模块的依赖会自动传递给子模块。







### 1.3.3、项目结构一致



约定大于配置，Maven 项目有统一的目录结构。







### 1.3.4、构建模型和插件结构一致



构建模型 和 插件结构 这两者的结构是一致的，都采用了 “gav” 的格式。

```xml
<plugin>
    <groupId>com.mortbay.jetty</groupId>
    <artifactId>maven-jetty-plugin</artifactId>
    <version>6.1.25</version>
    <configuration>
        <sacnIntervalSeconds>10</sacnIntervalSeconds>
        <contextPath>/test</contextPath>
    </configuration>
</plugin>
```







# 二、Maven 安装与配置



## 2.1、Maven 的安装

（1）安装 JDK ，cmd 中检查 JDK 版本 `java -version`。

（2）下载、解压 Maven。

（3）将 Maven 的 bin 目录加入环境变量。

（4）cmd 中，输入 `mvn -verion`，检查是否安装成功。





## 2.2、Maven项目的目录结构



![image-20220829095823674](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220829095823674.png)

<center><span style="color:red;font-size:1em;">Maven 项目的目录结构</span></center>

```text
项目目录：
--pom.xml
--src目录
----main目录
------java目录
------resources目录
----test目录
------java目录
------resources目录
```





`pom.xml`的格式：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.cyw</groupId>
    <artifactId>demo01</artifactId>
    <version>0.0.1</version>
	<packing>jar</packing>

    <name>demo01</name>
    <description>This is demo01</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>


    <dependencies>
        <dependancy>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.0.0</version>
            <scope>test</scope>
        </dependancy>
    </dependencies>

    
    <dependencyManagement>
<!--         <dependancy>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.1.0</version>
        </dependancy>
 -->
    </dependencyManagement>
    

<!--     
	<build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
             </plugin>
        </plugins>
    </build>
 -->

</project>

```









# 三、IDEA 集成 Maven



见另一版Maven笔记





# 四、Maven 项目的创建



见另一版Maven笔记







# 五、Maven 仓库



见另一版Maven笔记







# 六、Maven 多模块管理



案例：

> - parent：父模块
> - dao：子模块1，jdbc操作。
> - service：子模块2，业务逻辑。
> - controller：子模块3，接收响应前端请求。



[视频](https://www.bilibili.com/video/BV1Fz4y167p5?p=11)



<iframe height="500px" src="//player.bilibili.com/player.html?aid=586020921&bvid=BV1Fz4y167p5&cid=281053479&page=11" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>





（1）创建父工程（普通的maven项目，不选模板）

![image-20220829101927788](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220829101927788.png)

<center><span style="color:red;font-size:1em;">创建父工程</span></center>



（2）创建子工程（普通的maven项目，选择继承父工程的 pom.xml ）

以 `dao`模块为例：

![image-20220829102340500](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220829102340500.png)

选中模板：`org.apache.maven.archetypes:maven-archetype-quickstart`

若是web项目，可选中`maven-archetype-webapp`结尾的模板

![image-20220829102702898](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220829102702898.png)







# 七、Maven 打包



<iframe height="500px" src="//player.bilibili.com/player.html?aid=586020921&bvid=BV1Fz4y167p5&cid=281053441&page=12" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>





# 八、Maven 依赖

见另一版Maven笔记