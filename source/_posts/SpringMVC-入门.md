---
title: SpringMVC-入门
tag: Java
categories:
  - [后端,Java,SSM]

---

# SpringMVC-入门

[TOC]



## 0、资料



视频：

>   -   [尚硅谷-SpringMVC](https://www.bilibili.com/video/BV1Ry4y1574R?p=2)



书：

>   -   SSM从零开始学



博客：

>   -   [尚硅谷版-笔记-博客](https://mowangblog.github.io/SpringMVC-Demo/)



---



## 1、SpringMVC 概述



### 1.1、什么是MVC



M：Model 模型，也就是广义上的JavaBean（Service、Dao、实体类）

V：View 视图，HTML、JSP等前端页面

C：Controller 控制器，也就是Sevlet



【流程】：

![image-20220131101356685](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220131101356685.png)







![SpringMVC核心架构](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220201124418631.png)



---



### 1.2、什么是Spring MVC

Spring MVC 是Spring 的一个子项目，是Spring 为表示层提供的一整套完备的解决方案。

注：三层架构分为表示层、业务逻辑层、数据访问层。表示层主要是 **前端页面 + 后端的Servlet** 。



### 1.3、Spring MVC的特点



>   -   **简洁、灵活**，提高开发效率
>   -   Spring 的一部分
>   -   基于原生的Servlet，通过 **DispacherServlet** 来处理请求和响应。
>   -   全面覆盖**表示层需**要解决的问题
>   -   组件化程度高，**即插即用**
>   -   **性能好**，适合大型、超大型互联网项目的要求。





### 1.4、Spring MVC版-Hello World



<font style="color:red;">案例提示：</font>

-   本案例中使用到了 thymeleaf 模板引擎，这是一个在服务端获取后端的ModelAndView 对象后，解析，生成结果，输出给浏览器呈现结果的工具。



<font style="color:red;">步骤：</font>

-   创建`Mavnen`项目
-   编写`pom.xml`配置文件来导入jar包
-   在`web.xml `中配置前端控制器`DispacherServlet`
-   创建**请求控制器** `HelloWorldController`类（一个被`@Controller`注解标识的POJO）
-   在Spring的配置文件中开启**组件扫描**
-   在创建`WEB-INF/templates/index.html`，并在Spring的配置文件中注册 视图解析器（案例使用了thymeleaf）
-   在**请求控制器** `HelloWorldController`中新建`String index()`方法，返回视图名
-   在**请求控制器** `HelloWorldController`中新建`String index()`方法上添加`@RequestMapping("/")`注解，表示浏览器 url 访问 `/`  时，执行请求控制器的`index()`方法
-   可以成功访问首页
-   在首页添加一个访问其他页面的超链接（`<a th:href=“@{ /target }”>目标页</a>`）





>   1.   IDEA中，创建`Maven项目`（选择maven下的webapp模板）



![image-20220201132716172](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220201132716172.png)

项目的目录结构：

![image-20220201132917059](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220201132917059.png)



>   2.   填写maven的配置文件：`pom.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.cyw</groupId>
  <artifactId>SSM-05</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>

  <name>SSM-05 Maven Webapp</name>
  <!-- FIXME change it to the project's website -->
  <url>http://www.example.com</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.3.9</version>
    </dependency>
    <dependency>
      <groupId>ch.qos.logback</groupId>
      <artifactId>logback-classic</artifactId>
      <version>1.2.10</version>
    </dependency>
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>4.0.1</version>
      <scope>provided</scope>
    </dependency>
    <dependency>
      <groupId>org.thymeleaf</groupId>
      <artifactId>thymeleaf-spring5</artifactId>
      <version>3.0.14.RELEASE</version>
    </dependency>
  </dependencies>
<!-- ========================================================= -->
  <build>
    <finalName>SSM-05</finalName>
    <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
      <plugins>
<!--导入以下插件 -->
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-surefire-plugin</artifactId>
          <configuration>
            <testFailureIgnore>true</testFailureIgnore>
          </configuration>
        </plugin>
        <plugin>
          <artifactId>maven-clean-plugin</artifactId>
          <version>3.1.0</version>
        </plugin>
        <!-- see http://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_war_packaging -->
        <plugin>
          <artifactId>maven-resources-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.8.0</version>
        </plugin>
        <plugin>
          <artifactId>maven-surefire-plugin</artifactId>
          <version>2.22.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-war-plugin</artifactId>
          <version>3.2.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-install-plugin</artifactId>
          <version>2.5.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-deploy-plugin</artifactId>
          <version>2.8.2</version>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>
</project>

```



>   3.   配置`web.xml`，注册SpringMVC的前端控制器` DispacherServlet`

<font style="color:red;">默认配置的方式【不推荐】：</font>

在此配置方式下，SpringMVC 的配置文件将默认放在`WEB-INF`目录下，

且该 SpringMVC 配置文件的默认名称为`servlet名-servlet.xml`。

例如：以下 SpringMVC  配置文件的名称为`springMVC-servlet.xml`

```xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>Archetype Created Web Application</display-name>
  
  <!--
 	在web.xml 中填写以下配置:
		其中url-pattern标签的 "/"表示url可以请求“/login”或html、css等静态资源
		但不包括：.jsp文件
		在之前的JavaWeb阶段的“/*”表示的是所有的请求（包括.jsp）
  -->  
  <servlet>
    <servlet-name>springMVC</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  </servlet>  
  <servlet-mapping>
    <servlet-name>springMVC</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
  
    
</web-app>
```



<font style="color:red;">扩展配置的方式【推荐】：</font>

视频：https://www.bilibili.com/video/BV1Ry4y1574R?p=9

```xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>Archetype Created Web Application</display-name>
  
    <!--  注册前端控制器：DispacherServlet-->
      <servlet>
        <servlet-name>springMVC</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <!--    注册SpringMVC配置文件的所在路径-->
        <init-param>
          <param-name>contextConfigLocation</param-name>
          <param-value>classpath:springMVC.xml</param-value>
        </init-param>
    <!--    容器启动时，立即加载DispacherServlet-->
		<load-on-startup>1</load-on-startup>
      </servlet>  
      <servlet-mapping>
        <servlet-name>springMVC</servlet-name>
        <url-pattern>/</url-pattern>
      </servlet-mapping>
  
</web-app>

```



>   4.   创建 **请求控制器** `HelloWorldController`（一个被`@Controller`注解标识的POJO）

前端控制器`DispatcherServlet`负责统一的处理，然后将不同的请求交给**请求控制器**来处理（请求处理器）。

**请求控制器**中的处理方法叫 **控制器方法**。

```java
package com.cyw.controller;

import org.springframework.stereotype.Controller;

@Controller
public class HelloWorldController {
    
}
```



>   5.   开启组件扫描（在Spring的配置文件中）



```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    
	<!--    引入context 的xml约束和命名空间，开启组件扫描-->
    <context:component-scan base-package="com.cyw.controller"/>    
    

</beans>
```



>   6.   在`WEB-INF/template`目录下创建 `index.html`，在Spring 配置文件中，注册 Thymeleaf **视图解析器**



创建index.html：（需要引入命名空间）

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>首页</title>
</head>
<body>
   
    <h1>首页</h1>

</body>
</html>
```



配置视图解析器（以 thymeleaf 的视图解析器为例）：

```xml
<!-- 配置Thymeleaf视图解析器 -->
    <bean id="viewResolver" class="org.thymeleaf.spring5.view.ThymeleafViewResolver">
        <property name="order" value="1"/>
        <property name="characterEncoding" value="UTF-8"/>
        <property name="templateEngine">
            <bean class="org.thymeleaf.spring5.SpringTemplateEngine">
                <property name="templateResolver">
                    <bean class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">

                        <!-- 视图前缀 -->
                        <property name="prefix" value="/WEB-INF/templates/"/>

                        <!-- 视图后缀 -->
                        <property name="suffix" value=".html"/>
                        <property name="templateMode" value="HTML5"/>
                        <property name="characterEncoding" value="UTF-8" />
                    </bean>
                </property>
            </bean>
        </property>
    </bean>
```



>   7.   在 请求控制器 上添加一个被`RequestMapping("url路径")`注解修饰的**控制方法**

```java
package com.cyw.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class HelloWorldController {

    @RequestMapping("/")
    public String index(){
        // 返回视图的名称【页面路径 = 前缀 + 视图名称 + 后缀】
        // 最终："/WEB-INF/templates/index.html"
        // 即：访问“/" 时，执行 index()方法，返回视图页面的路径（交给视图解析器）
        return "index";
    }

}

```



>   8.   成功访问首页`/`

![image-20220201150936784](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220201150936784.png)







>   9.   页面跳转

`index.html`:

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>首页</title>
</head>
<body>
    <h1>首页</h1>
    <a th:href="@{ /target }" >目标页</a>
</body>
</html>
```

`target.html`:

```xml
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org" >
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h1>目标页</h1>
</body>
</html>
```

`请求控制器`：

```java
package com.cyw.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class HelloWorldController {

    @RequestMapping("/")
    public String index(){
        // 返回视图的名称【页面路径 = 前缀 + 视图名称 + 后缀】
        // 最终："/WEB-INF/template/index.html"
        return "index";
    }

    @RequestMapping("/target")
    public String toTarget(){

        return "target";
    }

}
```





### 1.5、Hello World 案例总结



总结：

![image-20220201154334899](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220201154334899.png)

以上案例在进行页面跳转时，使用的是请求转发的方式。



---



## 2、SpringMVC 的注解















---



## 3、SpringMVC 的数据绑定

















---



## 4、SpringMVC 的JSON与RESTful交互

















---



## 5、SpringMVC 的拦截器



























---



## 6、SSM 整合

