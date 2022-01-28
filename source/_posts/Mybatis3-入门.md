---
title: Mybatis3-入门
tag: Java
categories:
  - [后端,Java,SSM]

---

# Mybatis3-入门

[toc]



## 0、资料



视频：

>   -   [尚硅谷MyBatis实战教程](https://www.bilibili.com/video/BV1mW411M737?p=1)



书：

>   -   SSM从零开始学



博客：

>   -   [MyBatis · 语雀 (yuque.com)](https://www.yuque.com/u21744893/miusbc)
>   -   [一级缓存-详解](https://www.cnblogs.com/cxuanBlog/p/11324034.html)
>   -   [二级缓存-详解](https://www.cnblogs.com/cxuanBlog/p/11333021.html)



---



## 1、Mybatis 概述

Mybaits3 也就是 ibatis3。Mybatis是一个持久层的框架。

<font style="color:red">为什么使用 Mybatis ?</font>

>   -   Mybatis 是半自动的持久层框架
>   -   JDBC硬编码，耦合度高
>   -   Hibernate 和 JPA，全自动框架，对复杂的SQL语句也不好处理



<font style="color:red"> Mybatis 官网地址：</font>[MyBatis 3 -官方文档-中文](https://mybatis.org/mybatis-3/zh/index.html)

<font style="color:red"> Mybatis 下载地址：</font>https://github.com/mybatis/mybatis-3/releases



原生的接口式编程：Dao =》 DaoImpl

Mybatis口式编程：   Dao =》 dao接口的XML映射配置文件



<font style="color:red"> 注意：</font>

>   -   配置文件的提示：XML中引入DTD约束
>   -   SqlSession使用完后必须**手动关闭**
>   -   SqlSession 和 Connection 一样，都是 **非线程安全**的，每次使用都应该重新获取对象
>   -   Mybatis不用写Dao的实现类，Mybatis会根据**XML映射配置文件**生成**代理对象**
>   -   SqlSession.getMapper(UserDao.class) 得到的是**代理对象**



---



### 1.1、 初次使用Mybatis-案例

<font style="color:red"> 实体类-User：</font>

```java
package com.beans;
// 以下省略构造函数getter/setter、toString的代码
public class User {
    // 属性名应该尽量与数据库中的一致，如过不一致，则需要在SQL中手动取别名取成一致
	private int uid;
	private String username;
	private String pwd;
	private int money;
}
```

<font style="color:red">Dao接口-UserDao：</font>

```java
package com.dao;

import org.apache.ibatis.annotations.Select;

import com.beans.User;

public interface UserDao {
	// @Select("select * from user where uid = #{uid}")
	public User getUser(int uid);
}

```

<font style="color:red">全局配置文件：app.xml</font>

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    // 配置数据库的信息
      <environments default="development">
            <environment id="development">
              <transactionManager type="JDBC"/>
              <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/springstudy"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
              </dataSource>
            </environment>
      </environments>
    
     // 注册Dao接口的映射配置文件
      <mappers>
        	<mapper resource="com/conf/UserMapper.xml"/>
      </mappers>
</configuration>
```



<font style="color:red">Dao接口的映射配置文件【注解版】：UserMapper</font>

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

// namespace: 表示Dao接口的全类名
// mapper 标签内，如果使用注解来执行SQL，则不用写SQL语句的标签；否则，写
<mapper namespace="com.dao.UserDao">
	
</mapper>
```



<font style="color:red">Dao接口的映射配置文件【XML版】：UserMapper</font>

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

// namespace: 表示Dao接口的全类名
// mapper 标签内，如果使用注解来执行SQL，则不用写SQL语句的标签；否则，写
// 	select标签中，
//		- id ：方法名，
// 		- resultType ：返回值类型
//		- #{} ：形参，SQL语句的占位符
<mapper namespace="com.dao.UserDao">
  <select id="getUser" resultType="com.beans.User">
    select * from user where uid = #{uid}
  </select>
</mapper>
```



<font style="color:red">测试：</font>

```java
import java.io.*;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.*;
import com.beans.User;
import com.dao.UserDao;

public class Test {
	public static void main(String[] args) {
		try {
			InputStream in = Resources.getResourceAsStream("app.xml");
			SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(in);
			SqlSession session = sqlSessionFactory.openSession();
			UserDao dao = session.getMapper(UserDao.class);
			User user = dao.getUser(1);
            
            // User [uid=1, username=张三, pwd=123, money=1000]
			System.out.println(user);           
           
		} catch (IOException e) {		
			e.printStackTrace();
		}finally{
            // 关闭session
            session.close();
        }
	}
}
```



### 1.2、Maven-POM配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.cyw</groupId>
    <artifactId>SSM-01</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.26</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.9</version>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.16.14</version>
        </dependency>
    </dependencies>


    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <configuration>
                    <testFailureIgnore>true</testFailureIgnore>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
```



## 2、Mybatis 全局配置文件

Mybatis 全局XML配置文件是为了注册信息【也可以通过new配置对象的方法】，包括：

-   环境信息【数据库、事务管理器】
-   dao接口的映射配置文件的信息

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    
    // default属性：指定当前使用哪种环境
      <environments default="development">
            <environment id="development">
              <transactionManager type="JDBC"/>
              <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/springstudy"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
              </dataSource>
            </environment>
      </environments>
    
     // 指定Dao接口的映射配置文件的路径【必选】
      <mappers>
          <mapper resource="com/conf/UserMapper.xml"/>
          
          // 或者直接在DAO接口上使用注解写SQL，然后再全局配置中注册接口
          // <mapper class="com.dao.UserDao"/>
      </mappers>
    
</configuration>
```



---



### 2.1、 properties标签

Mybatis可以使用`properties标签`来引入**外部的properties文件**中的配置【一般是数据库配置信息】。

一般配置文件由Spring来导入，所以Mybatis的数据库配置不常用

`properties标签`的两个属性：

>   -   resource：引入**类路径**下的配置
>   -   url：引入**网络路径**下的配置

<font style="color:red;font-size:1.2em">例如：</font>

外部properties配置文件-db.properties：

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/springstudy
jdbc.username=root
jdbc.password=root
```



全局配置文件：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    
  <properties resource="com/conf/db.properties"></properties>
  <!-- <properties url="域名"></properties> -->
    
  // 配置信息  
  <environments default="development">
    <environment id="development">
      <transactionManager type="JDBC"/>
      <dataSource type="POOLED">
        <property name="driver" value="${jdbc.Driver}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
      </dataSource>
    </environment>
  </environments>
  

  <mappers>
   <!--  <mapper resource="com/conf/UserMapper.xml"/> -->
   <mapper class="com.dao.UserDao"/>
  </mappers>
</configuration>
```



---



### 2.2、数据表的属性名由下划线转驼峰



在Mybatis的全局配置文件中，使用`mapUnderscoreToCamelCase`，该配置是在 `settings 标签`的`setting子标签`中设置的。【 **settings 标签** 中只能放 **setting 标签**】

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>  
    
      // 全局设置 
      <settings>
            // 开启：自动将数据库的属性名由 下划线 转 驼峰
            <setting name="mapUnderscoreToCamelCase" value="true"/>
      </settings>  
	
      // 配置信息  
      <environments default="development">
        <environment id="development">
          <transactionManager type="JDBC"/>
          <dataSource type="POOLED">
            <property name="driver" value="${jdbc.Driver}"/>
            <property name="url" value="${jdbc.url}"/>
            <property name="username" value="${jdbc.username}"/>
            <property name="password" value="${jdbc.password}"/>
          </dataSource>
        </environment>
      </environments>
  
      // dao接口的映射配置文件
      <mappers>
       <!--  <mapper resource="com/conf/UserMapper.xml"/> -->
       <mapper class="com.dao.UserDao"/>
      </mappers>    

</configuration>
```



---



### 2.3、配置全类名的别名



<font style="color:red;">注意：</font>

>   -   别名不区分大小写
>   -   Mybatis内置了一部分别名【基本数据类型、包装类、集合类型、日期类型】





`typeAliases`标签可以用来设置多个类的别名。



<font style="color:red;font-size:1.2em">给单个类取别名（1）：</font>

`<typeAlias alias="别名" type="全类名"/>`是一个别名处理器，可以给全类名设置别名，方便程序员写全类名.

`typeAliases标签`的`alias属性`默认就是`类名的小写`。



<font style="color:red;font-size:1.2em">给单个类取别名（2）：</font>

在dao接口上（或者某个需要设置别名的类上）使用注解（`@Alias("别名")`）。

此方式可以在**批量取别名**的方式出现**别名冲突时**使用。



<font style="color:red;font-size:1.2em">给一个包中的全部类取别名（批量取别名）：</font>

`<package name="包名"/>`可以给**当前包以及子包**的全部类取一个**默认别名**（默认别名格式：类名小写）



```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration> 
    
    // 给全类名取别名  
    <typeAliases>
        <!-- 
			<typeAlias alias="user" type="com.entity.User"/>
			等价于 
            <typeAlias type="com.entity.User"/>
  		-->
         <typeAlias type="com.entity.User"/>
         <typeAlias alias="Book" type="com.entity.Book"/>  
        
        <!-- 批量取别名 -->
         <package name="包名" />
    </typeAliases>    
    
      // 全局设置 
      <settings>
            // 开启：自动将数据库的属性名由 下划线 转 驼峰
            <setting name="mapUnderscoreToCamelCase" value="true"/>
      </settings>  
	
      // 配置信息  
      <environments default="development">
        <environment id="development">
          <transactionManager type="JDBC"/>
          <dataSource type="POOLED">
            <property name="driver" value="${jdbc.Driver}"/>
            <property name="url" value="${jdbc.url}"/>
            <property name="username" value="${jdbc.username}"/>
            <property name="password" value="${jdbc.password}"/>
          </dataSource>
        </environment>
      </environments>
  
      // dao接口的映射配置文件
      <mappers>
       <!--  <mapper resource="com/conf/UserMapper.xml"/> -->
       <mapper class="com.dao.UserDao"/>
      </mappers>    

</configuration>
```



---



### 2.4、类型处理器

`typeHandlers`的作用：建立**Java类型**与**数据库类型**的桥梁。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration> 
    
    <typeHandlers>
 		<package name="com.cyw.Example"/>
	</typeHandlers>
    
    
    // 给全类名取别名  
    <typeAliases>       
         <typeAlias type="com.entity.User"/>
         <typeAlias alias="Book" type="com.entity.Book"/>          
        <!-- 批量取别名 -->
         <package name="包名" />
    </typeAliases>    
    
      // 全局设置 
      <settings>
            // 开启：自动将数据库的属性名由 下划线 转 驼峰
            <setting name="mapUnderscoreToCamelCase" value="true"/>
      </settings>  
	
      // 配置信息  
      <environments default="development">
        <environment id="development">
          <transactionManager type="JDBC"/>
          <dataSource type="POOLED">
            <property name="driver" value="${jdbc.Driver}"/>
            <property name="url" value="${jdbc.url}"/>
            <property name="username" value="${jdbc.username}"/>
            <property name="password" value="${jdbc.password}"/>
          </dataSource>
        </environment>
      </environments>
  
      // dao接口的映射配置文件
      <mappers>
       <!--  <mapper resource="com/conf/UserMapper.xml"/> -->
       <mapper class="com.dao.UserDao"/>
      </mappers>    

</configuration>
```



---



### 2.4、插件简介

插件可以拦截以下四大对象的一些方法。

四个对象：

>   -   StatementHandler
>   -   ParamterHandler
>   -   ResultSetlHandler
>   -   Executor



---



### 2.5、多个环境的配置

`environments`标签，可以包括多个`environment`标签，并利用`id属性`指定一个默认使用的环境，每个环境必须要配置**事务管理器**和**数据源**。

一般使用Spring来做事务控制，所以Mybatis的事务管理器**仅做了解**。



`transactionManager`的type属性的取值：【`jdbc` | `managed` | 自定义的事务管理器】

`dataSource`的type属性的取值：【`pooled` | `unpooled` | `jndi` | 自定义的数据源】

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    
    // 多个环境
    <environments default="development">
        
        <environment id="development">
          <transactionManager type="JDBC"/>
          <dataSource type="POOLED">
            <property name="driver" value="com.mysql.jdbc.Driver"/>
            <property name="url" value="jdbc:mysql://localhost:3306/springstudy"/>
            <property name="username" value="root"/>
            <property name="password" value="root"/>
          </dataSource>
        </environment>
        
        <environment id="test">
          <transactionManager type="JDBC"/>
          <dataSource type="POOLED">
            <property name="driver" value="com.mysql.jdbc.Driver"/>
            <property name="url" value="jdbc:mysql://localhost:3306/springstudy"/>
            <property name="username" value="root"/>
            <property name="password" value="root"/>
          </dataSource>
        </environment>
   </environments>  
 
  <mappers>
   <!--  <mapper resource="com/conf/UserMapper.xml"/> -->
   <mapper class="com.dao.UserDao"/>
  </mappers>
</configuration>
```



---



### 2.6、支持不同厂商的数据库

`databaseIdProvider`标签，可以让 Mybatis 支持不同厂商的数据库。

步骤：

>   -   在`全局配置文件`中，配置`databaseIdProvider`标签
>   -   在dao接口的`映射配置文件`中，SQL语句标签的`databaseId属性`设为全局配置文件中厂商property标签的`value属性的值`

全局配置文件：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    
    // 数据库厂商
    <databaseIdProvider type="DB_VENDER">
    	<property name="MySQL" value="mysql" />
        <property name="Oracle" value="oracle" />
        <property name="SQL Server" value="sqlserver" />
    </databaseIdProvider>
    
    // 环境
    <environments default="development">        
        <environment id="development">
          <transactionManager type="JDBC"/>
          <dataSource type="POOLED">
            <property name="driver" value="com.mysql.jdbc.Driver"/>
            <property name="url" value="jdbc:mysql://localhost:3306/springstudy"/>
            <property name="username" value="root"/>
            <property name="password" value="root"/>
          </dataSource>
        </environment>      
   </environments>  
 
  <mappers>
   <!--  <mapper resource="com/conf/UserMapper.xml"/> -->
   <mapper class="com.dao.UserDao"/>
  </mappers>
</configuration>
```

映射配置文件：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.dao.UserDao">
      <select id="getUser1" resultType="com.beans.User" 
              databaseId="mysql">
        select * from user where uid = #{uid}
      </select>
    
       <select id="getUser1" resultType="com.beans.User" 
              databaseId="sqlserver">
        select * from user_tb where uid = #{uid}
      </select>
</mapper>
```





---



## 3、Mybatis 映射配置文件

Mybatis 映射配置文件【必须要有，除非使用注解方式且已在mapper标签中注册接口】



<font  style="color:red;">映射配置文件中，方法的返回值：</font>

Mybatis的**增删改**方法要想由返回值可直接在方法的返回值，无需像查询一样单独定义返回值属性。



<font  style="color:red;">映射配置文件中，方法传参：</font>

方法传参可以直接使用全类名，SQL语句中的参数占位符可以使用`#{JavaBean中的属性名}`

```xml
<select id="getUserByUid" resultType="com.entity.User" >
    select * from user where uid = #{uid}
</select> 
```



<font  style="color:red;">映射配置文件中，主键自增：</font>

MySQL支持自增主键，Mybatis利用了原生JDBC中的Statement对象的statement.getGeneratedKeys()方法，

制作了一个属性`useGeneratedKeys`，默认值为false（关闭主键自增策略），配合`keyProperty`属性使用（指定Mybatis获取自增主键后，赋值给JavaBean的哪个属性）

UserMapper.xml：

```xml
<insert id="addUser" parameterType="com.entity.User"  
  	useGeneratedKeys="true" keyProperty="uid">
  	insert into user values(#{uid},#{username},#{pwd},#{money});
</insert>
```



Oracle不支持自增主键，则使用序列来模拟自增，每次插入的数据的主键是从序列中拿到的值，insert标签内配合`selectKey`标签。



### 3.1、增删改查-案例

bean：

```java
package com.entity;
public class User {
	private int uid;
	private String username;
	private String pwd;
	private int money;
}
```

mybatis-conf.xml：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <environments default="development">
    <environment id="development">
      <transactionManager type="JDBC"/>
      <dataSource type="POOLED">
        <property name="driver" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/springstudy"/>
        <property name="username" value="root"/>
        <property name="password" value="root"/>
      </dataSource>
    </environment>
  </environments>

  <mappers>
    <mapper resource="com/conf/UserMapper.xml"/>  
  </mappers>
</configuration>
```

UserMapper.xml：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.dao.UserDao">
     // 查询
      <select id="getUserByUid" resultType="com.entity.User" >
        select * from user where uid = #{uid}
      </select> 
    // 插入
      <insert id="addUser" parameterType="com.entity.User">
        insert into user values(#{uid},#{username},#{pwd},#{money});
      </insert>
    // 更新
      <update id="updateUser" parameterType="com.entity.User">
        update user 
            set username=#{username},pwd=#{pwd},money=#{money} 
            where uid=#{uid}  		
      </update>
    // 删除
      <delete id="delUser">
        delete from user where uid=#{uid}
      </delete>  
</mapper>
```



### 3.2、方法参数的处理

>   单个参数：

`#{}`，不会做任何处理，直接传值（参数名随便写都一样）

例如：`public User getUser(int uid)`



>   多个参数：【待定，测试时无法使用】

会做处理，多个参数会被封装为一个Map集合，`#{}`就是取Map中指定的Key的值。

在封装`参数Map`时，key就是 `param1`到` paramn`（或参数的索引号），value才是用户真正传入的参数值。

`public User getUser(int uid,String username)`



>   命名参数：

 需要明确封装参数Map的Key，即：在接口的方法的参数前使用注解`@Param("参数Map的键名")`

```java
package com.dao;
import org.apache.ibatis.annotations.Param;
import com.entity.User;

public interface UserDao {
	public User getUserByUid(int uid);
	public User getUser(@Param("uid") int uid,@Param("username") String username);
}

```

>   参数为Collcetion集合或数组的处理：

参数为Collcetion集合或数组时，即使只有一个参数，也会将参数封装为Map。

参数Map的Key键为：

-   Collection：collection
-   List：list
-   数组：array

```xml
#{list[0]}
#{collection[0]}
#{array[0]}
```





<font  style="color:red;">小结1（应用场景）：</font>

但参数时，参数命名无所谓。

如果传入的参数很多，且**是业务数据模型**，可以用传入**JavaBean对象**的方式来代替。

如果传入的参数很多，但**不是业务数据模型**，不经常使用，可以用传入**Map集合**的方式来代替。

如果传入的参数很多，但**不是业务数据模型**，经常使用，可以用传入**TO数据传输对象**的方式来代替（如：分页对象）。



<font  style="color:red;">小结2（应用方式）：</font>

当参数过多时，为了不混乱，可以在dao接口的参数前加上`@Param("map参数名")`来封装使用的参数map的key，使用`#{key名}`就可以取出参数map中指定的key所对应的value。





<font  style="color:red;">`${}`与`#{}`来取值的区别：</font>

-    `#{}`：采用预编译的形式，将参数设置到SQL语句中，类似JDBC中的 preparedStatement
-   `${}`：直接取值，类似JDBC中的Statement

大多数情况下应该使用`#{}`，但在**原生JDBC不支持占位符的地方**可以使用`${}`（如：sql语句中的表名、排序的字段名、排序顺序）





<font  style="color:red;">`#{}`取值方式的丰富的规则：</font>

规定了参数的一些规则：

-   javaType、jdbcType、mode（存储过程）、numericScale、resultMap、typeHandler、jdbcTypeName、expression
-   javaType通常在数据为null的情况下被设置【有些数据库，如Oracle无法识别mybatis对null值的默认处理】

```xml
// 在映射配置文件中的SQL语句处
#{email,jdbcType=NULL}
```



---



### 3.3、resultMap 标签（关系映射）



**resultMap标签的作用**：定义结果集的封装规则。





<font style="color:red;font-size:1.2em;">步骤：</font>

1、先在映射配置文件中利用 `resultMap` 标签自定义JavaBean的封装规则，`resultMap` 标签的`type`属性用来绑定某个已知的JavaBean的全类名，`id`属性作为唯一标识。

2、`resultMap` 标签指定主键（标签内）`<id column="数据表的列名" property="javabean的属性名"/>` ，`<result column="数据表的列名" property="javabean的属性名"/>` 指定数据表的普通属性的封装，如果没有手动指定result 子标签，则自动封装。

3、`association、collection`两个子标签用于指定关联属性（类型为类的属性），`property`的值为JavaBean的属性名，`javaType`的值为类型的全类名（`association 子标签`），`select`属性的值为另一个Dao接口中的方法的全类名，`column`属性的值为数据表中将值传给方法的列，`ofType`属性的值为集合中元素的类型的全类名（`collection 子标签`）。



<font style="color:red;font-size:1.2em;">关系映射：</font>

-   一对一（`association 子标签`）：【类A有一个类B的属性】类A的映射配置文件中，association子标签的select属性绑定类B的Dao接口的查询方法，该查询方法的返回值为一个对象。（两个类的关联属性是一个**2**类型）
-   一对多（`collection 子标签`）：【类A有一个类B的属性，且是集合类型】类A的映射配置文件中，association子标签的select属性绑定类B的Dao接口的查询方法，该查询方法的返回值为一个集合。（两个类的关联属性是一个**集合**类型）
-   多对多：【类A有一个类B的属性，且是集合类型，类B有一个类A的属性，且是集合类型】类A的映射配置文件中，association子标签的select属性绑定类B的Dao接口的查询方法，该查询方法的返回值为一个集合。类B的映射配置文件中，association子标签的select属性绑定类A的Dao接口的查询方法，该查询方法的返回值为一个集合。（两个类各自有一个关联属性，且各自是一个**集合**类型）



<font style="color:red;">一对一_示例：:</font>

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.dao.UserDao">
	
    // 自定义JavaBean类型【嵌套查询】
    // id子标签：用于定义主键的映射关系
    // result子标签：用于定义其他属性的映射关系
	<resultMap type="com.entity.User" id="myUser">
        // column : 数据表的列名
        // property：javabean的属性名
        // javaType: javabean的全类名（javaBean属性的类型）
        // fetch-Type: 懒加载【lazy（默认） | eager】
            <id column="uid" property="uid" />
            <result column="username" property="username" />
            <result column="pwd" property="pwd" />
            <result column="money" property="money" />
            <association property="dept" javaType="com.entity.Dept">
                <id column="deptId" property="deptId" />
                <result column="deptName" property="deptName" />
            </association>
	</resultMap>
    
     // 自定义JavaBean类型【分步查询】
	<resultMap type="com.entity.User" id="myUser">
		<id column="uid" property="uid" />
		<result column="username" property="username" />
		<result column="pwd" property="pwd" />
		<result column="money" property="money" />
        // select: “查询”方法的全类名
        // column: 数据表的列名
        // property: select属性的查询结果交给property指定的javaBean属性
            <association property="dept" 
                         select="com.entity.DeptDao.getDeptByDeptId"
                         column="dept"
                         >        	
            </association>
	</resultMap>
	
	<select id="getUserByUid1" resultMap="myUser">
	 	select * from user where uid = #{uid}
	</select>
  
</mapper>
```

<font style="color:red;">一对多_示例：:</font>

```xml
	<resultMap type="com.entity.User" id="myUser">
        // column : 数据表的列名
        // property：javabean的属性名
        // javaType: javabean的全类名（javaBean属性的类型）
        // fetch-Type: 懒加载【lazy（默认） | eager】
            <id column="uid" property="uid" />
            <result column="username" property="username" />
            <result column="pwd" property="pwd" />
            <result column="money" property="money" />
            <collection property="deptList" ofType="com.entity.Dept">
                <id column="deptId" property="deptId" />
                <result column="deptName" property="deptName" />
            </collection>
	</resultMap>
```











### 3.4、select标签

#### 1、返回值类型（resultType属性）

<font  style="color:red;">一般情况下：</font>

使用 别名 或 全类名。



<font  style="color:red;">返回 List 时：</font>

如果方法的返回值为` List `集合，则` select`标签的 `resultType`类型应该写`List集合中元素的类型`

```xml
// dao接口中的方法：	
	public List getUserLikeName(@Param("username") String username);

// UserMapper.xml
	  <select id="getUserLikeName" resultType="com.entity.User">
  		select * from user where username like #{username}
  	  </select>

// 测试
	List<User></User> users = dao.getUserLikeName("%李%");				
	System.out.println(users);
```



<font  style="color:red;">返回Map 时：</font>

如果方法的返回值为` Map `集合，则` select`标签的 `resultType`类型应该写`map`

```xml
// dao接口中的方法：【一条记录封装为map】	
	public Map getUserByUid(@Param("uid") int uid);

// UserMapper.xml
	  <select id="getUserByUid" resultType="map">
  		select * from user where uid = #{uid}
  	  </select>

// 测试
	Map user = dao.getUserByUid(1);				
	System.out.println(user);

===================================================

// dao接口中的方法：【多条记录封装为map】
	// 告诉Mybatis使用哪个属性作为主键
	@MapKey("uid") 
	public Map<Integer,User> getUserLike(@Param("username") String username);

// UserMapper.xml
	  <select id="getUserLike" resultType="map">
  		select * from user where username like #{uid}
  	  </select>

// 测试
	Map user = dao.getUserLike("%李%");
    System.out.println(users);
```



#### 2、返回值类型（resultMap属性）



引用 `resultMap 标签`所定义的结果集映射规则，具体见上面的`3.3小节`的 ResultMap 标签。



---

### 3.5、insert、update、delete标签



用法与select标签类型，见上面的select标签的笔记和官方文档的示例：

-   [Mybatis-中文文档-insert、update、delete](https://mybatis.org/mybatis-3/zh/sqlmap-xml.html#insert_update_and_delete)





### 3.6、SQL标签

`sql 标签`可以定义一些SQL语句中可以复用的一部分内容，然后在`select、insert、update、delte`标签中利用`<include refid=""></include>`标签和`property`标签来**引用**该SQL标签。



映射配置文件中：

```xml

// ${alias}是占位符，在select语句中，通过property标签传入具体的值
	<sql id="userColumns"> ${alias}.id,${alias}.username,${alias}.password </sql>

// 查询
    <select id="selectUsers" resultType="map">
          select
                <include refid="userColumns">
                    <property name="alias" value="t1"/>
                </include>
                <include refid="userColumns">
                    <property name="alias" value="t2"/>
                </include>
          from some_table t1
            	cross join some_table t2
    </select>
```



### 3.7、懒加载

懒加载就是在类A中，有一个类B的属性，当Mybatis查询类A时，不会自动查询类B，只有手动调用查询类B的方法时，才查询类B【即：延迟加载】。

Mybatis`默认关闭了懒加载`。



<font style="color:red;font-size:1.2em;">如何启用懒加载：</font>

在mybatis的**全局配置文件**中的`settings`标签中，编写配置。

```xml
<settings>
        <setting name="lazyLoadingEnabled" value="flase"/>
</settings>
```

| 加载方式       | lazyLoadingEnabled       | aggressiveLazyLoading                                 |
| -------------- | ------------------------ | ----------------------------------------------------- |
| 直接加载       | 必须是false，默认是false | 不管是什么，只要lazyLoadingEnabled是false就是直接加载 |
| 侵入式延迟加载 | 必须是true               | 必须是true                                            |
| 深度延迟       | 必须是true               | 必须是false，默认是false                              |







---



## 4、Mybatis 动态SQL

动态SQL：解决传统SQL语句的拼接繁琐的问题。



传统的SQL拼接时，where子句后面有多个多个条件那么就需要在一开始写`where 1=1`，然后开始拼接。使用动态SQL标签后，则可以不用写`where 1=1`。



<font style="color:red;font-size:1.2em;">动态SQL的标签：</font>

>   以下标签的使用方式类似JSP中的JSTL标签库的用法：
>
>   -   `if`
>   -   `choose` (`when`, `otherwise`)
>   -   `trim` (`where`【SQL中的where子句】, `set`【update操作时的set子句】)
>   -   `foreach`



###  4.1、<font style="color:red;">if 标签：</font>

>   语法：` <if test="拼接的条件"> 拼接的SQL语句  </if>`
>
>   -   test 属性 中可以使用`and`、`or` 等逻辑关键字组合多个条件
>   -   test 属性 中的字符串的判断：`abc !=null and abc != ''`
>   -   test 属性 中的数字的判断【需要使用转义字符】：`abc &lt;= 2 ` （大于等于）、`abc &gt;= 2 ` （小于等于）

if 标签的示例（1）：

```xml
<select id="findActiveBlogLike"
     resultType="Blog">
          select * from BLOG 
    	  where state = ‘ACTIVE’
              <if test="title != null">
                AND title like #{title}
              </if>
    
              <if test="author != null and author.name != null">
                AND author_name like #{author.name}
              </if>
</select>
```



if 标签的示例（2）-【可能因条件不成立而报错】：

```xml
<select id="findActiveBlogLike"
     resultType="Blog">

    // 条件不成立时：SELECT * FROM BLOG WHERE
          SELECT * FROM BLOG
          WHERE
              <if test="state != null">
                state = #{state}
              </if>
              <if test="title != null">
                AND title like #{title}
              </if>
              <if test="author != null and author.name != null">
                AND author_name like #{author.name}
              </if>
</select>
```



if 标签的示例（2）-【改进】：

```xml
<select id="findActiveBlogLike"
     resultType="Blog">
          SELECT * FROM BLOG
          <where>
                <if test="state != null">
                     state = #{state}
                </if>              
                <if test="title != null and title ！= ‘’ ">
                    AND title like #{title}
                </if>              
                <if test="author != null and author.name != null">
                    AND author_name like #{author.name}
                </if>
          </where>
</select>
```





### 4.2、<font style="color:red;">choose-when-otherwise 标签：</font>

```xml
<select id="findActiveBlogLike"
     resultType="Blog">
          SELECT * FROM BLOG 
    	  WHERE state = ‘ACTIVE’
          <choose>
                <when test="title != null">
                  AND title like #{title}
                </when>
              
                <when test="author != null and author.name != null">
                  AND author_name like #{author.name}
                </when>
              
                <otherwise>
                  AND featured = 1
                </otherwise>
          </choose>
</select>
```



### 4.3、<font style="color:red;">where 标签：</font>

>   注意：该标签解决了可能因条件全部不成立而导致的 SQL语句问题

```xml
<select id="findActiveBlogLike"
     resultType="Blog">
          SELECT * FROM BLOG
          <where>
                <if test="state != null">
                     state = #{state}
                </if>              
                <if test="title != null">
                    AND title like #{title}
                </if>              
                <if test="author != null and author.name != null">
                    AND author_name like #{author.name}
                </if>
          </where>
</select>
```



### 4.4、<font style="color:red;">trim标签：</font>

>   trim 标签可以定制SQL语句，类似于where标签

```xml
// 从where字符开始，如果标签内部的条件不成立，则去掉最终的SQL语句中无效的 and 或 or 字符    
	<trim prefix="where" prefixOverrides="and | or">
      <if test="state != null">
              <if test="state != null">
                   state = #{state}
              </if>              
              <if test="title != null">
                   AND title like #{title}
              </if>              
              <if test="author != null and author.name != null">
                    AND author_name like #{author.name}
              </if>
    </trim>
```

### 4.5、set 标签

set 标签是用于 update 标签内部的，其作用类似与 update 语句的 set 子句。

```xml
<update id="updateAuthorIfNecessary">
      update Author
            <set>
                  <if test="username != null">username=#{username},</if>
                  <if test="password != null">password=#{password},</if>
                  <if test="email != null">email=#{email},</if>
                  <if test="bio != null">bio=#{bio}</if>
            </set>
      where id=#{id}
</update>
```



### 4.6、foreach 标签

foreach 标签，用于遍历某个数组、集合。

常用的属性：

>   -   item：当前元素
>   -   index：当前元素的**迭代次数**、**map集合的key**
>   -   collcection：被迭代的数组、集合
>   -   open：数组、集合的**开始分隔符**
>   -   separator：数组、集合的**中间分隔符**
>   -   close：数组、集合的**闭合分隔符**

```xml
<select id="selectPostIn" resultType="domain.blog.Post">
      SELECT *
      FROM POST P
          <where>
                <foreach item="item" index="index" collection="list"
                    	 open="ID in (" separator="," close=")" 
                         nullable="true">
                      #{item}
                </foreach>
          </where>
</select>
```



### 4.7、bind 标签



bind 标签：用于在 `select 标签内部 `设置**模糊查询**的条件。



使用 bind 标签的原因：不同的数据库使用**模糊查询的语法不同**。



使用步骤：

>   -   用于在 `select 标签内部 ` 拼接模糊查询的条件。
>   -   在SQL语句中使用



```xml
<select id="selectBlogsLike" resultType="Blog">
  // title为参数变量名，也可替换成 _parameter.getTitle()
  <bind name="pattern" value="'%' + title + '%'" />    
    
  SELECT *
    FROM BLOG
  	WHERE 
    	title LIKE #{pattern}
</select>
```





----



## 5、Mybatis 缓存

-   [官方文档-缓存](https://mybatis.org/mybatis-3/zh/sqlmap-xml.html#cache)

-   [一级缓存-详解](https://www.cnblogs.com/cxuanBlog/p/11324034.html)
-   [二级缓存-详解](https://www.cnblogs.com/cxuanBlog/p/11333021.html)



Mybatis 中的SQL语句执行后会有缓存，再次执行相同的语句时，无需编译SQL语句，直接执行缓存中的SQL。



Mybatis 缓存包括了**一级缓存**和**二级缓存**，*默认开启了**一级缓存***。

>   -   **一级缓存**：是` SqlSession`级别【查询缓存】的，每次进行**增删改**操作时，一级缓存失效。
>   -   **二级缓存**：是` Mapper(dao)`级别【表级缓存】的，多个**一级缓存**可以共享同一个**二级缓存** ，二级缓存是事务性的。

![缓存-1](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220128125844044.png)



手动清理一级缓存：`sqlSession.clearCache();`

Mybatis的缓存在底层使用 `Map`封装。





**二级缓存**默认是不开启的，需要手动开启二级缓存。



<font style="color:red;">二级缓存的使用步骤:</font>

>   -   JavaBean 可序列化
>   -   MyBatis 的**全局配置文件**中设置`setting`标签

实现二级缓存的时候，MyBatis 要求返回的**POJO必须是可序列化**的。开启二级缓存的条件也是比较简单，通过直接在 MyBatis 的**全局配置文件**中通过：

```xml
<settings>
	<setting name = "cacheEnabled" value = "true" />
</settings>
```

然后，dao接口的**映射配置文件**中通过`<cache />`标签：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.dao.DeptDao">
    
        // eviction：清除策略，
    	// 		- LRU（默认）：按使用频率移除，优先删除不常用的缓存
    	//		- FIFO ：按缓存产生时的顺序来删除
    	//		- SOFT ：按 GC的状态 和 软引用规则 来删除
    	//		- WEAK ：按 GC的状态 和 弱引用规则 来删除
    	// flushInterval： 缓存的刷新时间的间隔
    	// size：缓存区的大小【“引用”的个数】，默认1024
        <cache
               eviction="FIFO"
               flushInterval="60000"
               size="512"
               readOnly="true"
        />
</mapper>
```







## 6、Mybatis 整合



<font style="color:red;font-size:1.3em;">步骤：</font>

>   1、编写：数据库（数据源）的配置：`druid.properties`

```properties
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/ssm_01
username=root
pwd=root
initialSize=5
maxActive=100
maxWait=5000
```





>   2、编写：Spring 的全局配置文件：`applicationContext.xml`

<font style="color:red;">2-1、引入命名空间和约束：</font>

-   beans

-   context

-   aop

-   tx

`applicationContext.xml`：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd
        http://www.springframework.org/schema/tx
        http://www.springframework.org/schema/tx/spring-tx.xsd
">
    
    
</beans>
```



<font style="color:red;">2-2、在读取`applicationContext.xml`中的 数据库配置文件：</font>

```xml
	
	<context:property-placeholder location="classpath:/conf/druid-conf.properties" />

```





<font style="color:red;">2-3、在读取`applicationContext.xml`中的 开启组件扫件、AOP代理、注解配置：</font>

```xml
   
	<context:component-scan base-package="com.cyw.*"/>
    <aop:aspectj-autoproxy/>
    <context:annotation-config />

```



<font style="color:red;">2-4、在读取`applicationContext.xml`中的 开启组件扫件、AOP代理、注解配置：</font>





>   3、编写：Mybatis 的全局配置文件：`mybatis-conf.xml`
>
>   















## 7、Mybatis 逆向工程



<font style="color:red;">Mybatis 逆向工程是什么？</font>

Mybatis 逆向工程（MyBatis Generator）也叫 MBG，可以通过**逆向工程的配置文件**自动生成 POJO（JavaBean）、Dao、Dao的映射配置文件。



<font style="color:red;">使用步骤：</font>

1、maven 项目在 `pom.xml `文件中添加以下代码：

```xml
<dependency>
    <groupId>org.mybatis.generator</groupId>
    <artifactId>mybatis-generator-core</artifactId>
    <version>1.4.0</version>
</dependency>
```

2、在项目中创建MGB的配置文件（名字随便起，此处命名为`genertorConfig.xml `）:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN" "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <context id="DB2Tables" targetRuntime="MyBatis3">
        <commentGenerator>
            <!-- 是否去除自动生成的注释 -->
            <property name="suppressAllComments" value="true" />
        </commentGenerator>
        <!-- Mysql数据库连接的信息：驱动类、连接地址、用户名、密码 -->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
            connectionURL="jdbc:mysql://localhost:3306/test" userId="root"
            password="root" />

        <!-- 默认为false，把JDBC DECIMAL 和NUMERIC类型解析为Integer，为true时 把JDBC DECIMAL 和NUMERIC类型解析为java.math.BigDecimal -->
        <javaTypeResolver>
            <property name="forceBigDecimals" value="false" />
        </javaTypeResolver>

        <!-- targetProject：生成POJO类的位置 -->
        <javaModelGenerator
            targetPackage="net.biancheng.pojo" targetProject=".\src">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false" />
            <!-- 从数据库返回的值被清理前后的空格 -->
            <property name="trimStrings" value="true" />
        </javaModelGenerator>

        <!-- targetProject：mapper映射文件生成的位置 -->
        <sqlMapGenerator targetPackage="net.biancheng.mapper"
            targetProject=".\src">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false" />
        </sqlMapGenerator>

        <!-- targetProject：mapper接口生成的的位置 -->
        <javaClientGenerator type="XMLMAPPER"
            targetPackage="net.biancheng.mapper" targetProject=".\src">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false" />
        </javaClientGenerator>

        <!-- 指定数据表 -->
        <table tableName="website"></table>
        <table tableName="student"></table>
        <table tableName="studentcard"></table>
        <table tableName="user"></table>
    </context>

</generatorConfiguration>
```

3、创建执行MBG 的类，来自动生成 POJO、Dao接口、Dao的映射配置文件：

```xml
package net.biancheng;

import java.io.File;
import java.util.*;
import org.mybatis.generator.api.MyBatisGenerator;
import org.mybatis.generator.config.Configuration;
import org.mybatis.generator.config.xml.ConfigurationParser;
import org.mybatis.generator.internal.DefaultShellCallback;

public class GeneratorSqlmap {
    public void generator() throws Exception {
		// 
        List<String> warnings = new ArrayList<String>();
        boolean overwrite = true;
            
        // 指定配置文件
        File configFile = new File("./config/generatorConfig.xml");
            
        // 创建解析器    
        ConfigurationParser cp = new ConfigurationParser(warnings);
        
        // 读取配置    
        Configuration config = cp.parseConfiguration(configFile);
        
        // 
        DefaultShellCallback callback = new DefaultShellCallback(overwrite);
        
        // 创建MGB生成器    
        MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config, callback, warnings);
            
        // 生成    
        myBatisGenerator.generate(null);
            
    }

    // 执行main方法以生成代码
    public static void main(String[] args) {
        try {
            GeneratorSqlmap generatorSqlmap = new GeneratorSqlmap();
            generatorSqlmap.generator();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```





## 8、Mybatis 运行原理







## 9、Mybatis 插件









## 10、Mybatis 扩展