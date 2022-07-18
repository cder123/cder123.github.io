---
title: SpringBoot-入门
tag: Java
categories:
  - [后端,Java,SpringBoot]

---





<h1>SpringBoot-入门</h1>

[toc]





# 零、资料



<font style="color:red;font-size:1.3em;">视频：</font>

> - [SpringBoot-黑马-视频](https://www.bilibili.com/video/BV15b4y1a7yG?p=2)
>
> 





<font style="color:red;font-size:1.3em;">文档：</font>

> - [Spring Boot 2 基础篇学习笔记-CSDN博客](https://blog.csdn.net/qq_42324086/article/details/121184789)





<font style="color:red;font-size:1.3em;">国内替换Spring Initizer脚手架：</font>https://start.springboot.io



社区版的配置：

<iframe height="500px" src="//player.bilibili.com/player.html?aid=424305624&bvid=BV1F3411j7jX&cid=516341794&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>







替换JUC（Eclipse）:

<iframe height="500px" src="//player.bilibili.com/player.html?aid=95017741&bvid=BV1bE411T7oZ&cid=162207937&page=31" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>





异常映射的处理（XML）：

<iframe height="500px" src="//player.bilibili.com/player.html?aid=95017741&bvid=BV1bE411T7oZ&cid=162209292&page=55" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>







---





# 一、基础篇



## 1、学习目标



<img src="https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220320091403617.png"/>

<img src="https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220320091439480.png"/>

<img src="https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220320091501359.png"/>











IDEA 隐藏不想看见的文件：

[IDEA隐藏不必要的文件 [例如.mvnw .git .idea\]_头顶凉凉先生丶的博客-CSDN博客_idea隐藏不需要的文件](https://blog.csdn.net/weixin_49904442/article/details/124742810)

<iframe style="height:500px;" src="//player.bilibili.com/player.html?aid=633835873&bvid=BV15b4y1a7yG&cid=430716204&page=7" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>







## 2、入门案例



在开始入门案例前，需要知道SpringBoot是干什么的？

> SpringBoot 是用于简化Spring 环境的搭建和开发的框架。
>
> SpringBoot 内置Tomcat，因此，可以直接运行项目。
>
> SpringBoot 内置了许多配置，因此，即使不写配置文件，也能运行项目。（maven依赖的版本来自`parent`，坐标来自`starter`）
>
> SpringBoot 配置文件 支持两种格式的`properties`和`yml`（推荐）【`application.yml`】



<font style="color:red;">注意：</font>

> - 所有的类都要写在启动类的同包或子包下，否则无法被 SpringBoot 扫描到。





案例-视频：

<iframe style="height:500px;" src="//player.bilibili.com/player.html?aid=633835873&bvid=BV15b4y1a7yG&cid=430715481&page=3" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>





### 2.1、创建 SpringBoot 工程



方式一：官网创建，下载到本地，导入IDEA：[官网创建SpringBoot工程](https://start.spring.io/)

<iframe style="height:500px;" src="//player.bilibili.com/player.html?aid=633835873&bvid=BV15b4y1a7yG&cid=430715789&page=4" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>





方式二：IDEA 创建SpringBoot工程：

<img src="https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220320092105081.png"/>

<img src="https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220320092212445.png"/>

<img src="https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220320094745307.png"/>









### 2.2、添加 Maven 依赖（按需）



默认生成的` POM.xml`文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.6.4</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <groupId>com.cyw</groupId>
    <artifactId>ex01</artifactId>
    <version>0.0.1-SNAPSHOT</version>

    <name>ex01</name>
    <description>ex01</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <excludes>
                        <exclude>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>

```



可以发现，以上默认生成的Maven配置中，最主要的是：

> - `parent`节点：继承SpringBoot的默认配置，SpiringBoot默认配置了许多Maven依赖的版本，通过parent节点，可以直接省略一部分Maven依赖版本号的编写。
> - `dependencies`节点：用于放置Maven配置，一般使用SpringBoot来编写Web项目时，一定会引入`spring-boot-starter-web`依赖、`spring-boot-starter-test`依赖。
> - `build`节点：一般SpringBoot想要运行项目，需要使用Maven来打成 jar 包，因此，需要引入SpringBoot的Maven打包的插件。









### 2.3、修改配置文件（按需）



`application.yml`：

```yml
server:
  port: 8080 # 默认配置（可省略）

```







### 2.4、目录结构：









### 2.5、编写 Controller



```java
package com.cyw.ex01.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;


@RestController
public class IndexController {

    @RequestMapping("/")
    public String index(){
        return "hello world";
    }

}

```









### 2.6、启动项目



在启动主类（引导类）中，运行项目：

```java
package com.cyw.ex01;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Ex01Application {

    public static void main(String[] args) {
        // 返回一个 ApplicationContext 容器
        SpringApplication.run(Ex01Application.class, args);
    }

}

```





### 2.7、更换内置的Tomcat



先排除Tomcat依赖，再导入Jetty依赖。



在`POM.xml`中：

```xml
		<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-tomcat</artifactId>
                </exclusion>
            </exclusions>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jetty</artifactId>
        </dependency>
```







## 3、RestFul 风格开发



RestFul 风格，就是使用HTTP的`get`查询、`post`添加、`put`修改、`delete`删除请求方式来访问Controller，进行增删改查。

(put 和 delete类似)

- `get`请求：参数以名词的形式，拼接在路径上`/books/{id}`（其中，`{id}`是后端`@RequestMapping`注解中的的占位符）。
- `post`请求：参数封装在请求体内，在Controller的控制方法的参数内，使用`@RequestBody`来修饰参数，封装前端传过来的参数。



RestFul 注解：

> - `@RestController`：相当于 `@Controller`+`@ResponseBody`
> - `@RequestMapping("/books")`
> - `@GetMapping("/books")`
> - `@PostMapping("/books")`
> - `@PutMapping`
> - `@DeleteMapping("/books/{id}")`



<img src="https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220320122226069.png"/>







GET请求的案例（控制器中）：

```java
    @RequestMapping("/books/{id}")
    public Result getBookById(@PathVariable("id") int id){
        HashMap<String, Object> map = new HashMap<String, Object>();
        map.put("id",2);
        map.put("bookname","Java编程思想（第2版）");
        // Result类是自己编写的结果集类
        return Result.ok("请求成功",map);
    }

// 两段代码等价
	@GetMapping("/books/{id}")
    public Result getBookById(@PathVariable("id") int id){
        HashMap<String, Object> map = new HashMap<String, Object>();
        map.put("id",2);
        map.put("bookname","Java编程思想（第2版）");
        return Result.ok("请求成功",map);
    }
```





POST请求的案例（控制器中）：

```java
    @RequestMapping(value="/books",method = RequestMethod.POST)
    public Result saveBook(@RequestBody Book book){
        bookService.saveBook(book);
        return Result.ok("保存成功");
    }

// 两段代码等价
    @PostMapping("/books")
    public Result saveBook(@RequestBody Book book){
        bookService.saveBook(book);
        return Result.ok("保存成功");
    }

```





## 4、配置文件



[配置文件-官网文档](https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html#appendix.application-properties)



SpringBoot 支持 <font style="color:red;">3种</font> 配置文件的格式：`.properties`、`.yml`、`.yaml`



配置文件的优先级：

> （1）`.properties`【优先级最高】
>
> （2）`.yml`
>
> （3）`.yaml`



多个不同格式的配置文件共存：

> 不同的配置文件对同一个配置项做配置：使用优先级高的配置
>
> 不同的配置文件对不同的配置项做配置：共存





`properties`配置文件的格式是`key=value`

```properties
# Tomcat默认端口
server.port=8080
```





`yml`配置文件的格式是分层（注意value前的一个空格）：

```yml
server:
  port: 8080 				# Tomcat默认配置（可省略）
  servlet:
    context-path: /test01	# 浏览器的url中的项目名
```

<img src="https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220320130217322.png"/>



`yaml`配置文件就是`yml`文件：

```yml
server:
  port: 8080 # Tomcat默认配置（可省略）
```







读取 Java 读取`yml`配置值的三种方式：



（方式一）在 `Java`类 中，读取 `yml `配置文件中的数据：【`注入单个配置值`】

```java
// 在yml配置文件中，
	user: 
		name: zhangsan
    
// 在Java类中，注入yml中的数据
    @Value("${user.name}")
    private String username;
```



在 `yml`中引用yml配置：

```yml
# 使用 ${属性名} 来引用配置

baseDir: c:\tmp

tmpDir: ${baseDir}\dataDir
tmpDir: "${baseDir}\data Dir"	# 转义特殊符号，需要加双引号， "data Dir"是一个目录
```





（方式二）`Java类` 批量读取 `yml`中的配置【`Environment 对象注入多个配置值`】：

```java
// Java类中：   
	@Autowired
    private Environment env;

    @RequestMapping("/")
    public String index(){
        
// 获取yml中的配置项的值
    System.out.println(env.getProperty("user"));

```









（方式三）使用Java类来封装`yml`配置：【`ConfigurationProperties注解 注入多个配置值`】：

```java
// yml 中
datasource:
  driver: com.mysql.jdbc.Driver
  url: jdbc:mysql://localhost:3306/test?serverTimezone=UTC
  username: root
  password: root
      
      
// Java类中：
      @Component
      @ConfigurationProperties("datasource")
      public class myDataSource {
          private String driver;
          private String url;
          private String username;
          private String password;
      }

// 使用
	@Autowired
    private myDataSource mydata;

	mydata.getUserName();
```







## 5、整合 Junit

SpringBoot 项目中，一般创建项目时，会导入`spring-boot-starter-test`Maven依赖，这就是整合Junit所需达到包。



在“测试启动类”中，需要使用`@SpringBootTest`注解修饰该启动类。



<font style="color:red;">注意：</font>“测试启动类”必须与“启动类”的包路径一致，否则，需要`@SpringBootTest(classes=启动类.class)`



```java
package com.cyw.ex01;

import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
class Ex01ApplicationTests {

    @Autowired
    private UserMapper userMapper;
    
    @Test
    void contextLoads() {
        userMapper.getUserById(12);
        System.out.println("SpringBoot 的Junit 测试");
    }

}

```





## 6、整合Mybatis 



注意：

- 所有SpringBoot的包，坐标都是：`spring-boot-starter`开头的。
- 所有非 SpringBoot的包，坐标都是：`xxxxx-boot-starter`结尾的。





### 6.1、导入对应的 starter



`POM.xml`：

```xml
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.2.2</version>
        </dependency>
```





### 6.2、配置 Mybatis



配置数据源



`application.yml`：

```yml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/test?serverTimezone=UTC
    username: root
    password: root
```





### 6.3、编写 mapper 接口



在mapper接口上添加`@Mapper`注解：

```java
package com.cyw.ex01.mapper;

import com.cyw.ex01.pojo.Book;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Select;


@Mapper
public interface BookMapper {
    @Select("select * from books where id = #{id}")
    public Book getBookById(Integer id);
}

```





### 6.4、配置 mapper 接口的注解扫描



在启动类上配置注解扫描：`@ComponentScan("com.cyw.ex01.mapper")`

```java
@SpringBootApplication
@ComponentScan("com.cyw.ex01.mapper")
public class Ex01Application {

    public static void main(String[] args) {
        ConfigurableApplicationContext context = SpringApplication.run(Ex01Application.class, args);
    }

}
```







### 6.5、测试代码



（1）自动装配 mapper：

```java
    @Autowired
    private BookMapper bookMapper;
```



（2）使用mapper：

```java
   @Test
    void testGetBookById() {
       bookMapper.getBookById(1);
    }

```







## 7、整合 Mybatis Plus





### 7.1、导入坐标



```xml
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.5.1</version>
        </dependency>
```



### 7.2、编写 mapper 接口



添加`@Mapper`注解，继承`BaseMapper`接口：

```java
@Mapper
public interface BookMapper extends BaseMapper<Book> {
   
}
```



### 7.3、 扫描 mapper包



给启动类添加组件扫描的注解：

```java
@ComponentScan("com.cyw.ex01.mapper")
```



### 7.4、测试代码



```java
    @Autowired
    private BookMapper bookMapper;

    @Test
    void testGetBookById() {
       bookMapper.selectById(1);
    }
```





## 8、整合 Druid



SpringBoot 自带了一个数据源 Hikari，若想换成Druid，可以进行以下配置





### 8.1、配置 starter



```xml
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.2.8</version>
        </dependency>
```





### 8.2、配置数据源



`application.yml`：

```yml
# 通用（type属性）
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/test?serverTimezone=UTC
    username: root
    password: root
    type: com.alibaba.druid.pool.DruidDataSource

# 专用（druid）
spring:
  datasource:
    druid:
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://localhost:3306/test?serverTimezone=UTC
      username: root
      password: root
```







## 9、SSMP-整合案例



效果展示：



<iframe style="height:500px;" src="//player.bilibili.com/player.html?aid=633835873&bvid=BV15b4y1a7yG&cid=430721063&page=32" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>













---





# 二、实用篇



## 1、学习目标









## 2、运维实用篇









## 3、开发实用篇





# 三、原理篇



## 1、学习目标











