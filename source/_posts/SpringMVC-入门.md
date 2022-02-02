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
>   -   [Thymeleaf-服务器模板引擎_动力结点](https://www.bilibili.com/video/BV1cq4y1E79S)



书：

>   -   SSM从零开始学



博客：

>   -   [尚硅谷版-笔记-博客](https://mowangblog.github.io/SpringMVC-Demo/)



---



## 1、SpringMVC 概述



### 1.1、什么是 MVC



M：Model 模型，也就是广义上的JavaBean（Service、Dao、实体类）

V：View 视图，HTML、JSP等前端页面

C：Controller 控制器，也就是Sevlet



【流程】：

![image-20220131101356685](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220131101356685.png)







![SpringMVC核心架构](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220201124418631.png)



---



### 1.2、什么是 Spring MVC

Spring MVC 是Spring 的一个子项目，是Spring 为表示层提供的一整套完备的解决方案。

注：三层架构分为表示层、业务逻辑层、数据访问层。表示层主要是 **前端页面 + 后端的Servlet** 。



### 1.3、Spring MVC 的特点



>   -   **简洁、灵活**，提高开发效率
>   -   Spring 的一部分
>   -   基于原生的Servlet，通过 **DispacherServlet** 来处理请求和响应。
>   -   全面覆盖**表示层需**要解决的问题
>   -   组件化程度高，**即插即用**
>   -   **性能好**，适合大型、超大型互联网项目的要求。



### 1.4、Thymeleaf 基本用法

Thymeleaf 是一个服务端的Java 模板引擎，通过在 HTML 标签中嵌入特殊的语法糖，将后端的数据渲染到前端的模板页（替换模板页的占位符部分），生成结果文件，实现页面的展示效果。

Thymeleaf 的功能类似 JSTL 标签库，但与JSTL不同的是，浏览器可以直接预览模板文件，无需服务端支持。



官网：[Thymeleaf](https://www.thymeleaf.org/)



<font style="color:red;">业界常用的模板：</font>

>   -   FreeMarker
>   -   Thymeleaf
>   -   Velocity



<font style="color:red;">Thymeleaf 的 Maven 坐标：</font>

```xml
<dependency>
  <groupId>org.thymeleaf</groupId>
  <artifactId>thymeleaf</artifactId>
  <version>3.0.14.RELEASE</version>
</dependency>
```



<font style="color:red;">语法：</font>

```html
// 使用语法：th:属性名=“${后端属性名}”
// 在以下示例中，不连接服务器时，input标签的值是张三，连接服务器时，被替换
<input type="text" 
       name="userName" 
       value="张三" 
       th:value="${user.name}" />
```



<font style="color:red;">简单表达式：</font>

-   变量表达式：`${...}`
-   选择变量表达式：`*{...}`
-   消息表达式：`#{...}`
-   链接网址表达式：`@{...}`
-   片段表达式：`~{...}`





<font style="color:red;">url 传参：</font>

```html
<a th:href="@{ /test/hello(username=‘zs’,pwd='root') }">
	跳转到 /test/hello 页面，并传参username 和 pwd
</a>
```





<font style="color:red;">案例：</font>

1.   创建普通的Maven项目，pom.xml 中导入坐标
2.   创建` HelloThymeleaf`测试类

```java

public class HelloThymeleaf{
    
    @Test
    public void test01(){        
        // 创建模板引擎
        TemplateEngine engine = new TemplateEngine();
        
        // 准备模板
        String inputStr = "<input type='text' th:value='${txt}'/>";
        
        // 准备数据，使用Context
        Context context =  new Context();
        context.setVariable("txt","hello_boys");
        
        // 处理模板,生成最终的html
        String out = engine.process(inputStr,context);        
    }
    
    
    @Test
    public void test02(){        
        // 创建模板引擎
        TemplateEngine engine = new TemplateEngine();
        
        // 创建一个类加载器模板解析器
        ClassLoaderTemplateResolver resolver = new ClassLoaderTemplateResolver();
        
        // 设置模板解析器
        engine.setTemplateResolver(resolver);
        
        // 准备数据，使用Context
        Context context =  new Context();
        context.setVariable("txt","hello_boys");        
        
        // 处理模板,生成最终的html,传给前端（此案例前端页面以定义好模板）
        String outHtml = engine.process("main.html",context);      
    }
    
    @Test
    public void test03(){        
        // 创建模板引擎
        TemplateEngine engine = new TemplateEngine();
        
        // 创建一个类加载器模板解析器
        ClassLoaderTemplateResolver resolver = new ClassLoaderTemplateResolver();
        
        // 设置前缀、后缀
        // 完整路径：templates/index.html
        resolver.setPrefix("templates/");
        resolver.setSuffix(".html");
        
        // 设置模板解析器
        engine.setTemplateResolver(resolver);
        
        // 准备数据，使用Context
        Context context =  new Context();
        context.setVariable("txt","hello_boys");        
        
        // 处理模板,生成最终的html,传给前端（此案例前端页面以定义好模板）
        String outHtml = engine.process("index",context);      
    }
}
```







### 1.5、Spring MVC 版-Hello World



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



<font style="color:red;">创建index.html：（需要引入命名空间）</font>

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



<font style="color:red;">在Spring的配置文件中，配置视图解析器（以 thymeleaf 的视图解析器为例）：</font>

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



<font style="color:red;">spring自带的视图解析器：</font>

```xml
<!--    扫描：控制器-->
    <context:component-scan base-package="com.cyw.controller"/>
<!--    注册：处理器映射器，将处理器（控制器）的name作为url来查找-->
    <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
<!--    注册：处理器适配器，配置对handlerRequest()方法的调用-->
    <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
<!--    注册：视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"/>
    
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





### 1.6、Hello World 案例总结



总结：

![image-20220201154334899](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220201154334899.png)

以上案例在进行页面跳转时，使用的是请求转发的方式。



---



## 2、SpringMVC 的注解



SpringMVC 的注解 可以放在类上，也可以放在方法上。



### 2.1、@RequestMapping 注解

#### 2.1.1、@RequestMapping 的功能：

`@RequestMapping` 注解的功能就是建立 **请求路径** 与 **控制器方法** 的映射关系，类似Servlet 中的`@WebServlet`注解的作用。

---



#### 2.1.2、@RequestMapping 的作用位置：

`@RequestMapping` 注解 修饰一个类时：设置请求路径的初始信息

`@RequestMapping` 注解 修饰一个方法时：设置请求路径的具体信息

既修饰一个类，又修饰该类的方法，则先匹配类上的 url ，再匹配方法上的 url

控制器：

```java
@Controller
@RequestMapping("/test")
public class RequestMappingController {

    //此时请求映射所映射的请求的请求路径为：/test/testRequestMapping
    @RequestMapping("/testRequestMapping")
    public String testRequestMapping(){
        return "success";
    }

}
```



---



#### 2.1.3、@RequestMapping 的 value 属性：

`@RequestMapping` 的 `value `属性，也就是 默认传入的属性（**按 url 匹配处理方法**），任何 htttp 的请求方式都行，如：

```java
@RequestMapping("/hello")
public String test01(){...}

------------------------------------------
    
// "/hello" 和“/test” 都匹配 test02 这个 处理器方法
@RequestMapping({"/hello",""/test""})
public String test02(){...}
```



---





#### 2.1.4、@RequestMapping 的 method 属性：

`@RequestMapping` 的 `method `属性，按照 HTTP 的请求方式（get、post、put、delete等）匹配控制器方法。当请求**不满足要求**时，报`405：Request method ‘POST’ not supported`错误。



<font style="color:red;">注意：</font>

目前浏览器只支持get和post，若在form表单提交时，为method设置了其他请求方式的字符串（put或delete），则按照默认的请求方式get处理。



<font style="color:red;">http 请求方式不同的案例：</font>

```java
    <form th:action="@{/test}" method="post">
        <input type="submit">
    </form>
        
// ===========================
    @RequestMapping(
            value = "/test",
            method = {RequestMethod.GET, RequestMethod.POST}
    )
    public String test01(){
        return "success";
    }
```



<font style="color:red;">派生注解：</font>

>   -   处理 get    请求的映射 –> `@GetMapping`
>   -   处理 post 请求的映射  –> `@PostMapping`



---



#### 2.1.4、@RequestMapping 的 params 属性：

@RequestMapping注解的 params 属性通过请求的**请求参数**匹配请求映射。

@RequestMapping注解的 params 属性是一个**字符串类型的数组**，可以通过**四种表达式**设置请求参数和请求映射的匹配关系。

<font style="color:red;">params 属性的值的写法：</font>

>   -   “ 参数 ”：要求请求映射所匹配的请求**必须携带**param 请求参数
>
>   -   “ ! 参数”：要求请求映射所匹配的请求必须**不能携带**param 请求参数
>
>   -   “ 参数 = value ”：要求请求映射所匹配的请求**必须携带**param请求参数且 param = value
>
>   -   “ 参数 != value ”：要求请求映射所匹配的请求**必须携带**param请求参数但是 param != value



```java
    <a th:href="@{/test(username='admin',password=123456)">
        测试@RequestMapping的params属性-->/test
    </a>  
    
// ======================
    
    @RequestMapping(
        value = "/test"
        ,method = {RequestMethod.GET, RequestMethod.POST}
        ,params = {"username","password != 123456"}
    )
    public String testRequestMapping(){
        return "success";
    }
```





---



#### 2.1.4、@RequestMapping 的 headers 属性：

`@RequestMapping`注解的`headers`属性通过请求的请求头信息匹配请求映射

@RequestMapping注解的headers属性是一个**字符串类型的数组**，可以通过四种表达式设置请求头信息和请求映射的匹配关系

“header信息”：要求请求映射所匹配的请求必须携带header请求头信息

“ ! header信息”：要求请求映射所匹配的请求必须不能携带header请求头信息

“header信息= value”：要求请求映射所匹配的请求必须携带header请求头信息且header=value

“header信息 != value”：要求请求映射所匹配的请求必须携带header请求头信息且header!=value

若当前请求满足`@RequestMapping`注解的`value 和 method`属性，但是`不满足headers`属性，此时页面显示`404错误，即资源未找到`



---



#### 2.1.5、SpringMVC支持ant风格的路径

ant风格的路径：路径的通配符匹配。

>   -   `？`：表示任意的**单个**字符 =>  @RequestMapping("/a?b/test")   => {“/a1b/test”，”/a2b/test”}
>
>   -   `*`：表示任意的**0~n个**字符 =>  @RequestMapping("/a*b/test") => {“/ab/test”，”/a1b/test”，”/a123b/test”}
>
>   -   `**`：表示任意的**一层或多层目录**  =>  @RequestMapping("/a**b/test") => {“/a/b/test”，”/a/c/b/test”，”/a/c/d/b/test”}
>
>   注意：在使用 `**` 时，只能使用 `/**/xxx` 的方式



```java
	// 匹配 a开头,b结尾的路径下的test【“/a1b/test”,“/a2b/test”】
	@RequestMapping("/a?b/test")
    public String testRequestMapping(){
        return "success";
    }
```



---



#### 2.1.6、SpringMVC支持 RestFul 风格的路径占位符



原始方式：`/deleteUser?id=1`

RestFul方式：`/deleteUser/1`

SpringMVC路径中的占位符常用于**RESTful**风格中，当请求路径中将某些数据通过`路径的方式`传输到服务器中，就可以在相应的@RequestMapping注解的value属性中通过`占位符{xxx}`表示传输的数据，在通过`@PathVariable`注解，将占位符所表示的数据`赋值`给控制器方法的形参。



```java
    
    <a th:href="@{/testRest/1/admin}">测试路径中的占位符-->/testRest </a>
    	<br>

// ====================
    
    @RequestMapping("/testRest/{id}/{username}")
    public String testRest(@PathVariable("id") String id, 
                           @PathVariable("username") String username){
        
        System.out.println("id:"+id+",username:"+username);
        return "success";
    }
	//最终输出的内容为-->id:1,username:admin

```



---



### 2.2、获取请求参数



#### 2.2.1、传统 Servlet 获取请求参数：

传统 Servlet 获取请求参数 是通过控制器参数中的 `HttpServletRequest `对象的`String getParamter("参数的键")`方法。

注意：前、后端的参数名要一致。

```java

	@ReuquestMapping("/hello/success")
	public String test01(HttpServletRequest  req){
        
        String username = req.getParamter("username");
        String pwd = req.getParamter("pwd");
        System.out.println(username+"--- " + pwd);   
        
        return "success";
    }

```



#### 2.2.2、控制器方法的形参 获取请求参数：

注意：前、后端的参数名要一致。

```java
	@ReuquestMapping("/hello/success")
	public String test01(String username,String pwd){   
        
        System.out.println(username+"--- " + pwd);      
        
        return "success";
    }


	@ReuquestMapping("/hello/test02")
	public String test02(String username,String[] hobby){   
        
        System.out.println(username+"--- " + Arrays.toString(hobby));      
        
        return "success";
    }
```



#### 2.2.3、@RequestParam 获取请求参数：

注意：前、后端的参数名可以不一致。

`@RequestParam()` 注解的作用：建立 **前端的请求参数** 与 **后端控制器方法形参** 的映射。

`@RequestParam()`注解的 `required `属性默认为 `true`，不能装配则报错400，该属性表示是否为必须。

`@RequestParam()`注解的 `defaultValue `属性设置默认值，只要前端传过来的参数不存在或为null 时，就使用注解的 `defaultValue `属性设置的默认值。

```java
	
	@ReuquestMapping("/hello/success")
	public String test01(@RequestParam("user_name") String username)        
    {   
        // 前端表单控件的name属性值为user_name,后端控制器方法的形参为 username
        // 前后端的参数名，不一致时，可以通过@RequestParam注解来建立映射关系
        System.out.println(username);      
        
        return "success";
    }
```



---



#### 2.2.5、@RequestHeader 获取请求参数：

`@RequestHeader `注解的作用：建立 **请求报文头的信息** 与 **控制器方法的形参** 的映射关系。

该注解的三个参数，value、required、defaultValue。这三个注解的参数的用法同` 2.2.3`小节` @RequestParam ` 。





---



#### 2.2.6、@CookieValue 获取请求参数：



`@CookieValue `注解的作用：建立 **Cookie的信息** 与 **控制器方法的形参** 的映射关系。

该注解的三个参数，value、required、defaultValue。这三个注解的参数的用法同` 2.2.3`小节` @RequestParam ` 。



---



#### 2.2.7、POJO 获取请求参数：

将控制器方法的形参设为一个POJO，POJO的属性名与前端请求的参数名一致，那么SpringMVC会将请求参数自动封装为POJO。



前端代码：

```html
    
	<form th:action="@{/test/testParamPOJO}" method="post">
        <input type="text" name="username" />
        <input type="text" name="pwd" />
        <input type="submit" />
    </form>

```

后端代码：

```java
// POJO 中
	@Data
	public class User{
        private String username;
        private String pwd;
    }

// testController 中
	
	public String  testParamPOJO(User user){
        System.out.println(user);
        return "success";
    }


```



---



#### 2.2.8、解决请求参数的乱码问题：

url 传参乱码：Tomcat 的`server.xml`中的编码配置问题。

post 传参乱码：编码不一致。



<font style="color:red;">解决方案：</font>配置 编码过滤器，并在`web.xml`中注册。



`web.xml`添加如下配置：

```xml
  <filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>UTF-8</param-value>
    </init-param>
    <init-param>
      <param-name>forceResponseEncoding</param-name>
      <param-value>true</param-value>
    </init-param>
  </filter>

  <filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>
```



---



### 2.3、域对象共享数据



<font style="color:red;">域对象：</font>

-   在`传统的Servlet`中，域对象主要有 PageContext（JSP页面）、 `Request`、`Session`、`Cookie`、`Application`。
-   在`SpringMVC`中，域对象主要有 `Request`、`Session`、`Application`。



<font style="color:red;">域对象共享数据的几种方式：</font>( 5+1+1)

>   -   ServletAPI 向 Request 域对象 共享数据
>   -   ModelAndView 向 Request 域对象 共享数据
>   -   Model 向 Request 域对象 共享数据
>   -   Map 向 Request 域对象 共享数据
>   -   ModelMap 向 Request 域对象 共享数据
>   -   Session 域对象 共享数据
>   -   Application 域对象 共享数据



---



#### 2.3.1、Request 域对象（ServletAPI）











---



#### 2.3.2、Request 域对象（ModelAndView）











---



#### 2.3.3、Request 域对象（Model）









---



#### 2.3.4、Request 域对象（Map）







---



#### 2.3.5、Request 域对象（ModelMap）









---



#### 2.3.6、Model、ModelMap、Map 之间的关系









---



#### 2.3.7、Session 域对象











---



#### 2.3.8、Application 域对象











---



## 3、SpringMVC 的数据绑定

部分内容见【`2.2`】小节

















---



## 4、SpringMVC 的JSON与RESTful交互

















---



## 5、SpringMVC 的拦截器



























---



## 6、SSM 整合

