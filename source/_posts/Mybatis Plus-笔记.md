---
title: Mybatis Plus-笔记
tag: Java
categories:
  - [后端,Java,SSM]

---



<h1>Mybatis Plus-笔记</h1>

[toc]





# 零、资料



视频：

> - [Mybatis Plus-尚硅谷【新版】](https://www.bilibili.com/video/BV12R4y157Be)
> - [Mybatis Plus-尚硅谷【旧版】](https://www.bilibili.com/video/BV1Ds411E76Y)





文档：

> - [Mybatis Plus官网](https://baomidou.com/)
> - [Mybatis Plus 开源项目地址](https://gitee.com/baomidou/mybatis-plus)





# 一、Mybatis Plus 简介



Mybatis Plus （简称 MP ），是一个Mybatis的增强工具，只做增强，不做修改，

为简化开发工作，提高效率而生。









# 二、Mybatis Plus 概览

![image-20220312165107737](https://gitee.com/cder123/note-drawing-bed-01/raw/master/image-20220312165107737.png)



# 三、Mybatis Plus 特点



![image-20220312170300239](https://gitee.com/cder123/note-drawing-bed-01/raw/master/image-20220312170300239.png)





![image-20220312170359149](https://gitee.com/cder123/note-drawing-bed-01/raw/master/image-20220312170359149.png)



# 四、入门案例



环境搭建：

<iframe style="height:500px;"
  src="//player.bilibili.com/player.html?aid=339472748&bvid=BV12R4y157Be&cid=700362020&page=4" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>



## 1、Maven 依赖



```xml
<dependencies>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>1.7.30</version>
        </dependency>
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-classic</artifactId>
            <version>1.2.3</version>
        </dependency>
    
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus</artifactId>
            <version>3.4.3.4</version>
        </dependency>
    
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
            <scope>test</scope>
        </dependency>
    
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.28</version>
        </dependency>
    

        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.2.8</version>
        </dependency>
    

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.22</version>
        </dependency>
    
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.3.16</version>
        </dependency>
       
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.3.15</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-tx</artifactId>
            <version>5.3.15</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aspects</artifactId>
            <version>5.3.15</version>
        </dependency>
</dependencies>

	
```





## 2、编写 SpringBoot 的配置文件



`application.yml`：

```yml
spring:
  # 配置数据源信息
  datasource:
    # 配置数据源类型：（例如：com.zaxxer.hikari.HikariDataSource）
    type: com.alibaba.druid.pool.DruidDataSource
    # 配置连接数据库信息
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/mybatis_plus?characterEncoding=utf-8&useSSL=false
    username: root
    password: root
```





## 3、扫描 Mapper 接口包



给 SpringBoot 的入口类添加注解（`@MapperScan("mapper接口包的类路径")`）：

```xml
package com.cyw.mybatisplusstudy;

import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
@MapperScan("com.cyw.mybatisplusstudy.mapper")
public class MybatisPlusStudyApplication {

    public static void main(String[] args) {
        SpringApplication.run(MybatisPlusStudyApplication.class, args);
    }
}

```





## 4、编写 Mapper 接口



创建 Mapper 接口，继承 MybatisPlus 提供的`BaseMapper`类，添加`@Repository`注解：

```java
package com.cyw.mybatisplusstudy.mapper;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.cyw.mybatisplusstudy.pojo.User;

public interface UserMapper extends BaseMapper<User> {
	// 接口方法       
        
}
```





## 5、创建 测试类



- 在 测试路径下，创建测试类`MybatisPlusTest`，

- 添加`@SpringBootTest`注解
- 自动装配`Mapper`接口
- 添加测试方法：

```java
package com.cyw.mybatisplusstudy;

import com.cyw.mybatisplusstudy.mapper.UserMapper;
import com.cyw.mybatisplusstudy.pojo.User;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import java.util.List;


@SpringBootTest
public class MybatisPlusTest {
    
    // IDEA 报错（但可以正常运行），可以在mapper接口上添加@Repository注解来解决
    @Autowired
    private UserMapper userMapper; 
    
    
    @Test
    public void testSelectList(){
        // 调用MybatisPlus的查询方法，若无查询条件,则传入null
        List<User> users = userMapper.selectList(null);

        // 用方法引用的方式打印
        users.forEach(System.out::println);

//        for (User user : users) {
//            System.out.println(user);
//        }

    }
}

```







## 6、日志



在` application.yml `配置文件中指定日志框架：

```

mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl

```



添加完日志的配置后，运行项目时，控制台可以看到 MybatisPlus 发送的SQL语句：



![image-20220312183858390](https://gitee.com/cder123/note-drawing-bed-01/raw/master/image-20220312183858390.png)





<font style="color:red;font-size:1.2em;">由 日志 的输出结果可以看出：</font>

> - MybatisPlus 发送的SQL语句的表名 就是 实体类的类名
> - MybatisPlus 发送的SQL语句的字段 就是 实体类的属性
>
> 实体类的类名和属性名要与数据库中完全一致。如果需要不一致，则可以在实体类上用`@TableName(“数据库表名”)`注解来手动建立【实体类与数据表的映射关系】







# 五、基本 CRUD



[文档地址](https://baomidou.com/pages/49cc81/#)



## 1、添加



在测试类中：

```java
    @Test
    public void testInsert(){
		//  SQL语句：INSERT INTO user ( id, name, age, email ) VALUES ( ?, ?, ?, ? )
        User user = new User(null, "张三", 20L, "123@qq.com");
        int lines = userMapper.insert(user);
        System.out.println(lines);

    }
```









## 2、删除



在测试类中：

```java
    @Test
    public void testDeleteById(){
        int lines = userMapper.deleteById(3L);
        System.out.println(lines);
    }


	@Test
	public void testDeleteByMap(){
        // map的键是数据表的列名，值为条件
        Map<String, Object> map = new HashMap<>();
        map.put("id",2L);
        int lines = userMapper.deleteByMap(map);
        System.out.println(lines);
    }

// 按多个id删除
	@Test
	public void testDeleteByBatchIds(){
        // 删除id为3、4、5的记录
        int lines = userMapper.deleteBatchIds(Arrays.asList(3,4,5));
        System.out.println(lines);
    }
```









## 3、修改



在测试类中：

```java
  	@Test
    public void testUpdateById(){
        
        User user = new User(1L,"李四",20L,"123@qq.com");
        
        int lines = userMapper.updateById(user);
        
        System.out.println(lines);
        
    }
```









## 4、查询



`selectList(Wrapper)`方法的参数是条件构造器，具体用法见后面的笔记。



在测试类中：

```java
// 按一个id查询    
	@Test
    public void testSelectById(){
        
        User user = userMapper.selectById(1L);
        System.out.println(user);
    }

// 按多个个id查询    
//	SELECT id,name,age,email FROM user WHERE id IN ( ? , ? , ? )
	@Test
    public void testSelectBatchIds(){
        
        List<User> users = userMapper.selectBatchIds(Arrays.asList(1, 2, 3));
        users.forEach(System.out::println);
    }


// 按条件map查询
    @Test
    public void testSelectByMap(){
        HashMap<String, Object> map = new HashMap<>();
        map.put("name","李四");
        List<User> users = userMapper.selectByMap(map);
        users.forEach(System.out::println);
    }


// 查询所有
 	@Test
    public void testSelectList(){
        // 调用MybatisPlus的查询方法，若无查询条件,则传入null
        List<User> users = userMapper.selectList(null);
        
        // 用方法引用的方式打印
        users.forEach(System.out::println);
    }

```







# 六、自定义 Mapper 的方法



Mybatis Plus 只做增强，不做改变，因此，如果 MyabtisPlus 默认提供的方法不能满足需求，则可以跟之前一样，自己定义Mapper接口的方法及其实现。





## 1、设置 配置文件（可选）



在SpringBoot 的`application.yml`配置文件中：

```
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
  mapper-locations: classpath*:/mapper/**/*.xml # 默认的路径
```



## 2、创建 接口的方法



```java
package com.cyw.mybatisplusstudy.mapper;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.cyw.mybatisplusstudy.pojo.User;
import org.springframework.stereotype.Repository;

import java.util.Map;

@Repository
public interface UserMapper extends BaseMapper<User> {
    Map<String,Object> selectUserById(Long id);
}

```







## 3、创建 mapper 配置文件



在 `resource`目录下，创建`mapper`目录（即：`resource/mapper/`）。

在`mapper`目录下，创建 mapper 接口的配置文件：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.cyw.mybatisplusstudy.mapper.UserMapper">

    <select id="selectUserById"
            resultType="map"
            parameterType="long">
        select * from user where id=#{id}
    </select>
    
</mapper>
```





## 4、创建 测试方法



在测试类中：

```xml
    @Test
    public void testSelectUserById(){
        Map<String, Object> map = userMapper.selectUserById(1L);
        System.out.println(map);
    }
```

