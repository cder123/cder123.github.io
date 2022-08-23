---
title: SSM-综合项目-传智健康
tag: Java
categories:
  - [后端,Java,SSM]
---





<h1>SSM-综合项目-传智健康</h1>

[toc]



# 零、资料



> 视频：
>
> - [传智健康-SSM-黑马](https://www.bilibili.com/video/BV1Bo4y117zV?p=7)
>
>  
>
> 参考：
>
> - [仓库-1](https://gitee.com/itxinfei/health_parent)
> - [仓库-2](https://gitee.com/delete_lin/heima8406_health)
> - https://pan.baidu.com/s/1LxIxcHDO7SYB96SE-GZfuQ，提取码：dor4 （第三阶段）
> - [项目-SQL-gitee](https://gitee.com/sugeladi06/health_parent/blob/master/health.sql#)





# 一、环境搭建



## 1、项目结构



![image-20220409120558245](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220409120558245.png)





![image-20220409124002526](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220409124002526.png)

<center style="color:red">项目结构</center>

## 2、创建项目的父工程（health）



<font style="color:red;font-size:1.1em;">父工程的作用</font>：设定全局的jar包版本。



项目的父工程，是一个普通的Maven项目，不需要选择骨架，只需要在父工程的`pom.xml`文件中，设置打包方式、版本号、编译插件即可。



<iframe style="height:500px;" src="//player.bilibili.com/player.html?aid=375763798&bvid=BV1Bo4y117zV&cid=344363143&page=8" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>



父工程的`pom.xml`文件，添加以下配置：

```xml
    <packaging>pom</packaging>

    <!-- 集中定义依赖版本号 -->
    <properties>
        <junit.version>4.12</junit.version>
        <spring.version>5.0.5.RELEASE</spring.version>
        <pagehelper.version>4.1.4</pagehelper.version>
        <servlet-api.version>2.5</servlet-api.version>
        <dubbo.version>2.6.0</dubbo.version>
        <zookeeper.version>3.4.7</zookeeper.version>
        <zkclient.version>0.1</zkclient.version>
        <mybatis.version>3.4.5</mybatis.version>
        <mybatis.spring.version>1.3.1</mybatis.spring.version>
        <mybatis.paginator.version>1.2.15</mybatis.paginator.version>
        <mysql.version>5.1.32</mysql.version>
        <druid.version>1.0.9</druid.version>
        <commons-fileupload.version>1.3.1</commons-fileupload.version>
        <spring.security.version>5.0.5.RELEASE</spring.security.version>
        <poi.version>3.14</poi.version>
        <jedis.version>2.9.0</jedis.version>
        <quartz.version>2.2.1</quartz.version>
    </properties>
    <!-- 依赖管理标签  必须加 -->
    <dependencyManagement>
        <dependencies>
            <!-- Spring -->
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-context</artifactId>
                <version>${spring.version}</version>
            </dependency>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-beans</artifactId>
                <version>${spring.version}</version>
            </dependency>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-web</artifactId>
                <version>${spring.version}</version>
            </dependency>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-webmvc</artifactId>
                <version>${spring.version}</version>
            </dependency>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-jdbc</artifactId>
                <version>${spring.version}</version>
            </dependency>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-aspects</artifactId>
                <version>${spring.version}</version>
            </dependency>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-jms</artifactId>
                <version>${spring.version}</version>
            </dependency>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-context-support</artifactId>
                <version>${spring.version}</version>
            </dependency>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-test</artifactId>
                <version>${spring.version}</version>
            </dependency>
            <!-- dubbo相关 -->
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>dubbo</artifactId>
                <version>${dubbo.version}</version>
            </dependency>
            <dependency>
                <groupId>org.apache.zookeeper</groupId>
                <artifactId>zookeeper</artifactId>
                <version>${zookeeper.version}</version>
            </dependency>
            <dependency>
                <groupId>com.github.sgroschupf</groupId>
                <artifactId>zkclient</artifactId>
                <version>${zkclient.version}</version>
            </dependency>
            <dependency>
                <groupId>junit</groupId>
                <artifactId>junit</artifactId>
                <version>4.12</version>
            </dependency>
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>fastjson</artifactId>
                <version>1.2.47</version>
            </dependency>
            <dependency>
                <groupId>javassist</groupId>
                <artifactId>javassist</artifactId>
                <version>3.12.1.GA</version>
            </dependency>
            <dependency>
                <groupId>commons-codec</groupId>
                <artifactId>commons-codec</artifactId>
                <version>1.10</version>
            </dependency>
            <dependency>
                <groupId>com.github.pagehelper</groupId>
                <artifactId>pagehelper</artifactId>
                <version>${pagehelper.version}</version>
            </dependency>
            <!-- Mybatis -->
            <dependency>
                <groupId>org.mybatis</groupId>
                <artifactId>mybatis</artifactId>
                <version>${mybatis.version}</version>
            </dependency>
            <dependency>
                <groupId>org.mybatis</groupId>
                <artifactId>mybatis-spring</artifactId>
                <version>${mybatis.spring.version}</version>
            </dependency>
            <dependency>
                <groupId>com.github.miemiedev</groupId>
                <artifactId>mybatis-paginator</artifactId>
                <version>${mybatis.paginator.version}</version>
            </dependency>
            <!-- MySql -->
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>${mysql.version}</version>
            </dependency>
            <!-- 连接池 -->
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>druid</artifactId>
                <version>${druid.version}</version>
            </dependency>
            <!-- 文件上传组件 -->
            <dependency>
                <groupId>commons-fileupload</groupId>
                <artifactId>commons-fileupload</artifactId>
                <version>${commons-fileupload.version}</version>
            </dependency>
            <dependency>
                <groupId>org.quartz-scheduler</groupId>
                <artifactId>quartz</artifactId>
                <version>${quartz.version}</version>
            </dependency>
            <dependency>
                <groupId>org.quartz-scheduler</groupId>
                <artifactId>quartz-jobs</artifactId>
                <version>${quartz.version}</version>
            </dependency>
            <dependency>
                <groupId>com.sun.jersey</groupId>
                <artifactId>jersey-client</artifactId>
                <version>1.18.1</version>
            </dependency>
            <dependency>
                <groupId>com.qiniu</groupId>
                <artifactId>qiniu-java-sdk</artifactId>
                <version>7.2.0</version>
            </dependency>
            <!--POI报表-->
            <dependency>
                <groupId>org.apache.poi</groupId>
                <artifactId>poi</artifactId>
                <version>${poi.version}</version>
            </dependency>
            <dependency>
                <groupId>org.apache.poi</groupId>
                <artifactId>poi-ooxml</artifactId>
                <version>${poi.version}</version>
            </dependency>
            <dependency>
                <groupId>redis.clients</groupId>
                <artifactId>jedis</artifactId>
                <version>${jedis.version}</version>
            </dependency>
            <!-- 安全框架 -->
            <dependency>
                <groupId>org.springframework.security</groupId>
                <artifactId>spring-security-web</artifactId>
                <version>${spring.security.version}</version>
            </dependency>
            <dependency>
                <groupId>org.springframework.security</groupId>
                <artifactId>spring-security-config</artifactId>
                <version>${spring.security.version}</version>
            </dependency>
            <dependency>
                <groupId>org.springframework.security</groupId>
                <artifactId>spring-security-taglibs</artifactId>
                <version>${spring.security.version}</version>
            </dependency>
            <dependency>
                <groupId>com.github.penggle</groupId>
                <artifactId>kaptcha</artifactId>
                <version>2.3.2</version>
                <exclusions>
                    <exclusion>
                        <groupId>javax.servlet</groupId>
                        <artifactId>javax.servlet-api</artifactId>
                    </exclusion>
                </exclusions>
            </dependency>
            <dependency>
                <groupId>dom4j</groupId>
                <artifactId>dom4j</artifactId>
                <version>1.6.1</version>
            </dependency>
            <dependency>
                <groupId>xml-apis</groupId>
                <artifactId>xml-apis</artifactId>
                <version>1.4.01</version>
            </dependency>
        </dependencies>
    </dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>${servlet-api.version}</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <!-- java编译插件 -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.2</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
        </plugins>
    </build>
```





## 3、创建子模块 health-common



作用：配置maven依赖（给项目中的其他模块提供maven配置），存放通用类。



在父工程名上右键，新建模块，该模块为普通的maven项目（不需要选骨架，因为是打成 jar 包）。



`health-common`的maven配置文件：

```xml
	<packaging>jar</packaging>

    <dependencies>
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper</artifactId>
        </dependency>
        <!-- Mybatis -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
        </dependency>
        <dependency>
            <groupId>com.github.miemiedev</groupId>
            <artifactId>mybatis-paginator</artifactId>
        </dependency>
        <!-- MySql -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <!-- 连接池 -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
        </dependency>
        <dependency>
            <groupId>commons-fileupload</groupId>
            <artifactId>commons-fileupload</artifactId>
        </dependency>
        <!-- Spring -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aspects</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jms</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context-support</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
        </dependency>
        <!-- dubbo相关 -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>dubbo</artifactId>
        </dependency>
        <dependency>
            <groupId>org.apache.zookeeper</groupId>
            <artifactId>zookeeper</artifactId>
        </dependency>
        <dependency>
            <groupId>com.github.sgroschupf</groupId>
            <artifactId>zkclient</artifactId>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
        </dependency>
        <dependency>
            <groupId>javassist</groupId>
            <artifactId>javassist</artifactId>
        </dependency>
        <dependency>
            <groupId>commons-codec</groupId>
            <artifactId>commons-codec</artifactId>
        </dependency>
        <dependency>
            <groupId>org.apache.poi</groupId>
            <artifactId>poi</artifactId>
        </dependency>
        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
        </dependency>
        <dependency>
            <groupId>com.qiniu</groupId>
            <artifactId>qiniu-java-sdk</artifactId>
        </dependency>
        <dependency>
            <groupId>com.sun.jersey</groupId>
            <artifactId>jersey-client</artifactId>
        </dependency>
        <dependency>
            <groupId>org.apache.poi</groupId>
            <artifactId>poi-ooxml</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-config</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-taglibs</artifactId>
        </dependency>
        <dependency>
            <groupId>com.aliyun</groupId>
            <artifactId>aliyun-java-sdk-core</artifactId>
            <version>3.3.1</version>
        </dependency>
        <dependency>
            <groupId>com.aliyun</groupId>
            <artifactId>aliyun-java-sdk-dysmsapi</artifactId>
            <version>1.0.0</version>
        </dependency>
    </dependencies>
```







## 4、创建子模块 health-interface



子模块 `health-interface `, 存放 `Service `层的接口，继承自`health-common` 模块（得到maven配置）



 `health-interface `的maven配置中，添加如下配置：

```xml
	<packaging>jar</packaging>

    <dependencies>
        <dependency>
            <groupId>com.cyw</groupId>
            <artifactId>health-common</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
    </dependencies>
```









## 5、创建子模块 health-service-provider



打成`war包`，所以骨架选择`webapp`。

子模块 `health-service-provider`是`Service`层和`Mapper`层的实现。

该子模块使用`Dubble`来提供服务。



<font style="color:red;">注意</font>：需要右键“src”目录，新建目录，来补全maven的目录结构



### 5.1、添加 Maven 配置

 `health-service-provider `的maven配置中，添加如下配置：

```xml
 <packaging>war</packaging>

    <name>health-service-provider Maven Webapp</name>
    <!-- FIXME change it to the project's website -->
    <url>http://www.example.com</url>

    <dependencies>
        <dependency>
            <groupId>com.cyw</groupId>
            <artifactId>health-interface</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.tomcat.maven</groupId>
                <artifactId>tomcat7-maven-plugin</artifactId>
                <version>2.2</version>
                <configuration>
                    <!-- 指定端口 -->
                    <port>81</port>
                    <!-- 请求路径 -->
                    <path>/</path>
                </configuration>
            </plugin>
        </plugins>
    </build>

```





### 5.2、添加 log4j 配置



在子模块` health-service-provider`的`src/main/resource/`目录下创建 log4j 配置文件：

```properties
### direct log messages to stdout ###
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.err
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n

### direct messages to file mylog.log ###
log4j.appender.file=org.apache.log4j.FileAppender
log4j.appender.file.File=./mylog.log
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n

### set log levels - for more verbose logging change 'info' to 'debug' ###

log4j.rootLogger=stdout
```





### 5.3 、添加 Mybatis 的核心配置文件



在子模块` health-service-provider`的`src/main/resource/`目录下创建 Mybatis 的核心配置文件（配置Mybatis 的分页插件）



`mybatis-config.xml`:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <plugins>
        <!-- com.github.pagehelper 为 PageHelper 类所在包名 -->
        <plugin interceptor="com.github.pagehelper.PageHelper">
            <!-- 设置数据库类型 Oracle,Mysql,MariaDB,SQLite,Hsqldb,PostgreSQL 六种数据库-->
            <property name="dialect" value="mysql"/>
        </plugin>
    </plugins>
</configuration>
```



### 5.4、添加 spring-mapper 配置文件



```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
							http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
							http://www.springframework.org/schema/context
							http://www.springframework.org/schema/context/spring-context.xsd
							http://www.springframework.org/schema/aop
							http://www.springframework.org/schema/aop/spring-aop.xsd
							http://www.springframework.org/schema/tx
							http://www.springframework.org/schema/tx/spring-tx.xsd
							http://www.springframework.org/schema/util
							http://www.springframework.org/schema/util/spring-util.xsd">

    <!--数据源-->
    <bean id="dataSource"
          class="com.alibaba.druid.pool.DruidDataSource" destroy-method="close">
        <property name="username" value="root" />
        <property name="password" value="root" />
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver" />
        <property name="url" value="jdbc:mysql://localhost:3306/health?characterEncoding=UTF-8&amp;ServerTimezone=Shanghai" />
    </bean>
    <!--spring和mybatis整合的工厂bean-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource" />
        <property name="configLocation" value="classpath:mybatis-config.xml" />
    </bean>
    <!--批量扫描接口生成代理对象-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!--指定接口所在的包-->
        <property name="basePackage" value="com.cyw.mapper" />
    </bean>
</beans>
```



### 5.5、添加 spring-tx 配置文件



```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                        http://www.springframework.org/schema/beans/spring-beans.xsd
                        http://www.springframework.org/schema/mvc
                        http://www.springframework.org/schema/mvc/spring-mvc.xsd
                        http://www.springframework.org/schema/tx
                        http://www.springframework.org/schema/tx/spring-tx.xsd
                        http://www.springframework.org/schema/context
                        http://www.springframework.org/schema/context/spring-context.xsd">
    <!-- 事务管理器  -->
    <bean id="transactionManager"
          class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>
    <!--
        开启事务控制的注解支持
        注意：此处必须加入proxy-target-class="true"，
              需要进行事务控制，会由Spring框架产生代理对象，
              Dubbo需要将Service发布为服务，要求必须使用cglib创建代理对象。
    -->
    <tx:annotation-driven transaction-manager="transactionManager"
                          proxy-target-class="true"/>
</beans>
```





### 5.6、添加 spring-service 配置文件



```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                            http://www.springframework.org/schema/beans/spring-beans.xsd
                            http://www.springframework.org/schema/mvc
                            http://www.springframework.org/schema/mvc/spring-mvc.xsd
                            http://code.alibabatech.com/schema/dubbo
                            http://code.alibabatech.com/schema/dubbo/dubbo.xsd
                            http://www.springframework.org/schema/context
                            http://www.springframework.org/schema/context/spring-context.xsd">
    <!-- 指定应用名称 -->
    <dubbo:application name="health-service-provider"/>
    <!--指定暴露服务的端口，如果不指定默认为20880-->
    <dubbo:protocol name="dubbo" port="20887"/>
    <!--指定服务注册中心地址-->
    <dubbo:registry address="zookeeper://127.0.0.1:2181"/>
    <!--批量扫描，发布服务-->
    <dubbo:annotation package="com.cyw.service"/>
</beans>
```





### 5.7、添加 spring-redis 配置文件



```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                         http://www.springframework.org/schema/beans/spring-beans.xsd
        				http://www.springframework.org/schema/mvc
                         http://www.springframework.org/schema/mvc/spring-mvc.xsd
        				http://code.alibabatech.com/schema/dubbo
                         http://code.alibabatech.com/schema/dubbo/dubbo.xsd
        				http://www.springframework.org/schema/context
                         http://www.springframework.org/schema/context/spring-context.xsd">

    <!--Jedis连接池的相关配置-->
    <bean id="jedisPoolConfig" class="redis.clients.jedis.JedisPoolConfig">
        <property name="maxTotal">
            <value>200</value>
        </property>
        <property name="maxIdle">
            <value>50</value>
        </property>
        <property name="testOnBorrow" value="true"/>
        <property name="testOnReturn" value="true"/>
    </bean>
    <bean id="jedisPool" class="redis.clients.jedis.JedisPool">
        <constructor-arg name="poolConfig" ref="jedisPoolConfig" />
        <constructor-arg name="host" value="127.0.0.1" />
        <constructor-arg name="port" value="6379" type="int" />
        <constructor-arg name="timeout" value="30000" type="int" />
    </bean>
</beans>
```



### 5.8、创建 包



创建包（`src/main/`）：

> （1）`com.cyw.mapper`
>
> （2）`com.cyw.service.impl`

创建目录（`resource/com/cyw/mapper/`）





### 5.9、配置 web.xml



```xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>Archetype Created Web Application</display-name>

  <!-- 加载spring容器 -->
  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath*:spring-*.xml</param-value>
  </context-param>
  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>  
  
</web-app>

```









## 6、创建子模块 health-backend



打成`war包`，所以骨架选择`webapp`。



子模块 health-backend 是该综合项目的后台系统（`Controller层`）。



作用：存放 `Controller`和`页面`



<font style="color:red;">注意</font>：需要右键“src”目录，新建目录，来补全maven的目录结构





### 6.1、添加 Maven 配置

`health-backend`模块的`pom.xml`配置：

```xml
 <dependencies>
       <dependency>
           <groupId>com.cyw</groupId>
           <artifactId>health-interface</artifactId>
           <version>1.0-SNAPSHOT</version>
       </dependency>
   </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.tomcat.maven</groupId>
                <artifactId>tomcat7-maven-plugin</artifactId>
                <version>2.2</version>
                <configuration>
                    <!-- 指定端口 -->
                    <port>82</port>
                    <!-- 请求路径 -->
                    <path>/</path>
                </configuration>
            </plugin>
        </plugins>
    </build>
```



### 6.2、创建 log4j 配置文件



直接从刚才创建的子模块 `health-service-provider`里复制（复制到`resource`目录下）。





### 6.3、创建 springmvc.xml



```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
						http://www.springframework.org/schema/beans/spring-beans.xsd
						http://www.springframework.org/schema/mvc
						http://www.springframework.org/schema/mvc/spring-mvc.xsd
						http://code.alibabatech.com/schema/dubbo
						http://code.alibabatech.com/schema/dubbo/dubbo.xsd
						http://www.springframework.org/schema/context
						http://www.springframework.org/schema/context/spring-context.xsd">
    <mvc:annotation-driven>
        <mvc:message-converters register-defaults="true">
            <bean class="com.alibaba.fastjson.support.spring.FastJsonHttpMessageConverter">
                <property name="supportedMediaTypes" value="application/json"/>
                <property name="features">
                    <list>
                        <value>WriteMapNullValue</value>
                        <value>WriteDateUseDateFormat</value>
                    </list>
                </property>
            </bean>
        </mvc:message-converters>
    </mvc:annotation-driven>
    <!-- 指定应用名称 -->
    <dubbo:application name="health-backend" />
    <!--指定服务注册中心地址-->
    <dubbo:registry address="zookeeper://127.0.0.1:2181"/>
    <!--批量扫描-->
    <dubbo:annotation package="com.cyw" />
    <!--
        超时全局设置 10分钟
        check=false 不检查服务提供方，开发阶段建议设置为false
        check=true 启动时检查服务提供方，如果服务提供方没有启动则报错
    -->
    <dubbo:consumer timeout="600000" check="false"/>
    <!--文件上传组件-->
    <bean id="multipartResolver"
          class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <property name="maxUploadSize" value="104857600" />
        <property name="maxInMemorySize" value="4096" />
        <property name="defaultEncoding" value="UTF-8"/>
    </bean>

    <import resource="spring-redis.xml"></import>
    <import resource="spring-security.xml"></import>

</beans>
```









### 6.4、创建 spring-redis.xml（暂时不用）



```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                         http://www.springframework.org/schema/beans/spring-beans.xsd
        				http://www.springframework.org/schema/mvc
                         http://www.springframework.org/schema/mvc/spring-mvc.xsd
        				http://code.alibabatech.com/schema/dubbo
                         http://code.alibabatech.com/schema/dubbo/dubbo.xsd
        				http://www.springframework.org/schema/context
                         http://www.springframework.org/schema/context/spring-context.xsd">

    <!--Jedis连接池的相关配置-->
    <bean id="jedisPoolConfig" class="redis.clients.jedis.JedisPoolConfig">
        <property name="maxTotal">
            <value>200</value>
        </property>
        <property name="maxIdle">
            <value>50</value>
        </property>
        <property name="testOnBorrow" value="true"/>
        <property name="testOnReturn" value="true"/>
    </bean>
    <bean id="jedisPool" class="redis.clients.jedis.JedisPool">
        <constructor-arg name="poolConfig" ref="jedisPoolConfig" />
        <constructor-arg name="host" value="127.0.0.1" />
        <constructor-arg name="port" value="6379" type="int" />
        <constructor-arg name="timeout" value="30000" type="int" />
    </bean>
</beans>
```







### 6.5、创建 spring-security.xml（暂时不用）





```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:security="http://www.springframework.org/schema/security"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
						http://www.springframework.org/schema/beans/spring-beans.xsd
						http://www.springframework.org/schema/mvc
						http://www.springframework.org/schema/mvc/spring-mvc.xsd
						http://code.alibabatech.com/schema/dubbo
						http://code.alibabatech.com/schema/dubbo/dubbo.xsd
						http://www.springframework.org/schema/context
						http://www.springframework.org/schema/context/spring-context.xsd
                     http://www.springframework.org/schema/security
                     http://www.springframework.org/schema/security/spring-security.xsd">

    <!--配置哪些资源匿名可以访问（不登录也可以访问）-->
    <!--<security:http security="none" pattern="/pages/a.html"></security:http>
    <security:http security="none" pattern="/pages/b.html"></security:http>-->
    <!--<security:http security="none" pattern="/pages/**"></security:http>-->
    <security:http security="none" pattern="/login.html"></security:http>
    <security:http security="none" pattern="/css/**"></security:http>
    <security:http security="none" pattern="/img/**"></security:http>
    <security:http security="none" pattern="/js/**"></security:http>
    <security:http security="none" pattern="/plugins/**"></security:http>
    <!--
        auto-config:自动配置，如果设置为true，表示自动应用一些默认配置，比如框架会提供一个默认的登录页面
        use-expressions:是否使用spring security提供的表达式来描述权限
    -->
    <security:http auto-config="true" use-expressions="true">
        <security:headers>
            <!--设置在页面可以通过iframe访问受保护的页面，默认为不允许访问-->
            <security:frame-options policy="SAMEORIGIN"></security:frame-options>
        </security:headers>
        <!--配置拦截规则，/** 表示拦截所有请求-->
        <!--
            pattern:描述拦截规则
            asscess:指定所需的访问角色或者访问权限
        -->
        <!--只要认证通过就可以访问-->
        <security:intercept-url pattern="/pages/**"  access="isAuthenticated()" />

        <!--如果我们要使用自己指定的页面作为登录页面，必须配置登录表单.页面提交的登录表单请求是由框架负责处理-->
        <!--
            login-page:指定登录页面访问URL
        -->
        <security:form-login
                login-page="/login.html"
                username-parameter="username"
                password-parameter="password"
                login-processing-url="/login"
                default-target-url="/pages/main.html"
                authentication-failure-url="/login.html"></security:form-login>

        <!--
          csrf：对应CsrfFilter过滤器
          disabled：是否启用CsrfFilter过滤器，如果使用自定义登录页面需要关闭此项，否则登录操作会被禁用（403）
        -->
        <security:csrf disabled="true"></security:csrf>

        <!--
          logout：退出登录
          logout-url：退出登录操作对应的请求路径
          logout-success-url：退出登录后的跳转页面
        -->
        <security:logout logout-url="/logout"
                         logout-success-url="/login.html" invalidate-session="true"/>

    </security:http>

    <!--配置认证管理器-->
    <security:authentication-manager>
        <!--配置认证提供者-->
        <security:authentication-provider user-service-ref="springSecurityUserService">
            <!--
                    配置一个具体的用户，后期需要从数据库查询用户
            <security:user-service>
                <security:user name="admin" password="{noop}1234" authorities="ROLE_ADMIN"/>
            </security:user-service>
            -->
            <!--指定度密码进行加密的对象-->
            <security:password-encoder ref="passwordEncoder"></security:password-encoder>
        </security:authentication-provider>
    </security:authentication-manager>

    <!--配置密码加密对象-->
    <bean id="passwordEncoder"
          class="org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder" />

    <!--开启注解方式权限控制-->
    <security:global-method-security pre-post-annotations="enabled" />
</beans>
```





### 6.6 、配置 web.xml



```xml
<!DOCTYPE web-app PUBLIC
        "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
        "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
    <display-name>Archetype Created Web Application</display-name>
    
    
    <!--委派过滤器，用于整合其他框架-->
    <filter>
        <!--整合spring security时，此过滤器的名称固定springSecurityFilterChain-->
        <filter-name>springSecurityFilterChain</filter-name>
        <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
    </filter>
    
    <!-- 解决post乱码 -->
    <filter>
        <filter-name>CharacterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
        <init-param>
            <param-name>forceEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    
    <filter-mapping>
        <filter-name>springSecurityFilterChain</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    
    <filter-mapping>
        <filter-name>CharacterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!-- 指定加载的配置文件 ，通过参数contextConfigLocation加载 -->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    
</web-app>

```





## 7、创建数据库





### 7.1、创建“检查组—数据表”



```sql
-- create database health;
use health;

/*
	检查组
******************************************************
*/
DROP TABLE IF EXISTS `t_checkgroup`;

CREATE TABLE `t_checkgroup` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `code` varchar(32) DEFAULT NULL,
  `name` varchar(32) DEFAULT NULL,
  `helpCode` varchar(32) DEFAULT NULL,
  `sex` char(1) DEFAULT NULL,
  `remark` varchar(128) DEFAULT NULL,
  `attention` varchar(128) DEFAULT NULL,
  PRIMARY KEY (`id`)
);


insert  into `t_checkgroup`(`id`,`code`,`name`,`helpCode`,`sex`,`remark`,`attention`)
 values (5,'0001','一般检查','YBJC','0','一般检查','无'),
(6,'0002','视力色觉','SLSJ','0','视力色觉',NULL),
(7,'0003','血常规','XCG','0','血常规',NULL),
(8,'0004','尿常规','NCG','0','尿常规',NULL),
(9,'0005','肝功三项','GGSX','0','肝功三项',NULL),
(10,'0006','肾功三项','NGSX','0','肾功三项',NULL),
(11,'0007','血脂四项','XZSX','0','血脂四项',NULL),
(12,'0008','心肌酶三项','XJMSX','0','心肌酶三项',NULL),
(13,'0009','甲功三项','JGSX','0','甲功三项',NULL),
(14,'0010','子宫附件彩超','ZGFJCC','2','子宫附件彩超',NULL),
(15,'0011','胆红素三项','DHSSX','0','胆红素三项',NULL),
(18,'0012','脂肪含量','ZFHL','0','对于久坐不动的上班族,提供自身油脂含量的检查','体检前5小时不能进食'),
(19,'0013','前列腺检查','QLX','1','对于男性的重要功能进行检查',NULL),
(20,'0014','健康成长','JKCZ','0','针对未成年人群提供的疾病早发现,早治疗计划',NULL);

```



### 7.2、创建检查项-数据表



```sql

DROP TABLE IF EXISTS `t_checkitem`;

CREATE TABLE `t_checkitem` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `code` varchar(16) DEFAULT NULL,
  `name` varchar(32) DEFAULT NULL,
  `sex` char(1) DEFAULT NULL,
  `age` varchar(32) DEFAULT NULL,
  `price` float DEFAULT NULL,
  `type` char(1) DEFAULT NULL COMMENT '查检项类型,分为检查和检验两种',
  `attention` varchar(128) DEFAULT NULL,
  `remark` varchar(128) DEFAULT NULL,
  PRIMARY KEY (`id`)
);

insert  into `t_checkitem`(`id`,`code`,`name`,`sex`,`age`,`price`,`type`,`attention`,`remark`) 
values (28,'0001','身高','0','0-100',5,'1','无','身高'),
(29,'0002','体重','0','0-100',5,'1','无','体重'),
(30,'0003','体重指数','0','0-100',5,'1','无','体重指数'),
(31,'0004','收缩压','0','0-100',5,'1','无','收缩压'),
(32,'0005','舒张压','0','0-100',5,'1','无','舒张压'),
(33,'0006','裸视力（右）','0','0-100',5,'1','无','裸视力（右）'),
(34,'0007','裸视力（左）','0','0-100',5,'1','无','裸视力（左）'),
(35,'0008','矫正视力（右）','0','0-100',5,'1','无','矫正视力（右）'),
(36,'0009','矫正视力（左）','0','0-100',5,'1','无','矫正视力（左）'),
(37,'0010','色觉','0','0-100',5,'1','无','色觉'),
(38,'0011','白细胞计数','0','0-100',10,'2','无','白细胞计数'),
(39,'0012','红细胞计数','0','0-100',10,'2',NULL,'红细胞计数'),
(40,'0013','血红蛋白','0','0-100',10,'2',NULL,'血红蛋白'),
(41,'0014','红细胞压积','0','0-100',10,'2',NULL,'红细胞压积'),
(42,'0015','平均红细胞体积','0','0-100',10,'2',NULL,'平均红细胞体积'),
(43,'0016','平均红细胞血红蛋白含量','0','0-100',10,'2',NULL,'平均红细胞血红蛋白含量'),
(44,'0017','平均红细胞血红蛋白浓度','0','0-100',10,'2',NULL,'平均红细胞血红蛋白浓度'),
(45,'0018','红细胞分布宽度-变异系数','0','0-100',10,'2',NULL,'红细胞分布宽度-变异系数'),
(46,'0019','血小板计数','0','0-100',10,'2',NULL,'血小板计数'),
(47,'0020','平均血小板体积','0','0-100',10,'2',NULL,'平均血小板体积'),
(48,'0021','血小板分布宽度','0','0-100',10,'2',NULL,'血小板分布宽度'),
(49,'0022','淋巴细胞百分比','0','0-100',10,'2',NULL,'淋巴细胞百分比'),
(50,'0023','中间细胞百分比','0','0-100',10,'2',NULL,'中间细胞百分比'),
(51,'0024','中性粒细胞百分比','0','0-100',10,'2',NULL,'中性粒细胞百分比'),
(52,'0025','淋巴细胞绝对值','0','0-100',10,'2',NULL,'淋巴细胞绝对值'),
(53,'0026','中间细胞绝对值','0','0-100',10,'2',NULL,'中间细胞绝对值'),
(54,'0027','中性粒细胞绝对值','0','0-100',10,'2',NULL,'中性粒细胞绝对值'),
(55,'0028','红细胞分布宽度-标准差','0','0-100',10,'2',NULL,'红细胞分布宽度-标准差'),
(56,'0029','血小板压积','0','0-100',10,'2',NULL,'血小板压积'),
(57,'0030','尿比重','0','0-100',10,'2',NULL,'尿比重'),
(58,'0031','尿酸碱度','0','0-100',10,'2',NULL,'尿酸碱度'),
(59,'0032','尿白细胞','0','0-100',10,'2',NULL,'尿白细胞'),
(60,'0033','尿亚硝酸盐','0','0-100',10,'2',NULL,'尿亚硝酸盐'),
(61,'0034','尿蛋白质','0','0-100',10,'2',NULL,'尿蛋白质'),
(62,'0035','尿糖','0','0-100',10,'2',NULL,'尿糖'),
(63,'0036','尿酮体','0','0-100',10,'2',NULL,'尿酮体'),
(64,'0037','尿胆原','0','0-100',10,'2',NULL,'尿胆原'),
(65,'0038','尿胆红素','0','0-100',10,'2',NULL,'尿胆红素'),
(66,'0039','尿隐血','0','0-100',10,'2',NULL,'尿隐血'),
(67,'0040','尿镜检红细胞','0','0-100',10,'2',NULL,'尿镜检红细胞'),
(68,'0041','尿镜检白细胞','0','0-100',10,'2',NULL,'尿镜检白细胞'),
(69,'0042','上皮细胞','0','0-100',10,'2',NULL,'上皮细胞'),
(70,'0043','无机盐类','0','0-100',10,'2',NULL,'无机盐类'),
(71,'0044','尿镜检蛋白定性','0','0-100',10,'2',NULL,'尿镜检蛋白定性'),
(72,'0045','丙氨酸氨基转移酶','0','0-100',10,'2',NULL,'丙氨酸氨基转移酶'),
(73,'0046','天门冬氨酸氨基转移酶','0','0-100',10,'2',NULL,'天门冬氨酸氨基转移酶'),
(74,'0047','Y-谷氨酰转移酶','0','0-100',10,'2',NULL,'Y-谷氨酰转移酶'),
(75,'0048','尿素','0','0-100',10,'2',NULL,'尿素'),
(76,'0049','肌酐','0','0-100',10,'2',NULL,'肌酐'),
(77,'0050','尿酸','0','0-100',10,'2',NULL,'尿酸'),
(78,'0051','总胆固醇','0','0-100',10,'2',NULL,'总胆固醇'),
(79,'0052','甘油三酯','0','0-100',10,'2',NULL,'甘油三酯'),
(80,'0053','高密度脂蛋白胆固醇','0','0-100',10,'2',NULL,'高密度脂蛋白胆固醇');

```





### 7.3、创建检查组-检查项的关联表



```sql

DROP TABLE IF EXISTS `t_checkgroup_checkitem`;

CREATE TABLE `t_checkgroup_checkitem` (
  `checkgroup_id` int(11) NOT NULL DEFAULT '0',
  `checkitem_id` int(11) NOT NULL DEFAULT '0',
  PRIMARY KEY (`checkgroup_id`,`checkitem_id`),
  KEY `item_id` (`checkitem_id`),
  CONSTRAINT `group_id` FOREIGN KEY (`checkgroup_id`) REFERENCES `t_checkgroup` (`id`),
  CONSTRAINT `item_id` FOREIGN KEY (`checkitem_id`) REFERENCES `t_checkitem` (`id`)
);

insert  into `t_checkgroup_checkitem`(`checkgroup_id`,`checkitem_id`) 
values (5,28),(18,28),(5,29),(18,29),(5,30),(5,31),(5,32),(6,33),(20,33),(6,34),(20,34),(6,35),(20,35),(6,36),(6,37),(7,38),(20,38),(7,39),(20,39),(7,40),(18,40),(7,41),(7,42),(7,43),(7,44),(20,44),(7,45),(20,45),(7,46),(7,47),(7,48),(7,49),(7,50),(7,51),(7,52),(7,53),(20,53),(7,54),(20,54),(7,55),(7,56),(18,56),(8,57),(19,57),(8,58),(19,58),(8,59),(8,60),(20,60),(8,61),(8,62),(19,62),(20,62),(8,63),(8,64),(19,64),(8,65),(19,65),(8,66),(8,67),(19,67),(8,68),(8,69),(8,70),(18,70),(8,71),(18,71),(20,71),(9,72),(9,73),(9,74),(10,75),(10,76),(10,77),(20,77),(11,78),(18,78),(20,78),(11,79),(11,80),(18,80);


```





## 8、安装ZooKeeper 



（1）[安装包下载地址](https://www.apache.org/dyn/closer.lua/zookeeper/zookeeper-3.6.3/apache-zookeeper-3.6.3-bin.tar.gz)

（2）安装：

```txt
1、解压apache-zookeeper-3.6.2-bin.tar.gz  至 D:\zookeeper\apache-zookeeper-3.6.2-bin。

2、进入conf目录，

3. 复制 zoo-sample.cfg，粘贴为zoo.cfg。

3、修改zoo.cfg中的 dataDir=D:\\zookeeper\\data

```

（3）启动：

```txt
1. 进入 bin 目录
2. 双击 zkServer.cmd
```







## 9、测试环境是否搭建成功



（1）点击 `health`父工程的maven命令：`clean`（生产周期中的命令）

（2）点击 `health`父工程的maven命令：`install`（生产周期中的命令）

（3）展开 `health-backend`子模块的maven命令：`tomcat7/tomcat7:run`（插件中的命令），右键debug运行。

（4）浏览器地址栏输入：`http://localhost:8082/pages/main.html`







# 二、基本 CRUD 的实现

























