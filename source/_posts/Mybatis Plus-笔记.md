



<h1>Mybatis Plus-笔记</h1>

[toc]





# 零、资料



视频：

> - [Mybatis Plus-尚硅谷【新版】](https://www.bilibili.com/video/BV12R4y157Be)
> - [Mybatis Plus-尚硅谷【旧版】](https://www.bilibili.com/video/BV1Ds411E76Y)





文档：

> - [Mybatis Plus官网](https://baomidou.com/)
> - [Mybatis Plus 开源项目地址](https://gitee.com/baomidou/mybatis-plus)





# 一、Mybatis Plus 概述



## 1、Mybatis Plus 简介



Mybatis Plus （简称 MP ），是一个Mybatis的增强工具，只做增强，不做修改，

为简化开发工作，提高效率而生。









## 2、Mybatis Plus 概览

![image-20220312165107737](https://gitee.com/cder123/note-drawing-bed-01/raw/master/image-20220312165107737.png)



## 3、Mybatis Plus 特点



![image-20220312170300239](https://gitee.com/cder123/note-drawing-bed-01/raw/master/image-20220312170300239.png)





![image-20220312170359149](https://gitee.com/cder123/note-drawing-bed-01/raw/master/image-20220312170359149.png)



# 二、入门案例



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
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.5.1</version>
        </dependency>   
    
    
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>           
        </dependency>
    

        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.2.8</version>
        </dependency>
    <!--        <dependency>-->
<!--            <groupId>org.springframework.boot</groupId>-->
<!--            <artifactId>spring-boot-starter-jdbc</artifactId>-->
<!--        </dependency>-->

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.22</version>
        </dependency>
    
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
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
    
    
#spring:
#  datasource:
#    type: com.zaxxer.hikari.HikariDataSource
#    driver-class-name: com.mysql.cj.jdbc.Driver
#    url: jdbc:mysql://localhost:3306/mybatis_plus?characterEncoding=utf-8&useSSL=false&serverTimezone=UTC
#    username: root
#    password: root
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







# 三、基本 CRUD



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
  // 按一个id删除
	@Test
    public void testDeleteById(){
        int lines = userMapper.deleteById(3L);
        System.out.println(lines);
    }

// 按条件删除
	@Test
	public void testDeleteByMap(){
        // map的键是数据表的列名，值为条件
        Map<String, Object> map = new HashMap<>();
        map.put("id",2L);
        int lines = userMapper.deleteByMap(map);
        System.out.println(lines);
    }

// 按多个id删除(批量删除)
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

// 按多个id查询    
//	SELECT id,name,age,email FROM user WHERE id IN ( ? , ? , ? )
	@Test
    public void testSelectBatchIds(){
        
        List<User> users = userMapper.selectBatchIds(Arrays.asList(1, 2, 3));
        users.forEach(System.out::println);
    }


// 按条件查询
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







# 四、自定义 Mapper 的方法



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





# 五、通用 Service



Myabtis Plus 中有一个接口 `IService`和其实现类 `ServiceImpl`，封装了常见的业务层逻辑。



官网文档：[通用Servcie](https://baomidou.com/pages/49cc81/)

> 
>
> 前缀命名方式区分 `Mapper` 层避免混淆：
>
> - `get 查询单行` 
> - `remove 删除`
> -  `list 查询集合`
> -  `page 分页` 



## 1、创建 Service 接口



- 创建一个`Service `接口，

- 继承 `IService `接口，

- 传入`实体类对象的泛型`



```java
package com.cyw.demo01.service;
import com.baomidou.mybatisplus.extension.service.IService;
import com.cyw.demo01.po.User;

public interface UserService extends IService<User> {

}

```



## 2、创建 Service 接口的实现类



- 创建 Service 接口的实现类。
- 继承 `ServiceImpl`类（传入泛型：service层要操作的`mapper`接口，要操作的`实体类`）。
- 实现刚才创建的`Service`接口。
- 使用`@Service`注解修饰该实现类。



```java
package com.cyw.demo01.service.impl;
import com.baomidou.mybatisplus.extension.service.impl.ServiceImpl;
import com.cyw.demo01.mapper.UserMapper;
import com.cyw.demo01.po.User;
import com.cyw.demo01.service.UserService;
import org.springframework.stereotype.Service;

@Service("userService")
public class UserServiceImpl extends ServiceImpl<UserMapper, User> implements UserService {
    

}

```





## 3、测试



```java
package com.cyw.demo01;

import com.cyw.demo01.service.UserService;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
public class MybatisPlusTest {
    @Autowired
    @Qualifier("userService")
    private UserService userService;

    
// 统计记录数    
    @Test
    void testCount(){        
        long count = userService.count();
        System.out.println(count);
    }
    

// 批量添加
    @Test
    void testSave(){        
        User zs = new User(null, "zs", 20L, "123@qq.com");
        User ls = new User(null, "ls", 21L, "456@qq.com");
        boolean b = userService.saveBatch(Arrays.asList(zs, ls));
        System.out.println(b);
    }
    
    
}
```





# 六、常用注解



## 1、绑定表名：@TableName



在实体类上使用`@TableName("数据表名")`注解修饰，指定该实体类绑定的数据表。

```java
@TableName("t_user")
public class User {
    private Long id;
    private String name;
    private Long age;
    private String email;
    ...
}
```



<font style="color:red;font-size:1.3em;">注意：</font>

> 当 每个数据表都有前缀（如：`t_user`中的`t_`）时，使用`@TableName`比较麻烦，所有，可以在SpringBoot的配置文件`application.yml`中，添加MybatisPlus的全局配置来统一添加前缀（代替`@TableName`注解）。



`application.yml`：

```yml
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl

  global-config:
    db-config:
      table-prefix: t_	# 设置数据表的前缀
```









## 2、绑定主键：@TableId



属性名与字段名**一致**的时候，`@TableId`注解，可以将被修饰的属性所对应的数据表中的字段作为主键。

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    @TableId
    private Long uid;
    private String name;
    private Long age;
    private String email;
}
```





属性名与字段名**不一致**的时候，`@TableId(value="字段名")`注解，手动指定映射关系

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    @TableId("id")
    private Long uid;
    private String name;
    private Long age;
    private String email;
}
```





主键不想使用雪花算法生成的ID时，先将数据表的主键设为自动递增，然后将实体类的主键用`@TableId(type=IdType.AUTO)`修饰。

```java

@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    @TableId(value="id",type= IdType.AUTO)
    private Long uid;
    private String name;
    private Long age;
    private String email;
}
```





### 2.1、设置统一的主键生成策略



在SpringBoot 的配置文件`application.yml`

```yml
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl

  global-config:
    db-config:
      table-prefix: t_
      id-type: auto		# 设置统一的主键策略（默认雪花算法，auto为主键自增）
```





### 2.2、雪花算法



数据量大的时候，需要：业务分库、主从复制，数据库分表。



业务分库、主从复制，数据库分表。



![image-20220313125426954](https://gitee.com/cder123/note-drawing-bed-01/raw/master/image-20220313125426954.png)



雪花算法：

长度共`64`bit（一个long型）。 

首先是一个`符号位`，1bit标识，由于long基本类型在Java中是带符号的，最高位是符号位，正数是0，负 数是1，所以id一般是正数，最高位是0。

` 41`bit 时间截(毫秒级)，存储的是时间截的差值（当前时间截 - 开始时间截)，结果约等于69.73年。

` 10`bit 作为机器的ID（5个bit是数据中心，5个bit的机器ID，可以部署在1024个节点）。

` 12`bit 作为毫秒内的流水号（意味着每个节点在每毫秒可以产生 4096 个 ID）。



![image-20220313125615813](https://gitee.com/cder123/note-drawing-bed-01/raw/master/image-20220313125615813.png)





## 3、绑定普通字段：@TableField



默认情况下，实体类的属性使用小驼峰法命名，数据表的字段用下划线作为分隔符命名，则可以字段建立映射关系。



但是，当实体类的属性 与 据表的字段命名完全不一致时，可以使用`@TableField("数据表的字段名")`



```java
public class User {
    @TableId(value="id",type= IdType.AUTO)
    private Long uid;
    @TableField("name")
    private String username;
    private Long age;
    private String email;
}
```







## 4、逻辑删除：@TableLogic



- <font style="color:red;">物理删除</font>：真实删除，将对应数据从数据库中删除，之后查询不到此条被删除的数据 。

- <font style="color:red;">逻辑删除</font>：假删除，将对应数据中代表是否被删除字段的状态修改为“被删除状态”，之后在数据库 中仍旧能看到此条数据记录。

- <font style="color:red;">使用场景</font>：可以进行数据恢复



### 4.1、数据表添加逻辑删除状态列



默认值设为0，若执行删除，则将给字段的值改为1。



![image-20220313130651144](https://gitee.com/cder123/note-drawing-bed-01/raw/master/image-20220313130651144.png)







### 4.2、实体类中添加逻辑删除属性



给实体类 添加 逻辑删除 属性，并使用`@TableLogic`注解修饰。

```java
public class User {
    @TableId(value="id",type= IdType.AUTO)
    private Long uid;
    @TableField("name")
    private String username;
    private Long age;
    private String email;
    
    @TableLogic
    private Integer isDel;	// 数据表的字段：is_del
}
```

指定默认值：`@TableLogic("默认值")`





### 4.3、测试删除



```java
    @Test
    void testDel(){
        boolean b = userService.removeById(1);
        System.out.println(b);
    }
```



可以发现，执行删除操作后，只是将逻辑删除的字段改为1，并不是真正的删除，这样可以进行数据恢复。



![image-20220313131609740](https://gitee.com/cder123/note-drawing-bed-01/raw/master/image-20220313131609740.png)









# 七、条件构造器（Wrapper）



条件构造器就是用来**封装** 查询和修改的**条件**的一个对象。

注意：删除用的也是`QueryWrapper`



![image-20220313183633024](https://gitee.com/cder123/note-drawing-bed-01/raw/master/image-20220313183633024.png)



![image-20220313183655337](https://gitee.com/cder123/note-drawing-bed-01/raw/master/image-20220313183655337.png)





## 1、查询条件构造器（QueryWrapper）



测试：

```java
    @Test
    void testSelect(){
        // 查询用户名包括“李”，年龄在20~30之间，邮箱不为null的用户
        QueryWrapper<User> wrapper = new QueryWrapper<>();
        
        // 此处的键为：数据库中字段的名字
        wrapper.like("name","李")
                .between("age",20,30)
                .isNotNull("email");

        List<User> users = userMapper.selectList(wrapper);
        users.forEach(System.out::println);
    }
```







## 2、排序条件构造器（QueryWrapper）



测试：

```java
    @Test
    void testSelect2(){
        // 查询用户，按age降序，uid升序
        QueryWrapper<User> wrapper = new QueryWrapper<>();

        // 此处的键为：数据库中字段的名字
        wrapper.orderByDesc("age")
                .orderByAsc("uid");

        List<User> users = userMapper.selectList(wrapper);
        users.forEach(System.out::println);
    }
```









## 3、删除条件构造器（QueryWrapper）



测试：

```java
    @Test
    void testDel(){
        // 删除用户，id中包含“1”
        QueryWrapper<User> wrapper = new QueryWrapper<>();
        wrapper.like("id",1);
        userMapper.delete(wrapper);
    }
```





## 4、修改条件构造器（UpdateWrapper）



- lt：小于，gt:大于，eq：等于，ne：不等于

- or():直接链式调用条件方法 

- and(消费者模式)：调用and(后再链式调用条件方法





测试：

```java
    @Test
    void testUpdate(){
        // 修改：(年龄大于20且用户名包含S),或(邮箱不为null)的用户
        UpdateWrapper<User> wrapper = new UpdateWrapper<>();

        // lt：小于，gt:大于，eq：等于，ne：不等于
        // and:直接链式调用条件方法 、or：调用or()后再链式调用条件方法
        wrapper.gt("age",20)
                    .like("name","S")
                    .or()
                    .isNotNull("email");
        
        User user = new User();
        user.setAge(32L);

        // 第一个参数：要修改成的值
        // 第二个参数：条件
        userMapper.update(user,wrapper);

    }
```





测试2：<font style="color:red;">（优先级）</font>

```java
@Test
    void testUpdate2(){
        // 修改用户：用户名包含S,并且（年龄大于20或邮箱为null）
        // 此处的and(...)用来lambda表达式，且lambda表达式的中的条件先执行
        UpdateWrapper<User> wrapper = new UpdateWrapper<>();
        wrapper.like("name","S")
                .and( i ->i.gt("age",20).or().isNull("email"));

        User user = new User();
        user.setUsername("小明");
        user.setEmail("123@163.com");
        userMapper.update(user,wrapper);
    }
```



