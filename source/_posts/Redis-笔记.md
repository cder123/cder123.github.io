---
title: Redis-笔记
tag: Redis
categories:
  - [SQL,Redis]
  - [后端,Java,Redis]  
 
---





<h1>Redis-笔记</h1>





# 一、Redis 的安装



## 1、安装步骤



1、安装GCC环境：

```shell
yum install gcc

gcc --vserion
```



2、上传 **Redis的压缩包** 到`/opt/`目录下



3、解压 **Redis的压缩包**：

```shell
tar -zxvf 压缩包名
```



4、进入解压后的目录，编译并安装：

```shell
cd 压缩包名

make

make install
```



5、安装完毕（默认安装在`/usr/local/bin/`目录下）



<font color=red>注意：</font>

此时，Redis有两个主要的目录，一个是解压目录（`/opt/redis-6.2.2/`），一个是安装目录`/usr/local/bin/`。





## 2、安装过程中可能出现的报错



问题：

> 若没有准备好C语言的编译环境，make编译时，会产生`–Jemalloc/jemalloc.h：没有那个文件`的报错。



解决：

> 在`/opt/redis解压目录/`目录下， 执行命令`make distclean`







## 3、Redis 安装目录介绍



|       目录名       | 说明                |
| :----------------: | :------------------ |
| `redis-benchmark`  | 性能测试工具        |
| `redis-check-aop`  | 修复AOP文件         |
| `redis-check-dump` | 修复dump.rdb文件    |
|  `redis-sentinel`  | redis集群使用       |
|   `redis-server`   | Redis服务器启动命令 |
|    `redis-cli`     | 客户端，操作入口    |





## 4、Redis 启动服务端步骤



Redis有 **前台启动** 和 **后台启动** 两种。前台启动就是shell窗口不能关；后台启动就是后台默默运行。

推荐使用Redis的后台启动。



1、备份`redis.conf`文件：

```shell
# 进入redis的解压目录
cd /opt/redis-6.2.6/


# 将redis.conf 文件备份到 /etc目录下
cp ./redis.conf /etc/redis.conf

```



2、将`/etc/redis.conf` 文件(128行)中的`daemonize no`改为`yes`

```shell
vi /etc/redis.conf

# 找到 daemonize 所在的行，输入 i 进入插入模式，将no改为yes

# 按 esc 键，输入 :wq 保存
```



3、进入redis的安装目录`/usr/local/bin/`，启动：

```shell
# 进入redis的安装目录
cd /usr/local/bin

# 启动redis的服务端，并指定配置文件（刚才备份到了 /etc 目录下）
redis-server /etc/redis.conf

# 查看是否启动成功
ps -ef | grep redis
```

![image-20220302174936819](https://gitee.com/cder123/note-drawing-bed-01/raw/master/image-20220302174936819.png)







## 5、Redis 启动客户端步骤



进入redis的安装目录，执行启动redis客户端的命令：

```shell
cd /usr/local/bin/

redis-cli
```





## 6、Redis 关闭客户端步骤



Redis关闭客户端有三种方式。



> 单例关闭：

```shell
redis-cli shutdown
```



> 进入终端关闭：

```shell
shutdown
```



> 多例关闭：

```shell
redis-cli -p 6379 shutdown
```





## 7、Redis 相关知识的介绍



Redis的端口是`6379`，默认有`16`个数据库（编号0~15），默认0号库。

redis 基于key-value。

redis 具有统一的密码管理（所有库的密码都相同）。

reids 是单线程 + 多路IO复用技术。

redis 与 Memcache 的不同：数据类型更多、支持持久化、单线程+IO复用





切换数据库：

```shell
select 数据库编号
```



查看当前数据库的key数：

```shell
dbsize
```



清空库：

```shell
flushdb
```





清空所有库：

```shell
flushall
```









# 二、Redis 常用数据类型



## 1、常用的操作



常用操作：

```shell

# 切换redis数据库,redis默认有16个数据库，编号为0~15
select 数据库编号


# 查看当前的数据库有多少key
dbsize


# 清空当前库
flushdb


# 清空所有库
flushall


# 查看当前库中的所有key
keys *


# 判断某个key是否存在
exits 键名


# 判断key 的数据类型
type 键名


# 删除指定的key及其对应的数据
del 键名


# 根据value值选择非阻塞删除，仅将keys从keyspace元数据中删除，真正的删除会在后续的异步操作
unlink 键名


# 给某个key设定过期时长
expire 键名 秒数


# 查看还有多少秒过期，-1表示永不过期，-2表示已过期
ttl 键名
```









Redis 常用的五大数据类型：

> - `string`
> - `list`
> - `set`
> - `zset`
> - `hash`





## 2、string 类型



string 类型是 redis 的一个基本类型，一个 key 对应一个 value 。每个value最大为 `512 MB`。

string 类型是 `二进制安全`的，可以包含任何数据，如：图片、序列化对象。



常用命令：

```shell

# 设置/修改数据
set 键名 值


# 读取数据
get 键名


# 追加数据,没有键则新建，有键则追加到原数据的末尾
append 键名 值


# 获取键所对应的value的长度
strlen 键名


# 只有不存在key时，才设置键值对
setnx 键名 值


# 让数值value自增1（只有是数值时才生效）,若为空，则设置后为1
incr 键名


# 让数值value自减1（只有是数值时才生效）,若为空，则设置后为1
decr 键名


# 指定步长的自增
incrby 键名 步长


# 指定步长的自减
decrby 键名 步长


# 一次性设置多个key-value
mset 键名1 值1 键名2 值2


# 一次性获取多个key-value
mget 键名1 键名2


# 当所指定的key不存在的时候，一次性设置多个key-value
msetnx 键名1 值1 键名2 值2


# 截取子串，类型Java中的substring()函数，左右均包括
getrange 键名 开始下标 结束下标


# 设置key-value的同时指定过期时间
setex 键名 过期时间 值


# 以新换旧，获取旧值、设置新值
getset 键名 新值
```



原子性：不会被线程调度打断。单线程时，都是原子操作；多线程时，不被多线程打断操作的就是原子性。（一个失败，则全部失败）

Redis的原子性得益于Redis的单线程。



string 类型就是一个简单动态字符串（SDS），类型Java中ArrayList的效果。

string 内部的数据小于`1 MB`时，翻倍扩容；数据大于`1 MB`时，每次最多扩容`1 MB`。最大长度`512 MB`。





