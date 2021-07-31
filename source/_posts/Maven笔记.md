---
title: Maven 笔记
tag: Maven
categories:
  - [编程工具,项目管理,Maven] 
  - [后端,Java,Java Web]
---



# Maven 笔记



[toc]





视频：[maven入门教程](https://www.bilibili.com/video/BV1x5411P7Hh?p=2)



## 0、Maven的作用：

-   管理依赖，帮助清理、编译、测试、打包、部署项目。
-   可以批量编译`.java程序`（`javac命令`只能1次编译1个文件）







## 1、Maven的目录结构



![目录结构](https://z3.ax1x.com/2021/07/26/WWcQUO.png)







![maven的工作方式](https://z3.ax1x.com/2021/07/26/WfFYqJ.png)



---









## 2、安装、配置 Maven



0.  前提：确保已经配置好JDK的环境。

1.  下载链接：[maven官网下载](http://maven.apache.org/download.cgi)

2.  新建环境变量`Maven_Home`，并将该环境变量指向安装目录（bin的上一级）

3.  将`%Maven_Home%\bin`加入环境变量的`Path`中。

4.  设置maven的 `Mirror 镜像`：

    ```xml
    <!--
    	maven的安装目录" /conf/settings.xml  "文件中，
    
    	找到mirrors标签： 
    -->
    
    
    、		<mirror>
        			<id>nexus-aliyun</id>
        			<mirrorOf>central</mirrorOf>
        			<name>Nexus aliyun</name>
        			<url>http://maven.aliyun.com/nexus/content/groups/public</url>
    		</mirror>
    ```

5.  设置项目的JDK版本，`pom.xml `文件的 `project标签`内添加以下内容：【此方式只能配置1个项目】

    ```xml
    
    		<properties>
    	  		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    	 	 	<maven.compiler.source>12</maven.compiler.source>
    	 	 	<maven.compiler.target>12</maven.compiler.target>
    		</properties>
    ```







---





## 3、编译、运行 Maven项目

-   cmd进入Maven项目目录（pom.xml所在的那一层），输入：mvn compile
-   进入编译生成的 target/classes/ 目录
-   cmd 输入 java  包名.类文件名
-   cmd中自动运行java程序





## 4、POM.xml文件



`pom`：project object model项目对象模型，是maven的灵魂。maven将项目当作1个模型处理。maven通过pom.xml文件来实现项目的构建和依赖的管理。



pom.xml实例：

```xml
<?xml version="1.0" ?>

// 根标签
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    
    // 版本说明
	<modelVersion>4.0.0</modelVersion>

   // 坐标：确定资源，是资源的唯一标识（由3个部分组成，简称 gav）
	<groupId>com.cyw</groupId>  		// 公司的标识，常写为公司域名的倒写
	<artifactId>Hello</artifactId>		// 项目名称,如果groupid中已有项目名，则此处填写子项目名
	<version>0.0.1-SNAPSHOT</version>	// 项目版本，由3部分组成：主版本.次版本.小版本号【SNAPSHOT表示：不稳定版】
    <packaging>jar</packaging>			// 项目的打包类型（jar【默认】,war,ear）
    
    // 项目名
	<name>Hello</name>
	  
    // 依赖环境
	<dependencies>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.0</version>
			<scope>test</scope>
		</dependency>
	</dependencies>

	// 编译设置
	<properties>
	  	<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
	 	 <maven.compiler.source>12</maven.compiler.source>
	 	 <maven.compiler.target>12</maven.compiler.target>
	</properties>
</project>
```



1、每个项目都需要有自己的 gav【在pom.xml文件中的3个坐标标签】

>   gav的示例：
>
>   -   `<groupId>com.cyw</groupId>`：公司的标识，常写为公司域名的倒写。
>   -   `<artifactId>Hello</artifactId>	`：项目名称,如果groupid中已有项目名，则此处填写子项目名。
>   -   `<version>0.0.1-SNAPSHOT</version>`：项目版本，由3部分组成：主版本.次版本.小版本号【SNAPSHOT表示：不稳定版】

2、项目所需的Jar包也需要在`pom.xml`中有gav作为唯一标识。

```xml


<dependencies>
    // 依赖jar包的 gav,【gav可以从 mvnrepository仓库 中获取】
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.0</version>
			<scope>test</scope>
		</dependency>
    
</dependencies>
```

3、gav可以从 mvnrepository仓库中获取：[mvnrepository官网](https://mvnrepository.com/)

>   搜索maven的gav详细的步骤：
>
>   -   搜索JAR包
>   -   点击所需的版本号
>   -   复制 gav信息





---





## 5、依赖

依赖：所需的 jar包。由dependence 和 gav 标签组合。

```xml

<dependencies>
    // 依赖jar包的 gav,【gav可以从 mvnrepository仓库 中获取】
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.0</version>
			<scope>test</scope>
		</dependency>
    
    // 第2个依赖包
    	<dependency>
    		<groupId>mysql</groupId>
    		<artifactId>mysql-connector-java</artifactId>
    		<version>8.0.26</version>
		</dependency>
    
</dependencies>
```







## 6、仓库

仓库是存东西的，Maven的仓库中存放的是

-   Maven自己的Jar包
-   第三方的Jar包
-   自己编写并打包的Jar包



---



仓库的分类：

-   本地仓库：自己电脑上的某个目录，默认路径：`系统账户目录/.m2/repository`



1、修改本地仓库的配置文件：

>   本地仓库没有所需的Jar包时，才从网络上下载。
>
>   -   创建`1个文件夹`作为本地仓库的根目录（不能为中文）
>   -   修改Maven工具的`settings文件`：`<localRepository>修改后的路径</localRepository>`





## 7、Maven的命令：

maven的命令是通过插件来实现的。<font style="color:red;">插件</font>也就是一些<font style="color:red;">Jar包</font>。



**Maven的生命周期命令**：【清理、编译、测试、打包、安装、部署】



1.  `mvn clean `：清除上次生成编译生成的 target 目录
2.  `mvn complie ` ：编译，将`src/main/java/`下的`java文件`编译为`class文件`，放入到`/target/classes/`目录下(即：复制到` classpath` 中)
3.  `mvn test-complie `：（单元测试）编译`/src/test/java`下的java文件

5.  `mvn package`：打包，打包成`jar 或 war`，生成的文件名（`artifactId-version-packaging`）
6.  `mvn install`：安装，将打包后的文件添加到Maven仓库中。
7.  `mvn deploy`：部署



---

单元测试：测的是方法。



步骤：

1.  在项目的`pom.xml`中添加依赖标签
2.  `/src/test/java`中编写测试类：类名以Test开头，方法public void，包名与原来的类相同。





---



## 8、IDEA 集成 Maven

idea中已经有了自带的maven，但是修改麻烦，所以需要我们自己集成1个maven。



集成maven的步骤：

1.  file => settings => build => buildTools => maven 
2.  设置maven的安装目录，settings配置文件，本地仓库路径

![idea中集成maven](https://z3.ax1x.com/2021/07/26/Wf6WWQ.png)



![idea集成maven-2](https://z3.ax1x.com/2021/07/26/Wfcnmt.png)



![idea集成maven-3](https://z3.ax1x.com/2021/07/26/WfcR76.png)
