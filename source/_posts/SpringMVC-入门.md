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

**注**：三层架构分为表示层、业务逻辑层、数据访问层。表示层主要是 **前端页面 + 后端的Servlet** 。



<font style="color:red;font-size:1.3em;">！！注意：！！！！！！</font>

>   Spring 的配置文件 和  SpringMVC的配置文件可以是同一个文件，但为了方便管理，一般分开写。【格式完全一样】



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


<p th:text="${username}"></p>
    

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

-   控制器中实现页面跳转：

    -   请求转发：`return "forward: success"`
    -   重定向：`return "redirect: success"`

    



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



环境搭建【视频】：https://www.bilibili.com/video/BV1Ry4y1574R?p=35





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
        ,params = {"username","password != 123456"}	// 此处，参数username是为必选
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



#### 2.2.8、解决请求参数的乱码问题（编码过滤器）：

url 传参乱码：Tomcat 的`server.xml`中的编码配置问题。

post 传参乱码：编码不一致。



<font style="color:red;">解决方案：</font>配置 编码过滤器，并在`web.xml`中注册。



<font style="color:red;">注意：【编码过滤器要配在最前面，一旦之前获取过请求参数，则该过滤器失效】。</font>



`web.xml`添加如下配置：

```xml
<!-- 此处省略了xml的声明标签、注册DispacherServlet的配置 -->  

<filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <!-- request 编码 -->
    <init-param>
      <param-name>encoding</param-name>
      <param-value>UTF-8</param-value>
    </init-param>
     <!-- response 编码 -->
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

>   -   `ServletAPI `向 **Request** 域对象 共享数据
>   -   `ModelAndView `向 **Request** 域对象 共享数据【**推荐使用**】
>   -   `Model `向 **Request** 域对象 共享数据
>   -   `Map `向 **Request** 域对象 共享数据
>   -   `ModelMap `向 **Request** 域对象 共享数据
>   -   `Session `域对象 共享数据
>   -   `Application `域对象（**ServletContext** 对象） 共享数据



---



#### 2.3.1、Request 域对象（ServletAPI）

ServletAPI 的方式：控制器方法传入HttpServletRequest、HttpServletResponse、HttpServletSession。



<font style="color:red">控制器：</font>

```java
@Controller
public class HelloController {
	
	@ReuquestMapping("/success")
	public String test01(HttpServletRequest  req){
        
        req.setAttribute("testRequestScope","hello,servletAPI");
        
        return "success";
    }
}
```

<font style="color:red">前端：</font>

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title> 
</head>
<body>
    
    <h1>首页</h1>
    
    // thmeleaf 获取 request 域中键为testReqScope的数据
    <p th:text="${testRequestScope}"></p>
    
</body>
</html>
```



---



#### 2.3.2、Request 域对象（ModelAndView）【推荐使用】

【推荐使用】

`ModelAndView`对象：其中的 `Model ` 指的是往域对象中共享数据的功能，`View `指的是指定视图名称并解析最终跳转到页面的过程。



<font style="color:red">注意：</font>

-   返回值必须是`ModelAndView`类型
-   `ModelAndView`对象需要调用`addObject(key,value);`方法来共享数据，类似 Servlet编程时的 `request `对象的 `setAttribute(key,value);`方法。
-   `ModelAndView`对象需要调用`setViewName(视图名);`方法，来设置视图名（转发给哪个页面）



<font style="color:red">控制器的方法：</font>

```java
 	@RequestMapping("/mav")
    public ModelAndView testMav(){
        ModelAndView mav = new ModelAndView();
        // 共享数据
        mav.addObject("testRequestScope","hello,mav");
        // 转发页面
        mav.setViewName("rst");
        return mav;
    }
```



<font style="color:red">前端 res.html：</font>

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h1>结果页：</h1>  
    
    ModelAndView域:【地址栏访问：http://ip:端口/项目名/mav 时跳转到该页面】
    <p  th:text="${testRequestScope}"></p>

</body>
</html>
```





---



#### 2.3.3、Request 域对象（Model）



<font style="color:red">注意：</font>

-   控制器方法的**形参**必须要有`Model`对象
-   控制器方法的**返回值**必须要是**视图名**
-   `Model`对象需要调用`addAttribute(key,value);`方法来共享数据，类似 Servlet编程时的 `request `对象的 `setAttribute(key,value);`方法。



<font style="color:red">控制器：</font>

```java
 	@RequestMapping("/modelTest")
    public String testMav(Model model){
        
        model.addAttribute("testRequestScope","hello,model");      
        return rst;
    }
```



<font style="color:red">前端：</font>

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h1>结果页：</h1>  
    
    Model域:【地址栏访问：http://ip:端口/项目名/modelTest 时跳转到该页面】
    <p  th:text="${testRequestScope}"></p>

</body>
</html>
```





---



#### 2.3.4、Request 域对象（Map）

控制器方法的形参若为`Map`类型的集合，则该Map集合就是`Request`域中**共享的数据**。

<font style="color:red">控制器的方法：</font>

```java
 	@RequestMapping("/mapTest")
    public String testMav(Map<String,Object> map){
        
        map.put("testRequestScope","hello,map");      
        return rst;
    }
```



<font style="color:red">前端：</font>

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h1>结果页：</h1>  
    
    Map域:【地址栏访问：http://ip:端口/项目名/mapTest 时跳转到该页面】
    <p  th:text="${testRequestScope}"></p>

</body>
</html>
```











---



#### 2.3.5、Request 域对象（ModelMap）



<font style="color:red">控制器的方法：</font>

```java
 	@RequestMapping("/modelMapTest")
    public String testMav(ModelMap modelmap){
        
        modelmap.put("testRequestScope","hello,ModelMap");      
        return rst;
    }
```



<font style="color:red">前端：</font>

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h1>结果页：</h1>  
    
    ModelMap域:【地址栏访问：http://ip:端口/项目名/modelMapTest 时跳转到该页面】
    <p  th:text="${testRequestScope}"></p>

</body>
</html>
```













---



#### 2.3.6、Model、ModelMap、Map 之间的关系

Model、ModelMap、Map类型的参数其实本质上都是 `BindingAwareModelMap `类型的

```java
public interface Model{}

public class ModelMap extends LinkedHashMap<String, Object> {}

public class ExtendedModelMap extends ModelMap implements Model {}

public class BindingAwareModelMap extends ExtendedModelMap {}
```

这几种方式最终都会返回 `ModelAndView `对象





---



#### 2.3.7、Session 域对象



<font style="color:red">控制器的方法：</font>

```java

@RequestMapping("/sessionTest")
public String testSession(HttpSession session){
    session.setAttribute("testSessionScope", "hello,session");
    return "rst";
}
```

<font style="color:red">前端：</font>

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h1>结果页：</h1>  
    
    Session域:【地址栏访问：http://ip:端口/项目名/sessionTest 时跳转到该页面】
    <p  th:text="${sesssion.testSessionScope}"></p>

</body>
</html>
```









---



#### 2.3.8、Application 域对象（ServletContext 对象）



<font style="color:red">控制器的方法：</font>

```java
    @RequestMapping("/testApplication")
    public String testApplication(HttpSession session){
        ServletContext application = session.getServletContext();
        application.setAttribute("testApplicationScope", "hello,application");
        return "rst";
    }
```









---



## 3、SpringMVC 的数据绑定

部分内容见【`2.2`】小节。



SpringMVC 在程序执行过程中，将**前端的请求参数**通过一定的方式*绑定*到**控制器参数**中。

即：在**数据绑定**的过程中，SpringMVC 将 `ServletRequet `对象 传递给 `DataBinder` 组件，然后 `DataBinder` 组件 调用 `ConversionService `组件将**请求参数的内容**进行类型与格式的转化，并填充到**参数对象**中，之后调用`Validator`组件对**参数对象**进行数据的合法性校验。最后将校验后的`BindingResult`对象赋值给控制器方法相应的参数。





### 3.1、SpringMVC 的绑定 默认数据类型



<font style="color:red;">常用的默认数据类型：</font>

>   -   `HttpServletRequest`
>   -   `HttpServletResponse`
>   -   `HttpSession`
>   -   `Model 或者 ModelMap`：Model 是一个接口，ModelMap 是Model  接口的一个实现类（作用：将Model 数据填充到 Request 域）



<font style="color:red;">步骤：</font>

-   导入Jar 包
-   在 `web.xml`文件中，配置前端控制器
-   在 `applicationContext.xml`（Spring 的配置文件）中，配置 视图解析器、组件扫描。
-   创建一个控制器类，使用`@Controller`注解修饰整个类，使用`@RequestMapping`注解修饰控制器方法
-   编写控制器方法、要返回的页面。



控制器：

```java

@Controller
public class HelloController{
    
    @ReuqestMapping("/testParam01")
    public String test01(HttpServletRequest req){
        
        String username = req.getParamter("username");
        System.out.println(username);
        
        return "success";
    }
    
}
```





### 3.2、SpringMVC 的绑定 简单数据类型



<font style="color:red;">步骤：</font>

-   导入Jar 包
-   在 `web.xml`文件中，配置前端控制器
-   在 `applicationContext.xml`（Spring 的配置文件）中，配置 视图解析器、组件扫描。
-   创建一个控制器类，使用`@Controller`注解修饰整个类，使用`@RequestMapping`注解修饰控制器方法
-   编写控制器方法、要返回的页面。



控制器：

```java

@Controller
public class HelloController{
    
    @ReuqestMapping("/testParam02")
    public String test02(Integer uid){        
       
        System.out.println(uid);
        
        return "success";
    }
    
}
```





### 3.3、SpringMVC 的绑定 POJO



<font style="color:red;">步骤：</font>

-   导入Jar 包
-   在 `web.xml`文件中，配置前端控制器
-   在 `applicationContext.xml`（Spring 的配置文件）中，配置 视图解析器、组件扫描。
-   创建一个控制器类，使用`@Controller`注解修饰整个类，使用`@RequestMapping`注解修饰控制器方法
-   编写控制器方法、要返回的页面。



POJO：

```java
import lombok.*;

@Data
public class User{
    private int uid;
    private String username;
    private String pwd;
}
```



控制器：

```java

@Controller
public class HelloController{
    
    @ReuqestMapping("/testParam03")
    public String test03(User user){        
       	// 当前端的参数名 与 POJO对象的属性名 可以一一对应时，可以自动封装为POJO
        System.out.println(user);
        
        return "success";
    }
    
}
```





### 3.4、SpringMVC 的绑定 包装POJO

包装POJO 的前端部分必须符合以下规则：

>   -   前端请求的参数名：为直接基本属性时，前端请求的参数名 与 POJO 的属性名一致。
>   -   前端请求的参数名：为POJO属性的子属性时，前端请求的参数名要带上POJO的属性名（如：`dept.deptName`）



<font style="color:red;">步骤：</font>

-   导入Jar 包
-   在 `web.xml`文件中，配置前端控制器
-   在 `applicationContext.xml`（Spring 的配置文件）中，配置 视图解析器、组件扫描。
-   创建一个控制器类，使用`@Controller`注解修饰整个类，使用`@RequestMapping`注解修饰控制器方法
-   编写控制器方法、要返回的页面。



包装POJO：【有属性为 class 类型】

```java
import lombok.*;

@Data
public class User{
    private int uid;
    private String username;
    private String pwd;
    private Dept dept;	// 部门
}
```



包装POJO内部的POJO：

```java
import lombok.*;

@Data
public class Dept{
    private int id;
    private String deptName;
}
```



控制器：

```java

@Controller
public class HelloController{
    
    @ReuqestMapping("/testParam04")
    public String test04(User user){  
        
       	Dept dept = user.getDept();
        System.out.println(dept);
        
        return "success";
    }
    
}
```







### 3.5、SpringMVC 的绑定 数组



绑定 数组 ，也就是前端传过来的是 checkbox 的数据。



<font style="color:red;">步骤：</font>

-   导入Jar 包
-   在 `web.xml`文件中，配置前端控制器
-   在 `applicationContext.xml`（Spring 的配置文件）中，配置 视图解析器、组件扫描。
-   创建一个控制器类，使用`@Controller`注解修饰整个类，使用`@RequestMapping`注解修饰控制器方法
-   编写控制器方法、要返回的页面。



控制器：

```java
@Controller
public class HelloController{
    
    @ReuqestMapping("/testParam02")
    public String test02(Integer[] uids){        
       
        if(uids != null){
            for(Integer id : uids){
                System.out.println(uid);
            }            
        }        
        
        return "success";
    }
    
}
```





### 3.6、SpringMVC 的绑定 集合



<font style="color:red;">绑定 集合时，无法直接使用，必须先创建一个POJO类，然后将集合作为POJO的成员变量，最后在控制器方法的参数中，将该POJO 作为参数类型。</font>



POJO：

```java
import lombok.*;

@Data
public class CollectionPOJO{
    private List<User> list;
}
```



控制器：

```java
@Controller
public class HelloController{
    
    @ReuqestMapping("/testParam02")
    public String test02(CollectionPOJO pojo){        
       List<User> list = pojo.getList();
       
       for(User user : list){
            System.out.println(user);
       } 
        
        return "success";
    }    
}
```



---



## 4、SpringMVC 的视图



SpringMVC中的视图是`View接口`，视图的作用**渲染数据**，将模型Model中的数据展示给用户。

SpringMVC视图的种类很多，默认有：**转发视图**、**重定向视图**。

当工程引入`jstl`的依赖，*转发视图*会自动转换为`JstlView`。

若使用的视图技术为`Thymeleaf`，在SpringMVC的配置文件中配置了 Thymeleaf 的视图解析器，由此视图解析器解析之后所得到的是 `ThymeleafView `。



### 4.1、ThymeleafView



当控制器方法中所设置的**视图名称没有任何前缀**时，此时的视图名称会被SpringMVC配置文件中所配置的视图解析器解析，视图名称拼接视**图前缀**和**视图后缀**所得到的**最终路径**，会通过**转发**的方式**实现跳转**。





### 4.2、转发视图



SpringMVC中默认的**转发视图**是`InternalResourceView`。



”转发“，有资格获取`/WEB-INF`目录下的资源，“重定向”没资格。

”转发“，无资格跨域，“重定向”有资格跨域。



SpringMVC 中创建**转发视图**的情况：

>   当控制器方法中所设置的视图名称以`"forward:"`为前缀时，创建`InternalResourceView`视图，此时的视图名称**不会**被SpringMVC配置文件中所配置的视图解析器解析，而是会将前缀`"forward:"`去掉，**剩余部分**作为**最终路径**通过**转发的方式**实现跳转。  
>
>   
>
>   例如：`"forward:/"`，`“forward:/employee”`



控制器的方法：

```java
    
	@RequestMapping("/testForward")
    public String testForward(){
        return "forward:/testHello";
    }

```





### 4.3、重定向视图



SpringMVC中默认的重定向视图是`RedirectView`。



”转发“，有资格获取`/WEB-INF`目录下的资源，“重定向”没资格。

”转发“，无资格跨域，“重定向”有资格跨域。



>   当控制器方法中所设置的视图名称以`"redirect:"`为前缀时，创建`RedirectView`视图，此时的视图名称不会被SpringMVC配置文件中所配置的视图解析器解析，而是会将前缀`"redirect:"`去掉，剩余部分作为最终路径通过重定向的方式实现跳转。
>
>    
>
>   例如：`"redirect:/"`，`“redirect:/employee”`。 
>
>    
>
>   **注意：**重定向视图在解析时，会先将`redirect:`前缀去掉，然后会判断剩余部分是否以`/`开头，若是则会自动拼接上下文路径。
>
>   



控制器的方法：

```java
    
	@RequestMapping("/testRedirect")
    public String testRedirect(){
        return "redirect:/testHello";
    }
```





### 4.4、视图控制器 view-controller



当控制器方法中，仅仅用来实现页面跳转，即：只需要设置视图名称时，可以将处理器方法使用`view-controller`标签进行表示。

注意：

>   当SpringMVC中设置任何一个`view-controller`时，其他控制器中的请求映射将**全部失效**，
>
>   此时需要在`SpringMVC的配置文件`中设置开启mvc注解驱动的标签：`<mvc:annotation-driven />`。





SpringMVC的配置文件中：

```xml
<!--
    path：设置处理的请求地址（相当于：@RequestMapping的地址）
    view-name：设置请求地址所对应的视图名称（相当于：页面的名称）
-->
	<mvc:view-controller path="/testView" view-name="success" />
	
<!--
	开启 MVC 的注解启动
-->
	<mvc:annotation-driven />
```





---



## 5、SpringMVC 与 RestFul 交互（HiddenHttpMethodFilter）



### 5.1、RestFul 简介



REST：**Re**presentational **S**tate **T**ransfer，表现层资源状态转移。



REST-解释：

>   1、**资源的表述**：例如，HTML/XML/JSON/纯文本/图片/视频/音频等等。资源的表述格式可以通过协商机制来确定。请求-响应方向的表述通常使用不同的格式。
>
>   2、**状态转移**：在客户端和服务器端之间转移（transfer）代表资源状态的表述。通过转移和操作资源的表述，来间接实现操作资源的目的。



### 5.2、RestFul 的实现

具体说，就是 HTTP 协议里面，四个表示操作方式的动词：`GET`、`POST`、`PUT`、`DELETE`。



它们分别对应四种基本操作：

>   -   GET ：用来获取资源
>   -   POST  ：用来新建资源
>   -   PUT ： 用来更新资源
>   -   DELETE  ：用来删除资源



REST 风格提倡 URL 地址使用统一的风格设计，从前到后各个单词使用**斜杠**分开，**不使用问号键值对**方式携带请求参数，而是将要发送给服务器的**数据作为 URL 地址的一部分**，以保证整体风格的一致性。

| 操作     | 传统方式         | REST风格                   |
| -------- | ---------------- | -------------------------- |
| 查询操作 | getUserById?id=1 | user/1   –> get请求方式    |
| 保存操作 | saveUser         | user       –> post请求方式 |
| 删除操作 | deleteUser?id=1  | user/1   –> delete请求方式 |
| 更新操作 | updateUser       | user       –> put请求方式  |



`HiddenHttpMethodFilter`过滤器可以将`POST`请求转化为浏览器目前不支持的`put `和 `delete `请求。

`HiddenHttpMethodFilter` 处理`put`和`delete`请求的两个条件：

>   -   当前请求的请求方式必须为`post`
>
>   -   当前请求必须传输请求参数`_method`(input隐藏域标签的name为`_method`，value属性的值为`put`或`delete`)
>
>   满足以上条件，**`HiddenHttpMethodFilter`** 过滤器就会将当前请求的请求方式转换为`请求参数\_method`的值，因此请求参数`_method`的值才是最终的请求方式。（在`web.xml`中配置）



<font style="color:red;">web.xml：</font>

```xml
// 以下只展示了该章节的核心配置
// 先注册 SpringMVC 的字符过滤器，再注册以下过滤器

    <filter>
        <filter-name>HiddenHttpMethodFilter</filter-name>
        <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
    </filter>

    <filter-mapping>
        <filter-name>HiddenHttpMethodFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```



<font style="color:red;">前端：</font>

```html

<form method="post" th:action="@{/user}">
    		<input type="hidden" name="_method" value="PUT">
    用户名： <input type="text" name="username"/>
    		<input type="submit" />
</form>

```





<font style="color:red;font-size:1.3em;">小结：</font>

> - `web.xml`中，配置`HiddenHttpMethodFilter`过滤器。
> - **控制器**的某个方法上的 `@RequestMapping`注解的method参数中 ，将 HTTP 请求方式设为 `put` 或 `delete`。
> - **前端页面**，form 表单的 method 属性设为`post`，表单内必须要有一个隐藏域标签`<input type="hidden" name="_method" value="put">`或`<input type="hidden" name="_method" value="delete">`





### 5.3、放行静态资源



- [放行静态资源-b站](https://www.bilibili.com/video/BV1Ry4y1574R?p=65)

<font style="color:red;">SpringMVC.xml：</font>

```xml
	<mvc:delault-servlet-handler />
```









---



## 6、SpringMVC 与 JSON 交互（HttpMessageConverter）



`HttpMessageConverter`，报文信息转换器，将**请求报文**转换为**Java对象**，或将**Java对象**转换为**响应报文**。

`HttpMessageConverter`提供了两个**注解**和两个**类型**：

>   （在控制器的方法上使用）
>
>   两个**注解**：
>
>   -   `@RequestBody`：用来修饰一个代表**请求体**的形参
>   -   `@ResponseBody`：用来修饰一个**控制器的方法**（将方法的最终返回结果作为响应体）
>
>   两个**类型**：
>
>   -   `RequestEntity`：整个完整的请求报文
>   -   `ResponseEntity`：整个完整的响应报文



### 6.1、@RequestBody 注解

【在形参上】

`@RequestBody`可以获取请求体，需要在控制器方法设置一个**形参**，使用`@RequestBody`进行标识，当前请求的请求体就会为当前注解所标识的形参赋值。【post 请求报文才有请求体】



```java
 // 前端   
	<form th:action="@{/testRequestBody}" method="post">
        用户名：<input type="text" name="username"><br>
        密码：<input type="password" name="password"><br>
        <input type="submit">
    </form>
        
//=====================================
        
// 控制器的方法        
    @RequestMapping("/testRequestBody")
    public String testRequestBody(@RequestBody String requestBody){
        System.out.println("requestBody:=>"+requestBody);
        return "success";
    }

// 输出结果【控制台】
// requestBody:=> username=admin&password=123456
```



### 6.2、RequestEntity 类型

【在形参上】

`RequestEntity`封装请求报文的一种类型，需要在控制器方法的形参中设置该类型的形参，当前请求的请求报文就会赋值给该形参，可以通过getHeaders()获取请求头信息，通过getBody()获取请求体信息。



```java
// ... 控制器方法    
	@RequestMapping("/testRequestEntity")
    public String testRequestEntity(RequestEntity<String> requestEntity){
        
        System.out.println("requestHeader:"+requestEntity.getHeaders());
        System.out.println("requestBody:"+requestEntity.getBody());
        return "success";
    }

// =====================================================
/*
	输出结果： 
		requestHeader:[ host:“localhost:8080”,
						connection:“keep-alive”, 
						content-length:“27”, 
						cache-control:“max-age=0”, 
						sec-ch-ua:"" Not A;Brand";v=“99”,
                        “Chromium”;v=“90”,
                        “Google Chrome”;v=“90"”,
                        sec-ch-ua-mobile:"?0",
                        upgrade-insecure-requests:“1”,
                        origin:“http://localhost:8080”, 
                        user-agent:“Mozilla/5.0 (Windows NT 10.0; Win64; x64)”
                      ] 	
        requestBody：username=admin & password=123

*/
```





### 6.3、@ResponseBody注解

【在控制器方法上】

`@ResponseBody`用于标识一个**控制器方法**，可将该方法的返回值直接作为响应报文的响应体响应到浏览器。



```java
// 页面显示：“success”字符串  

	@RequestMapping("/testResponseBody")
    @ResponseBody
    public String testResponseBody(){
        return "success";
    }

```



### 6.4、ResponseEntity 类型

【控制器方法的返回值】

`ResponseEntity `用于作为控制器方法的**返回值类型**，该控制器方法的返回值就是响应到浏览器的响应报文。



<font style="color:red;">使用场景：</font>文件下载



控制器：

```java
// ... 省略了控制器的其他方法

    @RequestMapping("/testResponseEntity")
    public ResponseEntity testResponseEntity(){
        
        return ResponseEntity;
    }
```





### 6.5、@ResponseBody 处理JSON：

#### 6.5.1、步骤：

`@ResponseBody`处理 `JSON `的步骤：

>   -   导入`jackson`的依赖。
>   -   在SpringMVC的核心配置文件中，**开启mvc的注解驱动**。
>   -   使用`@ResponseBody`注解标识一个控制器方法。
>   -   控制器方法返回一个POJO（自动转化为JSON）



<font style="color:red;">（1）导入Jackson 依赖：</font>

```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.12.1</version>
</dependency>
```



<font style="color:red;">（2）SpringMVC配置文件中，开启mvc注解驱动：</font>

```xml
	<mvc:annotation-driven />
```



<font style="color:red;">（3）使用`@ResponseBody`注解标识一个控制器方法</font>

<font style="color:red;">（4）将Java对象直接作为控制器方法的**返回值**返回，就会自动转换为 JSON 格式的字符串</font>

```java
    
			
// (3)    	
        @ResponseBody
        @RequestMapping("/testResponseUser")
        public User testResponseUser(){
            // (4)
            return new User(1001,"admin","123456",23,"男");
        }

// 浏览器的页面中展示的结果：
/*
	{	
		“id”:1001,
		“username”:“admin”,
		“password”:“123456”,
		“age”:23,“sex”:“男”
    }
*/
```



#### 6.5.2、整体演示：

<font style="color:red;">（1）前端：</font>

```html
    <div id="app">
        <a th:href="@{/testAjax}" @click="testAjax">testAjax</a><br>
    </div>

// ============================================

<script type="text/javascript" th:src="@{/static/js/vue.js}"></script>
<script type="text/javascript" th:src="@{/static/js/axios.min.js}"></script>
<script type="text/javascript">
    
    var vue = new Vue({
        el:"#app",
        methods:{
            testAjax:function (event) {
                axios({
                    method:"post",
                    url:event.target.href,
                    params:{
                        username:"admin",
                        password:"123456"
                    }
                }).then(function (response) {
                    alert(response.data);
                });
                event.preventDefault();
            }
        }
    });
</script>
```

<font style="color:red;">（2）控制器方法：</font>

```java
    
	@RequestMapping("/testAjax")
    @ResponseBody
    public String testAjax(String username, String password){
        System.out.println("username:"+username+",password:"+password);
        return "hello,ajax";
    }
```





### 6.6、@RestController注解【重点】

`@RestController`注解是SpringMVC提供的一个**复合注解**，标识在**控制器的类**上，

就相当于为类添加了`@Controller`注解，并且为其中的每个方法添加了`@ResponseBody`注解。

即：【`@Controller` + `@ResponseBody`】









### 6.7、文件下载与上传-案例



- [文件上传、下载-案例—B站](https://www.bilibili.com/video/BV1Ry4y1574R?p=75)



#### 6.7.1、文件下载

文件下载案例：使用了`ResponseEntity`类型



<font style="color:red;">控制器方法：</font>

```java
    @RequestMapping("/testDown")
    public ResponseEntity<byte[]> testResponseEntity(HttpSession session) throws IOException {
        
        //获取ServletContext对象
        ServletContext servletContext = session.getServletContext();
        
        //获取服务器中文件的真实路径
        String realPath = servletContext.getRealPath("/static/img/1.jpg");
        
        //创建输入流
        InputStream is = new FileInputStream(realPath);
        
        //创建字节数组
        byte[] bytes = new byte[is.available()];
        
        //将流读到字节数组中
        is.read(bytes);
        
        //创建HttpHeaders对象设置响应头信息
        MultiValueMap<String, String> headers = new HttpHeaders();
        
        //设置要下载方式以及下载文件的名字
        headers.add("Content-Disposition", "attachment;filename=1.jpg");
        
        //设置响应状态码
        HttpStatus statusCode = HttpStatus.OK;
        
        //创建ResponseEntity对象
        ResponseEntity<byte[]> responseEntity = new ResponseEntity<>(bytes, headers, statusCode);
        
        //关闭输入流
        is.close();
        return responseEntity;
    }

```



#### 6.7.2、文件上传



文件上传：要求`form`表单的**请求方式**必须为`post`，并且添加属性`enctype="multipart/form-data"`



SpringMVC 中将上传的文件封装到`MultipartFile`对象中，通过此对象可以获取文件相关信息。



<font style="color:red;font-size:1.3em;">步骤：</font>

> - 导入 `commons-fileupload.jar`包**依赖**
> - 在 SpringMVC 的配置文件中，添加**文件解析器**的配置
> - 编写**控制器方法**



<font style="color:red;">（1）导入maven 依赖：</font>

```xml
   
	<dependency>
        <groupId>commons-fileupload</groupId>
        <artifactId>commons-fileupload</artifactId>
        <version>1.3.1</version>
    </dependency>

```



<font style="color:red;">（2）在SpringMVC配置文件中，配置文件解析器：</font>

```xml
<!-- id必须有，且为 multipartResolver -->
<bean id="multipartResolver" 
      class="org.springframework.web.multipart.commons.CommonsMultipartResolver"/>

```



<font style="color:red;">（3）控制器方法：</font>

```java
    
	@RequestMapping("/testUp")
    public String testUp(MultipartFile photo, HttpSession session) throws IOException {
        
        //获取上传的文件的文件名
        String fileName = photo.getOriginalFilename();
        
        //处理文件重名问题
        String houzuiName = fileName.substring(fileName.lastIndexOf("."));        
        fileName = UUID.randomUUID().toString() + houzuiName;
        
        //获取服务器中photo目录的路径
        ServletContext servletContext = session.getServletContext();
        
        String photoPath = servletContext.getRealPath("photo");
        
        File file = new File(photoPath);
        
        if(!file.exists()){
            file.mkdir();
        }
        
        String finalPath = photoPath + File.separator + fileName;
        
        //实现上传功能
        photo.transferTo(new File(finalPath));
        
        return "success";
    }


```







---



## 7、SpringMVC 的拦截器



拦截器（Interceptor），类似 Servet 编程时的 Filter 过滤器的作用，可以使用拦截器进行权限验证。



过程：

> 前端页面 =》Filter  =》DispacherServlet  =》拦截器 =》HandlerMapping =》HandlerMapping  =》HandlerAdaptor =》 Handler （Controller）



自定义拦截器类的两种方式：

>   -   实现`HandlerInterceptor`接口 或者 继承`HandlerInterceptor`接口 的实现类。
>   -   实现`WebRequestInterceptor`接口 或者 继承`WebRequestInterceptor`接口 的实现类。



以实现`HandlerInterceptor`接口为例：

```java

public classMyInterceptor implements HandlerInterceptor{
    
    // 在控制器方法之前执行，返回值为true时，才继续执行；返回false时，中断之后的操作
    @Override
    public boolean preHandle(HttpServletRequest req
                            ,HttpServletResponse resp
                            ,Object obj )
    {
        ...
    }
    
    // 在控制器方法调用之后，视图解析之前执行【可修改ModelAndView对象】
    @Override
    public void postHandle(HttpServletRequest req
                            ,HttpServletResponse resp
                            ,Object obj.
                          	,ModelAndView mav)
    {
        ...
    }
    
    // 视图渲染完成，请求完毕之后执行【清理资源】
    @Override
    public void afterCompletion(HttpServletRequest req
                            ,HttpServletResponse resp
                            ,Object obj.
                          	,ModelAndView mav)
    {
        ...
    }
}

```



<font style="color:red;">步骤：</font>

>   -   定义一个拦截器类，实现拦截器接口（见上面的示例）
>   -   在`SpringMVC.xml` （SpringMVC的配置文件）中配置拦截器



`SpringMVC.xml` ：

```xml
// 以下只展示核心配置

    <mvc:interceptors>
         // 注册拦截器类
            <bean class="com.cyw.interceptor.MyInterceptor" />

         // 配置拦截器【第一个拦截器】
            <mvc:interceptor>
                // 拦截的路径
                <mvc:mapping path="/**"/>

                // 不拦截的路径
                <mvc:exclude-mapping path=""/>

                // 匹配的请求才进行拦截
                <bean class="com.cyw.interceptor.TestInterceptor01" />
            </mvc:interceptor>


         // 配置拦截器【第二个拦截器】
            <mvc:interceptor>
                // 拦截的路径
                <mvc:mapping path="/**"/>

                // 匹配的请求才进行拦截
                <bean class="com.cyw.interceptor.TestInterceptor02" />
            </mvc:interceptor>
    </mvc:interceptors>
```



---



## 8、异常处理器



SpringMVC 提供了一个处理控制器方法执行过程中所出现的异常的接口：`HandlerExceptionResolver`

`HandlerExceptionResolver `接口的实现类有：

> - `DefaultHandlerExceptionResolver`
> - `SimpleMappingExceptionResolver`



### 8.1、基于 XML 的异常处理器



SpringMVC 提供了自定义的异常处理器`SimpleMappingExceptionResolver`。



`SpringMVC`配置文件：

```xml
<bean class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
    <property name="exceptionMappings">
        <props>
            <!--
                properties的键表示处理器方法执行过程中出现的异常
                properties的值表示若出现指定异常时，设置一个新的视图名称，跳转到指定页面
            -->
            <prop key="java.lang.ArithmeticException">error</prop>
        </props>
    </property>
    <!--
        exceptionAttribute属性设置一个属性名，将出现的异常信息在请求域中进行共享
    -->
    <property name="exceptionAttribute" value="ex"></property>
</bean>

```







### 8.2、基于 注解 的异常处理器



```java
    
// @ControllerAdvice 将当前类标识为异常处理的组件

    @ControllerAdvice
    public class ExceptionController {

        // @ExceptionHandler用于设置所标识方法处理的异常
        // ex表示当前请求处理中出现的异常对象
        @ExceptionHandler(ArithmeticException.class)        
        public String handleArithmeticException(Exception ex, Model model){
            model.addAttribute("ex", ex);
            return "error";
        }

    }

```





























---



## 9、注解配置 SpringMVC































## 10、SpringMVC 的执行流程





























---



## 11、SSM 整合

