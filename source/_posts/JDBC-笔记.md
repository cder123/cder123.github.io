---
title: JDBC-笔记
tag: JDBC
categories:
  - [后端,Java,JDBC]
  - [SQL,MySQL]
---

# JDBC-笔记

[toc]

## 0. 工具下载

-   MySQL-下载:https://wws.lanzoui.com/izU3xpr459a 密码:96kc
-   Navicat-下载:https://wws.lanzoui.com/ihKFypr448d 密码:amqz
-   JDBC常用Jar包-下载:https://wws.lanzoui.com/ihKFypr448d 密码:amqz





## 1. JDBC-理解



-   [JDBC-bilibili](https://www.bilibili.com/video/BV1Bt41137iB?p=2)





1.  什么是JDBC？

>   JDBC：【Java DataBase Connectivity】
>
>   位置：`java.sql.*;`



2.  JDBC的本质：

>   JDBC是Sun公司提供的一套接口，
>
>   接口都有 调用者 和 实现者，
>
>   我们面向接口，去`调用、写实现类`,这些都是面向接口的编程。



3.  为什么要面向接口编程？

>   -   解耦合
>   -   多态机制



4.  为什么要制定JDBC接口？

>   因为每种数据库软件在底层的实现原理是不一样的。
>
>   为了适配不同的数据库，Sun公司提供了JDBC接口，每种数据库的厂家根据自家的数据库，写JDBC的实现类【一堆` .class文件`】，最后将这些实现类打包（`.jar`），形成`数据库驱动`。
>
>   **注意：** 数据库驱动是由各大数据库的厂商提供的，因此应该去厂商官网下载。







## 2. 导入驱动包

驱动：`mysql-connector-java-5.0.4-bin.jar`



![导入驱动jar包-1](https://z3.ax1x.com/2021/06/01/2mxDQU.png)



![jdbc-导入jar包-2](https://z3.ax1x.com/2021/06/01/2mxgoR.png)

![jdbc-导入jar包-3](https://z3.ax1x.com/2021/06/01/2mx4SK.png)





## 3. JDBC的使用步骤



-   [狂神说Java-JDBC](https://www.bilibili.com/video/BV1NJ411J79W?p=38)





步骤：

1.  注册驱动【需先下载，将文件后缀改为 `.jar`】：告诉Java要用什么牌子的数据库

>   -   新建1个`lib`文件夹，放入`数据库驱动jar包`
>   -   file -> projectStructure -> lib -> “+” -> 选中 数据库的驱动 -> apply
>   -   【如果没成功添加驱动，可以点击file -> projectStructure -> module -> 选择要应用到的模块 ->dependencies -> “+”号 -> 选择 驱动jar包】
>   -   利用反射  找到  驱动中的Driver类

```java
// mysql 5.5版本，【注意：8.0版本的包名有变化】
	Class.forName("com.mysql.jdbc.Driver");	
```



2.  获取连接：JVM和数据库的通道打开，属于进程间的通信，使用完后，一定要手动关闭。

>   -   传入url、用户名、密码

```java
// 获取连接，传入url、用户名、密码
//String url = jdbc:mysql://localhost:3306/School_1?serverTimezone=Asia/Shanghai
//					&useUnicode=true
//  				&characterEncoding=utf8
//      			&useSSL=true
        
	String url = "jdbc:mysql://localhost:3306/myDB?参数"	// mysql8.0需要在参数中传入时区
        
    String userName = "javaUser";

	String psw = "javaUser";
	

	Connection conn = DriverManager.getConnection(url,userName,psw);
```



3.  获取数据操作的对象：

```java

Statement statement = conn.createStatement();

```





4.  执行SQL语句

```java
        ResultSet resultSet = statement.executeQuery(sql); // 查询

        // statement.execute(sql);	// 所有sql

        // int lines = statement.executeUpdate(sql);	// 执行增、删、改，返回执行的行数

        // int[] counts = statement.executeBatch(sql);	// 执行批处理
```



5.  获取、处理 结果集

```java
   while(resultSet.next()){
// 结果集中的数据的获取       
           resultSet.getObject(列名或索引);
       
       	   // resultSet.getInt(列名或索引);
       
       	   // resultSet.getString(列名或索引);
       
       	   // resultSet.getDouble(列名或索引);
       
       	   // resultSet.getDate(列名或索引);
  
       
// 指针移动：
       		resultSet.next();
       		// resultSet.previous();
       		// resultSet.beforeFirst();
       		// resultSet.afterLast();
	       	// resultSet.absolute(row);	// 移动到指定行
   }
```



6.  释放资源

```java
	resultSet.close();
 	statement.close();
	conn.close();
```







### 2.2 案例-1：

```java
package cn.jabc.demo1;
import java.sql.*;
public class Demo {
    public static void main(String[] args){

        try {

            // 数据库的用户名；密码
                String user = "javaUser";
                String psw = "javaUser";

            // 数据库的url：jdbc:mysql://ip地址:端口号/数据库名?时区
                String url = "jdbc:mysql://127.0.0.1:3306/School_1?serverTimezone=Asia/Shanghai";

            // 加载驱动
                Class.forName("com.mysql.jdbc.Driver");

            // 获取连接【conn连接 就代表1个数据库】
                Connection conn = DriverManager.getConnection(url,user,psw);

                System.out.println("=== 数据已连接");

            // 创建数据操作对象
                Statement statement = conn.createStatement();

            // 创建SQL语句
                String sql = "select * from stu;";

            // 执行SQL,获取结果集
                ResultSet resultSet = statement.executeQuery(sql); // 查询
                statement.execute(sql);	// 所有sql
            	int lines = statement.executeUpdate(sql);	// 执行增、删、改，返回执行的行数
                int[] counts = statement.executeBatch(sql);	// 执行批处理

            // 输出结果
                while(resultSet.next()){
                    System.out.print("sid:"+resultSet.getObject("sid"));
                    System.out.println(" , sname:"+resultSet.getObject("sname"));
                }

            // 关闭资源
                resultSet.close();
                statement.close();
                conn.close();
                System.out.println("=== 连接已关闭");


        } catch(Exception e){
            e.printStackTrace();
        }

    }
}

```





### 2.3 封装JDBC的的工具类【初始版】



1.  编写工具类：【DBUtil】

```java
package cn.jdbcUtil;
import java.sql.*;

public class DBUtil {
    private static String url = "jdbc:mysql://localhost:3306/School_1";
    private static String user = "javaUser";
    private static String psw= "javaUser";


    static {
        try {
            Class.forName("com.mysql.jdbc.Driver");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }


    public static Connection getConn() throws SQLException {
        return DriverManager.getConnection(url,user,psw);
    }

    public static void relaeseConn(Connection conn, Statement st,ResultSet rs){

        try {
            if(rs!=null) rs.close();
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }
        try {
            if(st!=null) st.close();
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }
        try {
            if(conn!=null) conn.close();
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }

    }
}

```

2.  使用：

```java
package cn.jdbcdemo;

import cn.jdbcUtil.DBUtil;

import java.sql.*;


public class Demo {
    public static void main(String[] args) throws Exception{

        try {
            Connection conn = DBUtil.getConn();
            Statement statement = conn.createStatement();
            ResultSet res = statement.executeQuery("select * from stu;");
            while(res.next()){
                System.out.println(res.getString("sid")+" "+res.getString("sname"));
            }

            DBUtil.relaeseConn(conn,statement,res);

        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }


    }
}

```





### 2.4 封装JDBC的的工具类【改进版】

1.  编写：配置文件【`db.properties`】

注意：`配置文件必须是src目录的直接孩子。【不能是孙子】`

```java
driver = com.mysql.jdbc.Driver
url = jdbc:mysql://localhost:3306/School_1?serverTimezone=Asia/Shanghai&useUnicode=true&characterEncoding=utf8&useSSL=true
user = javaUser
psw = javaUser
```



2.  编写工具类：【DBUtil_2】

```java
package cn.jdbcUtil;
import java.io.InputStream;
import java.sql.*;
import java.util.Properties;

public class DBUtil_2 {
    private static String driver = null;
    private static String url = null;
    private static String user = null;
    private static String psw= null;

// 获取配置文件
    static {
        try {
            InputStream is = DBUtil_2.class.getClassLoader().getResourceAsStream("db.properties");
            
            Properties properties = new Properties();
            properties.load(is);
            
// 获取配置文件中的4个配置
            driver = properties.getProperty("driver");
            url = properties.getProperty("url");
            user = properties.getProperty("user");
            psw = properties.getProperty("psw");
 
// 注册驱动
            Class.forName(driver);

       
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

// 获取连接
    public static Connection getConn() throws SQLException {
        return DriverManager.getConnection(url,user,psw);
    }
 
    
// 释放资源
    public static void relaeseConn(Connection conn, Statement st,ResultSet rs){

        try {
            if(rs!=null) rs.close();
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }
        try {
            if(st!=null) st.close();
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }
        try {
            if(conn!=null) conn.close();
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }

    }
}

```



### 2.5 封装JDBC的工具类【最终版】

将`Statement `改为`PreparedStatement`, 更安全。可以防止用户输入SQL语句后，输入的语句 参与到 后端的SQL语句的编译，导致被SQL注入的问题。`PreparedStatement`将用户输入的语句作为字符串，传入后端的SQL语句中进行SQL语句的拼接。

```java
package cn.jdbcUtil;
import java.io.InputStream;
import java.sql.*;
import java.util.Properties;

public class DBUtil_3 {
    private static String driver = null;
    private static String url = null;
    private static String user = null;
    private static String psw= null;


    static {
        try {
            InputStream is = DBUtil_3.class.getClassLoader().getResourceAsStream("db.properties");
            Properties properties = new Properties();
            properties.load(is);

            driver = properties.getProperty("driver");
            url = properties.getProperty("url");
            user = properties.getProperty("user");
            psw = properties.getProperty("psw");

            Class.forName(driver);


        } catch (Exception e) {
            e.printStackTrace();
        }
    }


    public static Connection getConn() throws SQLException {
        return DriverManager.getConnection(url,user,psw);
    }

    public static void relaeseConn(Connection conn, PreparedStatement ps,ResultSet rs){

        try {
            if(rs!=null) rs.close();
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }
        try {
            if(ps!=null) ps.close();
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }
        try {
            if(conn!=null) conn.close();
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }

    }
}

```





### 2.6 案例1-改进版：

```java
package cn.jdbcdemo;

import cn.jdbcUtil.DBUtil;
import cn.jdbcUtil.DBUtil_2;

import java.sql.*;


public class Demo {
    public static void main(String[] args) throws Exception{


        try {
            //Connection conn = DBUtil.getConn();
            Connection conn = DBUtil_2.getConn();
            
            // Statement statement = conn.createStatement();
            
            // 要执行的SQL语句
            String sql = "select * from stu where sid = ? ;";
            
            
        	// 预编译SQL语句，?问号为占位符，
            // 占位符的内容在执行时，只作为字符串，不会被当作SQL的一部分参与编译
            // 采用预编译，可以防止：SQL注入
            PreparedStatement ps = conn.prepareStatement(sql);
            
            // 设置占位符的内容，此处表示将第一个占位符的值设为3
            // 【注意】：JDBC中的下标 从 1 开始计数
            ps.setInt(1,3);

            ResultSet res = ps.executeQuery();
            while(res.next()){
                System.out.println(res.getString("sid")+" "+res.getString("sname"));
            }

            DBUtil.relaeseConn(conn,ps,res);

        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }


    }
}


```



### 2.7 案例2-登录功能:

注意：使用了`2.5小节中的 DBUtil_3工具类`

```java
package cn.jdbcdemo;

import cn.jdbcUtil.DBUtil_3;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.Scanner;

public class UserLogin {
    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        System.out.print("请输入用户名：");
        String username = sc.nextLine();


        System.out.print("请输入密码：");
        String upsw = sc.nextLine();

        Connection conn=null;
        PreparedStatement ps=null;
        ResultSet res = null;

        if(username!=null && upsw!=null){

            try {
                conn = DBUtil_3.getConn();
                String sql = "select * from stu where sname=? and spsw=?";
                ps = conn.prepareStatement(sql);
                ps.setString(1,username);
                ps.setString(2,upsw);

                res = ps.executeQuery();

                if(res.next()){
                    System.out.println("登录成功");
                }else {
                    System.out.println("登录失败");
                }

            } catch (SQLException throwables) {

                throwables.printStackTrace();
            }finally {

                try {
                    DBUtil_3.relaeseConn(conn,ps,res);
                } catch (Exception e) {
                    e.printStackTrace();
                }
                sc.close();
            }

        }

    }
}

```







## 4. IDEA-连接MySQL



-   选择：数据库的品牌

![idea-连MySQL](https://z3.ax1x.com/2021/06/01/2nuNGT.png)







-   在IDEA中，登录到MySQL
    -   注意：url在填写时，需要加上`时区`

![idea-连接myql](https://z3.ax1x.com/2021/06/01/2nK51U.png)







-   选择具体要使用的数据库

![idea-连接mysql-3](https://z3.ax1x.com/2021/06/01/2nM4UI.png)









查看数据库的内容：双击数据库

修改数据库：双击单元格，修改数据，回车，点击提交

![idea中可视化修改数据](https://z3.ax1x.com/2021/06/01/2nlzcT.png)







![idea中直接使用SQL语句](https://z3.ax1x.com/2021/06/01/2n1RrF.png)







## 5. JDBC事务-操作



```java
package cn.jdbcdemo;

import cn.jdbcUtil.DBUtil_3;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class Demo02 {
    public static void main(String[] args) {
        Connection conn = null;
        PreparedStatement ps = null;
        ResultSet rs = null;

        try {
// 获取连接
            conn = DBUtil_3.getConn();

// 关闭自动提交 =》开启事务
            conn.setAutoCommit(false);

// 执行 2个事务
            String sql1 = "update account_tb set money = money-500 where userID=2";
            ps = conn.prepareStatement(sql1);
            ps.executeUpdate();

            String sql2 = "update account_tb set money = money+500 where userID=1";
            ps = conn.prepareStatement(sql2);
            ps.executeUpdate();
// 提交事务
            conn.commit();
            
// 重新开启事务的自动提交            
            conn.setAutoCommit(true);

        } catch (SQLException throwables) {
            try {
// 回滚事务                
                conn.rollback();
            } catch (SQLException e) {
                e.printStackTrace();
            }
            throwables.printStackTrace();
        }finally {
            DBUtil_3.relaeseConn(conn,ps,rs = null);
        }


    }
}

```









## 6. 数据库连接池

-   [狂神说-bilibili](https://www.bilibili.com/video/BV1NJ411J79W?p=45)
-   [DBCP连接池所用到的两个jar包下载地址-CSDN博客](https://blog.csdn.net/ximexi/article/details/112609042)

-   [c3p0-jar包-下载CSDN博客](https://blog.csdn.net/huangyuhua068/article/details/82086259)













数据库连接过程：

-   连接 -> 执行 -> 释放  【这3个过程，浪费系统资源，开销大】



池化技术：

>   提前准备好一些连接资源（如：conn对象，preparedStatement对象，ResultSet等），用户用完后，不释放资源，而是将资源重新放回池中。
>
>   **小结**：`数据库连接池`就是存放资源的`容器`。



-   最小连接数：设为常用的连接数，如：10
-   最大连接数：如：15
-   等待超时：如：10 ms





使用数据库连接池的好处：

>   -   节约资源
>   -   高效访问





编写连接池：实现 DataSource接口【获取连接】

>   开源数据源 实现:
>
>   -   DBCP:
>   -   C3P0：
>   -   Druid：阿里

使用以上的数据库连接池后，无需 再编写 数据库的连接代码。





`数据库连接池`实现了：`javax.sql.DataSource`接口

-   获取连接：`getConnection()`
-   释放连接：连接池中的conn调用`conn.close()`，归还连接



### 6.1 DBCP:

需要导入的包

-   `commons-dbcp-1.4.jar`
-   `commons-pool-1.6.jar`



步骤：

-   编写`DBCP.properties` 配置文件,放在模块`src目录下的第一层`
-   导入Jar包：
    -   `commons-dbcp-1.4.jar`
    -   `commons-pool-1.6.jar`
    -   `mysql-connector-java-5.0.4-bin.jar`
-   编写工具类【在工具类中创建数据源，返回连接】
-   获取连接，执行语句，处理数据，释放资源。





DBCP-properties配置：

```java
#连接设置
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/School_1?serverTimezone=Asia/Shanghai&useUnicode=true&characterEncoding=utf8&useSSL=true
username=root
password=root

#!-- 初始化连接 --
initialSize=10

#最大连接数量
maxActive=50

#!-- 最大空闲连接 --
maxIdle=20

#!-- 最小空闲连接 --
minIdle=5

#!-- 超时等待时间以毫秒为单位 6000毫秒/1000等于60秒 --
maxWait=60000
#JDBC驱动建立连接时附带的连接属性属性的格式必须为这样：【属性名=property;】
#注意：user 与 password 两个属性会被明确地传递，因此这里不需要包含他们。
connectionProperties=useUnicode=true;characterEncoding=UTF8

#指定由连接池所创建的连接的自动提交（auto-commit）状态。
defaultAutoCommit=true

#driver default 指定由连接池所创建的连接的只读（read-only）状态。
#如果没有设置该值，则“setReadOnly”方法将不被调用。（某些驱动并不支持只读模式，如：Informix）
defaultReadOnly=

#driver default 指定由连接池所创建的连接的事务级别（TransactionIsolation）。
#可用值为下列之一：（详情可见javadoc。）NONE,READ_UNCOMMITTED, READ_COMMITTED, REPEATABLE_READ, SERIALIZABLE
defaultTransactionIsolation=READ_UNCOMMITTED
```





DBCPUtil.class

```java
package cn.DBCP_demo;


import org.apache.commons.dbcp.BasicDataSourceFactory;
import javax.sql.DataSource;
import java.io.InputStream;
import java.sql.*;
import java.util.Properties;

public class DBCPUtil {


    private static DataSource dataSource = null;

    static {
        try {
            InputStream is = DBCPUtil.class.getClassLoader().getResourceAsStream("dbcpConfig.properties");
            Properties properties = new Properties();
            properties.load(is);

// 创建数据源
            dataSource = BasicDataSourceFactory.createDataSource(properties);


        } catch (Exception e) {
            e.printStackTrace();
        }
    }


    public static Connection getConn() throws SQLException {
// 从数据源获取连接
        return dataSource.getConnection();
    }

    public static void relaeseConn(Connection conn, PreparedStatement ps, ResultSet rs) {

        try {
            if (rs != null) rs.close();
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }
        try {
            if (ps != null) ps.close();
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }
        try {
            if (conn != null) conn.close();
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }

    }
}


```





演示：

```java
package cn.DBCP_demo;


import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class Demo {

    public static void main(String[] args) {
        try {
// 利用工具类获取连接
            Connection conn = DBCPUtil.getConn();


            String sql = "select * from stu where sid = ? ;";


            PreparedStatement ps = conn.prepareStatement(sql);


            ps.setInt(1,3);

            ResultSet res = ps.executeQuery();
            while(res.next()){
                System.out.println(res.getString("sid")+" "+res.getString("sname"));
            }

            DBCPUtil.releaseConn(conn,ps,res);

        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }


    }
}

```



### 6.2 c3p0：



使用`XML`作为`配置文件`的格式



需要导入的Jar包：

-   `mchange-commons-java.jar`
-   `c3p0.jar`





使用的方式【2种】：

-   硬编码：直接在代码中设置参数
-   配置文件【配置文件名不可改】: `c3p0.properties` 或`c3p0-config.xml`



`配置文件`应该存放在`src目录`下。





C3P0的使用步骤：

>   -   导入 JAR包：`mchange-commons-java.jar`+`c3p0.jar`
>   -   定义配置文件：`src目录`下`c3p0-config.xml`
>   -   创建数据库连接池：`CombopooledDadtaSource`对象
>   -   获取连接：`comboPooledDadtaSource.getConnection();`





`c3p0-config.xml`配置文件：【c3p0自动寻找项目中的本地配置文件】

```xml
<?xml version="1.0" encoding="utf-8"?>
<c3p0-config>
  <default-config>
    <property name="driverClass">com.mysql.jdbc.Driver</property>
    <property name="jdbcUrl">jdbc:mysql://localhost:3306/School_1</property>
    <property name="user">javaUser</property>
    <property name="password">javaUser</property>
    
    <property name="initialPoolSize">5</property>
    <property name="maxPoolSize">10</property>
    <property name="checkoutTimeout">3000</property>
  </default-config>

</c3p0-config>
```





java工具类：

```java
package cn.c3p0Demo;


import com.mchange.v2.c3p0.ComboPooledDataSource;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class C3P0Util {


    private static DataSource dataSource = null;

    static {
        try {
// 创建数据库连接池
            dataSource = new ComboPooledDataSource();


        } catch (Exception e) {
            e.printStackTrace();
        }
    }


    public static Connection getConn() throws SQLException {
        // 从数据源获取连接
        return dataSource.getConnection();
    }

    public static void releaseConn(Connection conn, PreparedStatement ps, ResultSet rs) {

        try {
            if (rs != null) rs.close();
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }
        try {
            if (ps != null) ps.close();
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }
        try {
            if (conn != null) conn.close();
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }

    }
}

```



演示：

```java
package cn.c3p0Demo;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class Demo {
    public static void main(String[] args) {
        try {
            Connection conn = C3P0Util.getConn();

            String sql = "select * from stu where sid = ? ;";
            PreparedStatement ps = conn.prepareStatement(sql);


            ps.setInt(1,3);

            ResultSet res = ps.executeQuery();
            while(res.next()){
                System.out.println(res.getString("sid")+" "+res.getString("sname"));
            }

            C3P0Util.relaeseConn(conn,ps,res);

        } catch (SQLException throwables) {
            throwables.printStackTrace();
        } finally {
        }
    }
}

```







### 6.3 Druid：

Druid是阿里巴巴出的一款数据库连接池。



使用步骤：

>   -   导入JAR 包：`druid-1.0.9.jar`
>   -   导入配置文件：
>       -   `properties`格式的配置文件
>       -   可以放在任意目录下
>       -   配置文件名无要求
>   -   通过工厂来获取数据库连接对象`Conn`：`DruidDataSourceFactory`
>   -   执行数据库相关操作







`Druid.properties`配置文件：

```java
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql://127.0.0.1:3306/School_1
username=javaUser
password=javaUser
initialSize=5
maxActive=10
maxWait=3000
```



Druid工具类：

```java
package cn.DruidDemo;

import com.alibaba.druid.pool.DruidDataSourceFactory;

import javax.sql.DataSource;
import java.io.IOException;
import java.io.InputStream;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.Properties;

public class DruidUtil {

    private static DataSource dataSource = null;
// 重点代码
    static {
        InputStream is = DruidUtil.class.getClassLoader().getResourceAsStream("druid.properties");
        
        Properties properties = new Properties();

        try {
            properties.load(is);
// 创建连接池            
            dataSource = DruidDataSourceFactory.createDataSource(properties);


        } catch (Exception e) {
            e.printStackTrace();
        }

    }
    
// 返回 静态的数据库连接池    
   public static DataSource getDataSource(){
       return dataSource;
   }

// 获取连接
    public  static Connection getConnection()throws Exception{
        return dataSource.getConnection();
    }

    
// 归还资源    
    public static void releaseConn(Connection conn, PreparedStatement ps, ResultSet rs){

        try {
            if(conn!=null){
                conn.close();
            }
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }

        try {
            if(ps!=null){
                ps.close();
            }
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }

        try {
            if(rs!=null){
                rs.close();
            }
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }


    }

}

```



main方法：

```java
package cn.DruidDemo;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

public class DruidDemo {
    public static void main(String[] args) {

        Connection conn = null;
        PreparedStatement ps =null;
        ResultSet rs = null;

        try {
// 获取连接            
            conn = DruidUtil.getConnection();
            String sql = "select * from stu where sid = ?;";
            ps = conn.prepareStatement(sql);
            ps.setInt(1,3);
            rs = ps.executeQuery();

            if(rs.next())
            System.out.println(rs.getString("sname"));

        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            DruidUtil.releaseConn(conn,ps,rs);
        }
    }
    
}

```













### 6.4 JDBC Templete



Spring 框架对 JDBC的简单封装：`JdbcTemplete对象`。

【以下示例中，使用了`Druid示例`中的`获取DataSource的方法`：`DruidUtil.getDataSource();`】

步骤：

>   -   导入JAR包
>       -   `commons-logging-1.2.jar`
>       -   `spring-beans-5.0.0.RELEASE.jar`
>       -   `spring-core-5.0.0.RELEASE.jar`
>       -   `spring-jdbc-5.0.0.RELEASE.jar`
>       -   `spring-tx-5.0.0.RELEASE.jar`
>   -   通过` DataSource` 对象 来创建`JdbcTemplete `对象：
>       -   `JdbcTemplete jt = new JdbcTemplete(dataSource);`
>   -   调用` JdbcTemplete对象的方法`来进行 增、删、改、查。
>       -   `update(sql,占位符对应的参数)`：增、删、改
>       -   `queryForMap(sql,占位符对应的参数)`：查询的`结果集`封装为 `Map集合`，1条记录
>       -   `queryForList(sql)`：查询的`结果集`封装为 `List集合`，多条记录
>       -   `query(sql,BeanPropertyRowMapper)`：查询的`结果集`封装为 `JavaBean对象`
>       -   `queryForObject(sql,Long.class)`：查询的`结果集`封装为 `对象`





注意：

-   以下代码中，为简化操作，直接使用了6.3小节的`DruidUtil.getDataSource();`来获取`DataSource`

```java
package cn.JdbcTemplete;

import cn.DruidDemo.DruidUtil;
import org.springframework.jdbc.core.JdbcTemplate;

public class Demo {
    public static void main(String[] args) {
        
        JdbcTemplate jt = new JdbcTemplate(DruidUtil.getDataSource());
        String sql = "update stu set spsw = ? where sid = ?";

        // 传入sql语句、sql中的参数
        int count = jt.update(sql,"abc123",3);

        System.out.println(count);

    }
}

```







下面测试-增删改查的代码：

```java
package cn.JdbcTemplete;

import cn.DruidDemo.DruidUtil;
import org.junit.Test;
import org.springframework.jdbc.core.JdbcTemplate;

import java.util.Map;

public class Demo2 {

    private JdbcTemplate jt = new JdbcTemplate(DruidUtil.getDataSource());

    
    
// 修改
    @Test
    public void jt_DML(){
        String sql = "update stu set spsw = ? where sid =?;";
        int count = jt.update(sql,"789",3);
        System.out.println(count);
    }
    
    
    
    

// 查询【1条记录】  
    @Test
    public void jt_DQL(){
        String sql = "select * from stu where sid =?;";

        Map<String,Object> mp= jt.queryForMap(sql,2);
        System.out.println(mp);
    }

    
    
    

// 查询【此处会报错，因为数据不止一条记录，但queryForMap方法只转化1条记录，】
    @Test
    public void jt_DQL2(){
        String sql = "select * from stu;";

        Map<String,Object> mp= jt.queryForMap(sql);
        System.out.println(mp);
    }

    
    
    
    
// 查询【多条记录】    
    @Test
    public void jt_DQL3(){
        String sql = "select * from stu;";

        List<Map<String,Object>> list1= jt.queryForList(sql);
        System.out.println(list1);
    }
    
    
    
    
//  查询   
      public void jt_DQL4(){
        String sql = "select * from stu;";
        
        List<Student> list1 = jt.query(sql,new BeanPropertyRowMapper<Student>(Student.class);     

        for (Student student : list1) {
            System.out.println(student);
        }
    }
                                       
                                       
                                       
                                       
// 查询【聚合函数】                                       
   public void jt_DQL5(){
        String sql = "select count(*) from stu;";
       
        Long total = jt.queryForObject(sql,Long.class);
       
        System.out.println(total);
    }         
                                       
                                       

}
```





