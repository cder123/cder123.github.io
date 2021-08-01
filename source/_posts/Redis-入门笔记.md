---
title: Redis-入门笔记
tag: Redis
categories:
  - [SQL,Redis]
  - [后端,Java,Redis]  
 
---





# Redis-入门笔记

[toc]



## 1、概述

Redis是一种高性能的非关系型数据库。



关系型数据库与非关系型数据库是互补的。



思想：缓存（将常用但又不经常变动的数据放入缓存）, 将Redis作为1个大缓存。



![](https://z3.ax1x.com/2021/07/31/WjPQHA.png)





---





## 2、下载安装



在生产中，使用的是Linux版，以下为便于学习，使用了Windows版，占用`6379`端口。



-   下载地址：[Redis中文网](https://www.redis.net.cn/)
-   安装：可以直接解压使用



主要的3个文件：

>   1.  `redis.windows.conf`：配置文件
>   2.  `redis-cli.exe`：客户端
>   3.  `redis-server.exe`：服务器





---





## 3、Redis 的使用





### 3.1 Redis的数据结构（数据类型）：

>   1、键值对（key-value）：其中键为字符串，值有以下<font style="color:red;">5种类型</font>：
>
>   -   string：字符串类型的数据
>   -   hash：map格式的数据
>   -   set：不重复的无序集合
>   -   sortedSet：不重复的有序集合
>   -   list：linkedList类型的数据





---





### 3.2 常用操作命令



1、string 的 <font style="color:red;">存储、获取、删除</font>

>   -   `set 键 值`
>   -   `get 键 值`
>   -   `del 键 值`



2、hash 的  <font style="color:red;">存储、获取、删除</font>

>   -   `hset 键 字段名 值`：`hset myhash username 123`，`hset myhash psw 456`
>   -   `hget 键 字段名` 或 `hgetall 键 `
>   -   `hdel 键 字段名 `





3、list 的  <font style="color:red;">存储、获取、删除</font>

>   -   `lpush`
>   -   `rpush`
>   -   `lrange 键 起始下标 终止下标`：起始下标从0开始
>   -   `lpop 键`





4、set 的  <font style="color:red;">存储、获取、删除</font>

>   -   `sadd 键 值`
>   -   `smembers 键 `
>   -   `srem 键 值`





5、sortedSet   <font style="color:red;">存储、获取、删除</font>

>   -   `zadd 键 分数 值`
>   -   `zrange 键 起始下标 终止下标`：zrange myss 0 -1 withscore
>   -   `zrem 键 值`





6、通用命令：

>   1.  `keys *`：
>   2.  `type 键`
>   3.  `del 键`





---



### 3.3 Redis数据的持久化



-   RDB：（redis.windows.conf文件）默认的机制，在一定的间隔时间内检测key的变化，并持久化。

>   1.  编辑配置文件：
>
>       例如：`save 900 1` ，表示 900秒（15min）后有1个key发生变化，就持久化1次。 
>
>   2.  重启Redis服务,并指定配置文件名：`redis-server.exe redis.windows.conf `



-   AOF：以日志的形式记录每条命令。

>   1.  编辑配置文件：
>
>       将`redis.windows.conf文件`下的`appendonly配置`改为`yes`
>
>       a. `appendfync always`：每次操作都持久化。
>
>       b. `appendfsync everysec`：每隔一秒持久化1次
>
>       c. `appendfsync no`：不持久化







---







## 4、Jedis

Jedis 是Jave操作Redis数据库的工具，相当于jdbc的作用。



步骤：

>   1、下载并导入Jar包（`commons-pool.jar`和`jedis.jar`）
>
>   2、使用



字符串的使用（其他数据类型类似）

```java
// 在Java中，导入相关的2个jar包，new出对象

	Jedis jedis = new Jedis("ip地址",端口号)

// 存储数据    
	jedis.set("键","值")
 
// 指定过期时间    
	jedis.setex("键",毫秒数)    
        
// 关闭连接    
	jedis.close()    

```







---







## 5、JedisPool 连接池

```java
// main函数
	JedisPoolConfig config = new JedisPoolConfig()
	config.setMaxTotal(50);	// 最大连接数
	config.setMaxIdle(10);	// 空闲连接

	JedisPool jp = new JedisPool(config,"ip地址",端口号);

	Jedis jedis = jp.getResource();

	jedis.set("键"，值);

	jedis.close()
        
```





一般写1个Jdedis的工具类，提供设置参数和获取连接的方法。

-   配置文件(`jedis.properties`)：

```properties
host=127.0.0.1
port=6379
maxTotal=50
maxIdle=10

```



-   工具类（`JedisPoolUtils`）

```java

// 步骤：
//  1. 加载配置文件: /src/jedis.properties文件
//	2、获取连接：getJedis()

public class JedisPoolUtils{
    private static JedisPool jp;
    
    static{
        InputStream is = JedisPoolUtils.class.getClassLoader.getResourceAsStream("jedis.properties");
        
        Properties properties = new Properties();
        properties.load(is);
        
        JedisPoolConfig config =  new JedisPoolConfig();
        
        config.setMaxTotal(properties.getProperty("maxTotal"));	// 最大连接数
		config.setMaxIdle(properties.getProperty("maxIdle"));	// 空闲连接
        
        jp = new JedisPool(config,properties.getProperty("host"),properties.getProperty("port"));
    }
    
    
    public static Jedis getJedis(){        
        return jp.getResource();
    }
    
}
```

