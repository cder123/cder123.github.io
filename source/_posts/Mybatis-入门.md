---
title: Mybatis-入门
tag: Java
categories:
  - [后端,Java,SSM]

---

# Mybatis-入门

[toc]



---

-   [视频：SSM框架合集-bilibili](https://www.bilibili.com/video/BV1mE411X7yp?p=9)
-   [狂神说-mybatis](https://www.bilibili.com/video/BV1NE411Q7Nx?p=2)
-   [官方中文-文档](https://mybatis.org/mybatis-3/zh/index.html)



-   mybatis有道笔记：

>   -   http://note.youdao.com/s/TiZlbvyu
>   -   http://note.youdao.com/s/582K2o08
>   -   http://note.youdao.com/s/P6lsYYPa
>   -   http://note.youdao.com/s/GStngY9I
>   -   http://note.youdao.com/s/LlyG6F9D
>   -   http://note.youdao.com/s/6itQ06sM
>   -   http://note.youdao.com/s/QqCX2pY6
>   -   http://note.youdao.com/s/TsyIKKGN
>   -   http://note.youdao.com/s/ROBlOd50
>   -   http://note.youdao.com/s/V4t6L4xg
>   -   http://note.youdao.com/s/clAdMNlv
>   -   http://note.youdao.com/s/N52FP2j9
>   -   http://note.youdao.com/s/OupQYDaB
>   -   http://note.youdao.com/s/7Cr6MA45
>   -   http://note.youdao.com/s/XXBFJgZg
>   -   http://note.youdao.com/s/9L5TytEZ





## 0、前导



1.   框架简述：开发时为解决某些问题而使用的半成品软件，是一种解决方案。

2.   使用框架的好处：封装了很多细节，提高了效率。

3.   三层架构：

     >   -   表现层：展现数据
     >   -   业务层：处理业务
     >   -   持久层：数据库交互

4.   持久层的解决方案：

     >   -   JDBC：Connection + PreparedStatement + ResultSet
     >   -   Spring的JdbcTemplete：Spring对JDBC的简单封装
     >   -   Apache的DBUtils
     >
     >   以上3种都只是工具，还算不上框架











---



## 1、Mybatis的概述



Mybatis是一款优秀的<font style="color:red;">持久层框架</font>，封装了JDBC，使得开发者只需要关注SQL本身。通过<font style="color:red;">XML 或注解</font>的形式配置Statement，通过<font style="color:red;">Java对象和Statement的动态参数</font>映射成为SQL语句。（Mybatis框架通过<font style="color:red;"> ORM</font> 解决了实体与数据库的映射问题）。



ORM 模型：（Object Relation Mapping）数据库的1张表 对应 1个Java实体类。









---



## 2、Mybatis的环境搭建



1.   IDEA 新建 Maven工程（不选模板）
2.   在 POM.xml中导入配置（前提是：已经配好maven环境）

pom.xml：【Mybatis的坐标可以直接官网复制，然后填写版本号】

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.cyw</groupId>
    <artifactId>MyBatisDemo_day01</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.5</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.0.4</version>
        </dependency>
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.12</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>3.8.2</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>
```



3.   在包下创建 <font style="color:red;">实体类（com.cyw.entity.User）</font>
4.   在包下创建 <font style="color:red;">接口(com.cyw..dao.UserDao)、在接口中定义方法（findAll）</font>
5.   在<font style="color:red;">Resource目录</font>下创建 <font style="color:red;">Mybatis的主配置文件（SqlMapConfig.xml）</font>（数据库的配置文件）



SqlMapConfig.xml：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
<!--    配置环境-->
    <environments default="mysql">
<!--        配置mysql的环境-->
        <environment id="mysql">
<!--            配置事务类型-->
            <transactionManager type="JDBC"></transactionManager>
            <dataSource type="POOLED">
<!--                配置连接数据库的4个信息-->
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatisdemo"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>
<!--    映射配置文件的位置(要用/来分割)-->
    <mappers>
        <mapper resource="com/cyw/dao/UserDao.xml"/>
    </mappers>
</configuration>
```



3.   在<font style="color:red;">Resource目录</font>下创建1个<font style="color:red;">映射配置文件</font>（与Dao接口的全类名相同的 xml文件）<font style="color:red;">( com/cyw/dao/UserDao.xml )</font>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.cyw.dao.UserDao">
<!--    配置接口的方法：查询所有用户-->
    <select id="findAll" resultType="com.cyw.entity.User">
        select * from user;
    </select>
</mapper>
```



环境配置的注意要点：

>   -   **接口的 xml 映射配置文件** 和 **接口的名字** 要保持一致。（Dao也可以叫Mapper）
>   -   IDEA中，创建**包**要用**小数点**分割，创建**目录**要用**斜杠**分割
>   -   **映射配置文件的目录结构**   要与 **接口的包结构** 保持一致。
>   -   **映射配置文件的Mapper标签的 namespace属性** 的属性值必须是**接口的全类名**
>   -   **映射配置文件的操作标签（select）的 id属性** 的属性值必须是**接口的方法名**
>
>   遵循第3~5点好处：无需写dao接口的实现类。







### 环境配置小结：

>   1.   配置maven的 pom.xml
>   2.   在 src/main/resources/ 目录中创建SqlMapConfig.xml （mybatis全局配置），填写4个DB信息
>   3.   创建实体类
>   4.   创建 dao接口
>   5.   创建 dao接口的 xml 配置文件
>   6.   在 SqlMapConfig.xml 指定 dao接口配置文件的路径
>   7.   在  dao接口的 xml 配置文件 中 指定 接口的全类名、要调用的接口方法、实体对象的全类名，sql语句。
>   8.   创建1个获取 SqlSession的工具类
>   9.   获取 SqlMapConfig.xml 的资源输入流
>   10.   在 SqlSession工具类中创建SqlSeeesionBulider对象
>   11.   SqlSeeesionBulider对象 根据 资源输入流 建造1个SqlSessionFatory对象
>   12.   SqlSessionFatory对象 调用 OpenSqlSession( ) 获取 SqlSession
>   13.   SqlSession 调用 getMapper(接口名.class) 得到 dao对象
>   14.    dao对象 调用 接口的方法得到 结果集









---



## 3、Mybatis的入门案例



案例-踩坑：【log4j:ERROR call failed】

-   原因：虚拟机里没划分D盘，只有E盘
-   解决：打开 log4j.properties 文件，log4j.appender.LOGFILE.File 的值改为 e:\axis.log

 log4j.properties 文件：

 ```properties
 # Set root category priority to INFO and its only appender to CONSOLE.
 #log4j.rootCategory=INFO, CONSOLE            debug   info   warn error fatal
 log4j.rootCategory=debug, CONSOLE, LOGFILE
 
 # Set the enterprise logger category to FATAL and its only appender to CONSOLE.
 log4j.logger.org.apache.axis.enterprise=FATAL, CONSOLE
 
 # CONSOLE is set to be a ConsoleAppender using a PatternLayout.
 log4j.appender.CONSOLE=org.apache.log4j.ConsoleAppender
 log4j.appender.CONSOLE.layout=org.apache.log4j.PatternLayout
 log4j.appender.CONSOLE.layout.ConversionPattern=%d{ISO8601} %-6r [%15.15t] %-5p %30.30c %x - %m\n
 
 # LOGFILE is set to be a File appender using a PatternLayout.
 log4j.appender.LOGFILE=org.apache.log4j.FileAppender
 
 # 注意：日志的存放位置（要选择电脑上已存在的盘）
 log4j.appender.LOGFILE.File=e:\axis.log
 log4j.appender.LOGFILE.Append=true
 log4j.appender.LOGFILE.layout=org.apache.log4j.PatternLayout
 log4j.appender.LOGFILE.layout.ConversionPattern=%d{ISO8601} %-6r [%15.15t] %-5p %30.30c %x - %m\n
 ```



-   踩坑-2：java.lang.ExceptionInInitializerError

```xml
// 在 pom.xml中加入
<build>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
        </resources>
    </build>
```









### 3.1 案例-1：

#### 0. Pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.cyw</groupId>
    <artifactId>MyBatisDemo_day01</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.5</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.0.4</version>
        </dependency>
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.12</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>3.8.2</version>
            <scope>test</scope>
        </dependency>     
    </dependencies>

</project>
```





#### 1.   SQL

```sql
create database `MybatisDemo`;

use `MybatisDemo`;

DROP TABLE IF EXISTS `user`;

CREATE TABLE `user` (
  `id` int(11) NOT NULL auto_increment,
  `username` varchar(32) NOT NULL COMMENT '用户名称',
  `birthday` datetime default NULL COMMENT '生日',
  `sex` char(1) default NULL COMMENT '性别',
  `address` varchar(256) default NULL COMMENT '地址',
  PRIMARY KEY  (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;



insert  into `user`(`id`,`username`,`birthday`,`sex`,`address`) values (41,'老王','2018-02-27 17:47:08','男','北京'),(42,'小二王','2018-03-02 15:09:37','女','北京金燕龙'),(43,'小二王','2018-03-04 11:34:34','女','北京金燕龙'),(45,'传智播客','2018-03-04 12:04:06','男','北京金燕龙'),(46,'老王','2018-03-07 17:37:26','男','北京'),(48,'小马宝莉','2018-03-08 11:44:00','女','北京修正');


```





#### 2. User 实体类

com/cyw/entity/User 实体类

```java
package com.cyw.entity;

import java.io.Serializable;
import java.util.Date;

public class User implements Serializable {
    private Integer id;
    private String username;
    private Date birthday;
    private String  sex;
    private String  address;

    public User() {
    }

    public User(Integer id, String username, Date birthday, String sex, String address) {
        this.id = id;
        this.username = username;
        this.birthday = birthday;
        this.sex = sex;
        this.address = address;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", birthday=" + birthday +
                ", sex='" + sex + '\'' +
                ", address='" + address + '\'' +
                '}';
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public Date getBirthday() {
        return birthday;
    }

    public void setBirthday(Date birthday) {
        this.birthday = birthday;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }
}

```



#### 3. UserDao接口

com/cyw/dao/UserDao接口

```java
package com.cyw.dao;

import com.cyw.entity.User;
import java.util.List;

public interface UserDao {
    /**
     * 查询所有用户
     * @return
     */
    
    // 用注解代替xml的映射配置：@Select("select * from user")
    List<User> findAll();
}

```



#### 4.  log4j.properties 配置文件

 src/main/resources/log4j.properties 

```properties
# Set root category priority to INFO and its only appender to CONSOLE.
#log4j.rootCategory=INFO, CONSOLE            debug   info   warn error fatal
log4j.rootCategory=debug, CONSOLE, LOGFILE

# Set the enterprise logger category to FATAL and its only appender to CONSOLE.
log4j.logger.org.apache.axis.enterprise=FATAL, CONSOLE

# CONSOLE is set to be a ConsoleAppender using a PatternLayout.
log4j.appender.CONSOLE=org.apache.log4j.ConsoleAppender
log4j.appender.CONSOLE.layout=org.apache.log4j.PatternLayout
log4j.appender.CONSOLE.layout.ConversionPattern=%d{ISO8601} %-6r [%15.15t] %-5p %30.30c %x - %m\n

# LOGFILE is set to be a File appender using a PatternLayout.
log4j.appender.LOGFILE=org.apache.log4j.FileAppender
log4j.appender.LOGFILE.File=e:\axis.log
log4j.appender.LOGFILE.Append=true
log4j.appender.LOGFILE.layout=org.apache.log4j.PatternLayout
log4j.appender.LOGFILE.layout.ConversionPattern=%d{ISO8601} %-6r [%15.15t] %-5p %30.30c %x - %m\n

```



#### 5. SqlMapConfig.xml

src/main/resources/SqlMapConfig.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
<!--    配置环境-->
    <environments default="mysql">
<!--        配置mysql的环境-->
        <environment id="mysql">
<!--            配置事务类型-->
            <transactionManager type="JDBC"></transactionManager>
            <dataSource type="POOLED">
<!--                配置连接数据库的4个信息-->
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatisdemo"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>
<!--    映射配置文件的位置-->
    <mappers>
        <mapper resource="com/cyw/dao/UserDao.xml"/>
        <!--    使用注解配置时，
				可以直接在下面的mapper标签中设置class属性来代替 resource属性,
				即：com.cyw.dao.UserDao
					<mapper class="com.cyw.dao.UserDao"/>
				然后在接口定义的方法上写SQL操作的注解，例如： @Select(“sql语句”)
		-->
        <mapper class="com/cyw/dao/UserDao.xml"/>
    </mappers>
</configuration>
```



#### 6. UserDao.xml

 src/main/resources/com/cyw/dao/UserDao.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">


<mapper namespace="com.cyw.dao.UserDao">
<!--    配置接口的方法：查询所有-->
    <select id="findAll" resultType="com.cyw.entity.User">
        select * from user;
    </select>
</mapper>
```





#### 7.  Main

test/java/com/cyw/test/UserDaoTest

```java
package com.cyw.test;

import com.cyw.dao.UserDao;
import com.cyw.entity.User;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.InputStream;
import java.util.List;

public class UserDaoTest {
    public static void main(String[] args)throws Exception {
        
        // 获取配置文件
        InputStream is = Resources.getResourceAsStream("SqlMapConfig.xml");
        
        // 创建SqlSessionFactoryBuilder
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        
        // 建造sqlSessionFactory
        SqlSessionFactory sqlSessionFactory = builder.build(is);
        
        // 获取会话
        SqlSession sqlSession = sqlSessionFactory.openSession();
        
        // 获取接口的代理对象（动态代理）,map就是dao
        UserDao userDao = sqlSession.getMapper(UserDao.class);
        
        // 调用方法
        List<User> users = userDao.findAll();
        
        for (User user : users) {
            System.out.println(user);
        }
        
        // 关闭资源
        sqlSession.close();
        is.close();        
        
        。
    }
}

```



#### 8. 封装工具类

```java
package com.cyw.utils;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.InputStream;

public class SqlSessionUtil {
    private static SqlSessionFactory factory;
    static {

        try {
            InputStream is = Resources.getResourceAsStream("SqlMapConfig.xml");
            SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
            factory = builder.build(is);

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    
	// 返回SqlSession
    public static SqlSession getSqlSession(){
        return factory.openSession();
    }
}

```





注意：

>   1.   Dao配置文件的**namespace的包名**要与**dao / mapper 接口名**一致。













### 3.2 解决结果集的实体类属性与数据库中的字段名不一致的问题：

2种解决方案：

-   在SQL查询时，数据库字段取 别名。
-   dao的配置文件中定义1个ResultMap，指定 id 和 type（type为结果集实体类）。 在ResultMap标签内，定义Result标签，绑定Result标签的 column属性 (数据库字段) 和 property属性 (实体类的属性) 属性。定义Select标签的ResultMap属性= ResultMap的 id 。

```xml
// resultMap中一般只指定【sql字段】与【结果集的实体类的属性】不一致的属性
// property：结果集实体类的属性
// column：数据表的列名

<resultMap id="userResultMap" type="User">    
  <id property="id" column="user_id" />
  <result property="username" column="user_name"/>
  <result property="password" column="hashed_password"/>
</resultMap>
    
    
<select id="selectUsers" resultMap="userResultMap">
  select user_id, user_name, hashed_password
  from some_table
  where id = #{id}
</select>    
```







### 3.3 配置日志

在mybatis的核心配置文件（SqlMapConfig.xml）中的Configuration标签下settings 标签中 配置日志

```xml
// 注意：setting标签下的 value属性的取值为：
// NO_LOGGING
// STDOUT_LOGGING
// LOG4J	【前3个重点掌握】
// LOG4J2
// SLF4J
// JDK_LOGGING
// COMMONS_LOGGING


<configuration>
      <settings>    
        	<setting name="logImpl" value="LOG4J"/>   
      </settings>
</configuration>
```





















---



## 4、Mybatis的单表（CRUD）



**注意：** 在接口的配置文件中编写SQL语句时，传参数可以使用占位符`#{实体类属性名}`，示例可参照案例2的第2步。





###  4.1 案例-2（CRUD）：

在案例-1的基础上实现增删改查

前提：数据库、实体类都不变。

dao接口：

```java
package com.cyw.mapper;

import com.cyw.entity.User;

import java.util.List;

public interface UserMapper {
    List<User> findAll();
    User findUserById(int id);
    int delUser(int id);
    int insertUser(User user);
    int updateUser(User user);
}

```





> 1.   查找所有用户 (MybatisTest测试类中)

配置：（接口的xml配置文件的mapper标签内）

```xml
//     id="findAll" 为接口的方法名
// 	   resultType="com.cyw.entity.User"  为结果实体类的全类名

<select id="findAll" resultType="com.cyw.entity.User">
        select * from mybatisdemo.user;
</select>
```



执行：

```java
@Test
public void findAll(){
    SqlSession sqlSession = SqlSessionUtil.getSqlSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    List<User> users = mapper.findAll();

    for (User user : users) {
        System.out.println(user);
    }

    sqlSession.close();

}
```



> 2.   查找单个用户

配置：

```xml
//     id="findUserById" 为接口的方法名
// 	   resultType="com.cyw.entity.User"  为结果实体类的全类名 
// 	   parameterType="int" 为方法调用时，所需传入的参数的类型
//	   #{传入的参数，即：实体类的属性}  为占位符

<select id="findUserById" resultType="com.cyw.entity.User" parameterType="int">
        select * from mybatisdemo.user where id=#{id};
</select>
```



执行：

```java
  @Test
    public void findUserById(){
        
        SqlSession sqlSession = SqlSessionUtil.getSqlSession();
        
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        
        // 查找 id为42的人
        User user = mapper.findUserById(42);
        
        System.out.println(user);
        
        sqlSession.close();
    }
```



> 3.   删除用户

配置：

```xml
    <delete id="delUser" parameterType="int">
        delete from  mybatisdemo.user where id=#{id};
    </delete>
```

执行：

```java
 @Test
public void delUser(){
    SqlSession sqlSession = SqlSessionUtil.getSqlSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    mapper.delUser(41);
    sqlSession.commit();
    sqlSession.close();
}
```



> 4.   更新用户

配置：

```xml
<update id="updateUser" parameterType="com.cyw.entity.User" >
        update  mybatisdemo.user set username = #{username},birthday = #{birthday},sex = #{sex},address = #{address} where id=#{id};
</update>
```



执行：

```java
 @Test
public void updateUser(){
    SqlSession sqlSession = SqlSessionUtil.getSqlSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    mapper.updateUser(new User(50,"张三",new Date(3,1,1),"男","石家庄"));
    sqlSession.commit();
    sqlSession.close();
}
```





---



### 4.2  SqlMapConfig.xml 的常用参数：



> 1.   数据库连接信息：

```xml
// 1. SqlMapConfig 中,property可以定义在外部的properties文件中，
// 	  然后通过SqlMapConfig配置文件的properties标签的resource属性引入。
// 2. 也可以直接将property写在properties标签内。
// 注意：当既有外部的properties、又有内部的properties时，优先使用外部的属性配置。

  <properties resource="com/cyw/db.properties">
        <property name="username" value="cyw"/>
        <property name="psw" value="123"/>
  </properties>

```



> 2.   结果集类取**别名**：

```xml

// 指定全类名的别名（取在在具体的类上，可以自定义别名）
    <typeAliases>
        // 给1个类取别名
        <typeAlias type="com.cyw.entity.User" alias="User"/>     
    </typeAliases>

// 指定全类名的别名（取在在具体的包上，不能自定义别名，只能是非限定类名，想自定义则必须配合注解使用）
    <typeAliases>
        // 给1个包取别名，然后在javabean的定义上使用注解：@Alias("user")
       <package name="com.cyw.entity" />
    </typeAliases>

```



> 3.   数据库的连接环境

```xml

// environments标签的 default属性 指定的是 内部environment标签的id

<environments default="mysql">
        <environment id="mysql">
            <transactionManager type="JDBC"></transactionManager>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatisdemo"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    
    	<environment id="test">
            <transactionManager type="JDBC"></transactionManager>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatisdemo"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
</environments>
```






在上面的案例中，Mybatis在使用代理dao时，只干了2件事：

>   -   创建代理对象（getMapper方法获取的dao）
>   -   利用代理对象来调用 接口的方法



小结：

-   代理对象调用接口的方法（传入参数）=》接口的定义 =》接口的 xml 配置文件





---



## 5、Mybatis 参数和返回值

























---



## 6、Mybatis的dao操作

















---



## 7、Mybatis的配置细节





















---



## 8、Mybatis连接池

















---



## 9、Mybatis事务控制

























---



## 10、Mybatis多表查询





























---



## 11、Mybatis的加载机制

































---



## 12、Mybatis的缓存

































---



## 13、Mybatis的注解开发

