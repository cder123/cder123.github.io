---
title: Redis-笔记
tag: Redis
categories:
  - [SQL,Redis]
  - [后端,Java,Redis]  
 
---





<h1>Redis-笔记</h1>





# 零、参考资料

- [Redis 6-尚硅谷版](https://www.bilibili.com/video/BV1Rv41177Af?p=2)
- [Redis 6 -尚硅谷版-博客](https://zhangc233.github.io/2021/05/02/Redis/)
- [Redis 6-笔记](https://blog.csdn.net/weixin_47872288/article/details/118410080)





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



## 1、通用的操作



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





## 3、list 类型



list 类型是`单键多值`，一个键多个值。在底层使用的是双向链表，因此可以在任意位置进行插入和删除。

list 列表元素较少时，使用空间连续的**压缩列表**`ziplist`，

list 列表元素较少时，使用空间连续的**压缩列表**`ziplist`，





常用命令：

```shell

# 查看连续的多个数据，结束下标=-1时，表示开始下标到最后一个元素
lrange 键名 开始下标 结束下标

# (从左边开始)按下标查看列表的元素
lindex 键名 下标


# 获取列表长度
llen 键名


# 从左边插入
lpush 键名1 值1 键名2 值2


# 从右边插入
rpush 键名1 值1 键名2 值2


# 从左边删除（list左边的1个值），可以在命令最后指定要删除的元素个数
lpop 键名


# 从右边删除（list右边的1个值）
rpop 键名


# list1右边的数据移到list1左边
rpoplpush list1的键名 list2的键名



# 在第一个指定值的（前面brfore|后边after）插入新值
linsert 键名 before 指定值 新值
linsert 键名 after 指定值 新值


# 从左边删除n个指定值
lrem 键名 n 指定值


# 替换指定下标的值
lset 键名 下标 新值
```









## 4、set 类型



set 类型是一个可去重的 `string`类型的**无序集合**。

set 类型的底层是` value为 null 的 hash 表`，所以其增删改查的复杂度都是`O(1)`





常用操作：

```shell

# 添加多个值到set集合中
sadd 键 值1 值2


# 删除set集合中指定的元素
srem 键 值1 值2


# 随机删除set集合中的一个值
spop 键


# 取出set集合中的所有值
smembers 键


# 随机查询set集合中的n个值（不会删除）
srandmember 键 n


# 判断指定的set集合中是否存在该值,存在：1 ；不存在：0
sismember 键 值


# 返回set集合中的元素个数
scard 键



# 将值从源set集合移动到目标集合
smove src的key dst的key 值


# 返回两个集合的交集
sinter 键1 键2


# 返回两个集合的并集
sunion 键1 键2


# 返回两个集合的差集
sdiff 键1 键2
```







## 5、hash 类型



hash 类型对应的数据结构：压缩列表`ziplist`、哈希表`hashtable`。

`field-value`长度较短、且数量较少时，使用 `ziplist`。否则，使用 `hashtable`。



![image-20220304191813669](https://gitee.com/cder123/note-drawing-bed-01/raw/master/image-20220304191813669.png)

<center>hash 结构</center>



常用操作：

```shell

# 添加数据（hash的数据部分是哈希表，field相当于数据部分的哈希表的键）
hset 键 字段 值


# 获取数据
hget 键 字段


# 设置hash表数据部分的多个值
hmset  键 字段1 值1 字段2 值2


# 设置hash表数据部分的多个值
hexists  键 字段


# 获取指定的hash表所有的字段
hkeys  键


# 获取指定的hash表中所有的值
hvals  键


# 给指定的hash表中指定的字段所对应的值增加指定的增量
hincrby 键 字段 增量


# 当指定的字段不存在时，添加值
hsetnx 键 字段 值
```







## 6、zset 类型



zset 类型的**没有重复元素有序集合**，是元素均为字符串。

zset 集合的每个成员都关联的一个<font color=red>评分（score）</font>，按 “评分” **升序**将集合的的元素排列。

zset 集合的成员不能重复，<font color=red>但“评分”可以重复</font>。

 

因为元素是有序的，所以可以很快根据 “评分” 、“次序”来获取一个范围的元素。



在底层使用`跳跃表`实现，既有红黑树的效率，又比红黑树实现起来简单。



常用操作：

```shell
# 添加元素
zadd 键 评分 值1 值2 值3


# 返回范围在[起始下标 , 终止下标]之间的元素,若结尾有withscores，表示返回：评分+值
zrange 键 起始下标 终止下标 withscores


# 返回zset集合中，所有score值在[min,max]之间的集合值，按score升序排列(最大值后的参数可省略)
zrangebyscore 键 评分的最小值 评分的最大值 withscore limit 偏移量 个数


#返回zset集合中，所有score值在[min,max]之间的集合值，按score降序排列(最大值后的参数可省略)
zrevrangebyscore 键 评分的最小值 评分的最大值 withscore limit 偏移量 个数


# 给元素的score评分加上增量
zincrby 键 增量 值


# 删除指定的元素
zrem 键 值


# 统计zset集合内，score在 [min,max]内的元素个数
zcount 键 最小值 最大值


# 返回该值在集合中的排名，从0开始
zrank 键 值
```



<iframe style="height:600px;"  src="//player.bilibili.com/player.html?aid=247670776&bvid=BV1Rv41177Af&cid=326379063&page=12" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>





## 7、bitmaps 类型



常用于统计用户的访问量。

活跃的用户越多，（相比 set 类型）使用 bitmaps 来统计活跃用户就越省空间。



- bitmaps 类型 本质上还是字符串类型，但可以对字符串中的`位`进行操作。

- bitmaps 类型 单独提供了一套命令，所以在 Redis 中使用bitmaps和使用字符串的命令有所不同。

- 可以把 bitmaps 类型 看作是以`bit 位`为单位的数组，数组的每个单元都只能存储 0 和 1，数组的下标在 bitmaps 类型中叫<font style="color:purple;">偏移量</font>。



![image-20220307182133578](https://gitee.com/cder123/note-drawing-bed-01/raw/master/image-20220307182133578.png)







常用命令：

```shell

# 设置bitmap，偏移量是数组下标（一般用来表示用户的id），值是0或1
setbit 键 偏移量 值


# 获取
getbit 键 偏移量


# 统计 bitmaps 字符串中的有多少个bit位是1。开始、结束字节若为负数，表示从最后一个字节开始往前数
bitcount 键 开始字节 结束字节


# 位运算，可以做多个bitmaps的 and、or、not、xor等操作，并将结果保存到destkey所表示的bitmaps中
bitop 位运算符  运算结果的键 操作bitmaps1的键 操作bitmaps2的键
```





setbit：

![image-20220307182719390](https://gitee.com/cder123/note-drawing-bed-01/raw/master/image-20220307182719390.png)



![image-20220307182908997](https://gitee.com/cder123/note-drawing-bed-01/raw/master/image-20220307182908997.png)



bitop：

![image-20220307183940869](https://gitee.com/cder123/note-drawing-bed-01/raw/master/image-20220307183940869.png)



<iframe style="height:500px;"  src="//player.bilibili.com/player.html?aid=247670776&bvid=BV1Rv41177Af&cid=326379607&page=15" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>



## 8、hyperloglog 类型



统计相关的功能需求，例如：网站的 PageView（PV）可以使用Redis 的 incr、incrby 实现。

但是，例如：网站的 UniqueView（UV）、独立IP数、搜索记录数等需要去重的问题应该如何解决？



求集合中不重复元素的个数的问题——<font style="color:red;">基数问题</font>

> 基数问题的解决方案：
>
> - 存储在MySQL中，使用`distinct count(*)`计算不重复的个数。
> - 使用 Redis 提供的 hash、set、bitmaps 等数据结构。
>
>  
>
> 以上两种方法虽然结果精确，但随着数据量的增加，将导致占用的空间越来越多。



hyperloglog 类型 就是专门用于做`基数统计`的数据类型。



<font style="color:red;font-size:1.2em">优点：</font>

> 在输入的数据数量或体积非常大的时候，计算基数所需的空间<font style="color:red;">很小且固定</font>。
>
> 在 hyperloglog 类型中，每个键只需花费`12 KB`的内存就可以计算接近 <font style="color:red;">2^64^</font> 个不同的基数



<font style="color:red;font-size:1.2em">缺点：</font>

> 只会根据输入元素来计算基数，不会存储输入元素，因此不能返回输入的元素。





常用命令：

```shell

# 添加指定元素到 hyperloglog 中，添加成功返回1，失败返回0
pfadd 键 元素1 元素2 元素3 .......


# 统计基数
pfcount 键


# 将多个hyperloglog 类型的数据合并后保存到另一个 hyperloglog 中
pfmerge 合并后的结果键 待合并的htyperloglog1 待合并的htyperloglog2

```





<iframe style="height:500px;" src="//player.bilibili.com/player.html?aid=247670776&bvid=BV1Rv41177Af&cid=326379758&page=16" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>





## 9、geospatial 类型



geospatial 类型 存储的就是元素的二维坐标（经纬度）。

Redis 基于该数据类型，提供了 经纬度的设置、查询、范围查询、距离查询、经纬度hash等常见操作。



常用命令：

```shell

# 添加
# 两极无法直接添加，一般使用时会下载城市数据然后通过Java一次性导入。不能重复添加，超出范围会报错。
# 有效的经度：-180~+180
# 有效的纬度：-85.05112878 ~ 85.05112878
geoadd 键 经度1 纬度1 地点的名称1 [经度2 纬度2 地点的名称2........]


# 获取
geopos 键 地点的名称


# 获取两点之间的直线距离，长度单位：m米（默认）、km千米、ft英尺、mi英里
geodist 键 地点的名称1 地点的名称2 长度单位


# 找出方圆多少距离的元素
geo 键 经度 纬度 b距离 长度单位
```















# 三、Redis 配置文件



&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在安装时，我们将Redis的配置文件（`redis.conf`）复制到了`/etc/`目录下，并在启动`redis-server`时，指定了`/etc/redis.conf`配置文件。下面讲解Redis的配置文件。



## 0、常用设置



（1）注释掉 bind，使得 Redis 可以被外网PC访问：

```
# bind 127.0.0.1 -::1
```



（2）将 `protected-mode` 的值改为`no`，使得 Redis 可以被外网PC访问：

```shell
protected-mode no
```



（3）将 `daemonize` 的值改为`yes`，使得 Redis 可以后台运行：

```shell
daemonize yes
```



（4）设置`maxmemory`，防止内存不足时，服务器宕机：

```shell
maxmemory <bytes>
```







## 1、units 单位



&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Redis 的配置文件的开头定义了一些基本的度量单位，只支持 `byte`字节，不支持`bit`位。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font style="color:purple;">大小写不敏感</font>。

![image-20220307120806024](https://gitee.com/cder123/note-drawing-bed-01/raw/master/image-20220307120806024.png)





## 2、include 引入其他配置文件



```shell
include /path/to/local.conf

include /path/to/other.conf
```





## 3、network 网络设置



### 3.1、bind 绑定 IP 地址

建议注释掉，否则只能本地访问。

```shell
# bind 192.168.1.100 10.0.0.1     # listens on two specific IPv4 addresses
# bind 127.0.0.1 ::1              # listens on loopback IPv4 and IPv6
# bind * -::*                     # like the default, all available interfaces

bind 127.0.0.1 -::1
```





### 3.2、protected-mode 是否允许远程访问



```shell
# yes: 不能远程访问
# no: 可以远程访问
protected-mode yes
```





### 3.3、port 服务监听的端口



```shell
port 6379
```





### 3.4、 tcp-backlog 连接队列

tcp-backlog 是一个连接队列。

<font style="color:red;">tcp-backlog 连接队列总和 = 未完成三次握手的队列 + 已完成三次握手的队列</font>

在<font style="color:red;">高并发环境</font>下，需要将<font style="color:red;"> tcp-backlog </font>调高来避免<font style="color:red;">慢客户端连接</font>的问题。





<font style="color:red;">注意：</font>

> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Linux 会将`tcp-backlog`的值减小到`/proc/sys/net/core/somaxconn`的值（128）。所以，需要确认增大`/proc/sys/net/core/somaxconn` 和 `/proc/sys/net/ipv4/tcp_max_syn_backlog`（128）来达到想要的效果。



```shell
# TCP listen() backlog.
#
# In high requests-per-second environments you need a high backlog in order
# to avoid slow clients connection issues. Note that the Linux kernel
# will silently truncate it to the value of /proc/sys/net/core/somaxconn so
# make sure to raise both the value of somaxconn and tcp_max_syn_backlogprotected-mode yes
# in order to get the desired effect.

tcp-backlog 511
```







### 3.5、 time-out 超时



```shell
# Close the connection after a client is idle for N seconds (0 to disable)
timeout 0
```





### 3.6、tcp-keepalive 



300 秒没有操作就断开

```shell
# A reasonable value for this option is 300 seconds, 
tcp-keepalive 300
```





## 4、general 通用配置





### 4.1、daemon 后台启动



```shell
# By default Redis does not run as a daemon. Use 'yes' if you need it.
# Note that Redis will write a pid file in /var/run/redis.pid when daemonized.
# When Redis is supervised by upstart or systemd, this parameter has no impact.
daemonize yes
```





### 4.2、pidfile 文件



```shell
# Creating a pid file is best effort: if Redis is not able to create it
# nothing bad happens, the server will start and run normally.
#
# Note that on modern Linux systems "/run/redis.pid" is more conforming
# and should be used instead.
pidfile /var/run/redis_6379.pid
```





### 4.3、loglevel 日志等级



```shell
# Specify the server verbosity level.
# This can be one of:
# 	debug (a lot of information, useful for development/testing)
# 	verbose (many rarely useful info, but not a mess like the debug level)
# 	notice (moderately verbose, what you want in production probably)
# 	warning (only very important / critical messages are logged)
# 生产环境
loglevel notice
```





### 4.4、database 默认的数据库数



```shell
# Set the number of databases. The default database is DB 0, you can select
# a different one on a per-connection basis using SELECT <dbid> where
# dbid is a number between 0 and 'databases'-1
# 范围：[0,databases-1]
databases 16
```







## 5、security 安全



### 5.1、设置密码



在`redis-cli`客户端中，输入：

```shell
config get requirepass

config set requirepass "123456"

auth 123456
```





## 6、limit 限制



### 6.1、maxclients 最大客户端连接数



- 该配置用于设置Redis最多同时可以连接多少个客户端。

- 默认为`10000`个客户端。

- 若达到此限制，则拒绝新的连接请求，并向连接的请求方发送`max number of clients reached`来做回应。



```shell
# maxclients 10000
```





### 6.2、maxmemory 最大内存



- <font style="color:red;font-size:1.3em;">建议必须设置</font>，否则当内存满时，会造成服务器宕机。

- 设置 redis 可以使用的内存大小。一旦达到内存的使用上限，则会移除内部的数据。

- 移除数据的规则可以通过`maxmemory-policy`来指定。



```shell
maxmemory <bytes>
```







# 四、订阅与发布



## 1、什么是订阅和发布



Redis 发布和订阅是一种消息通信模式：

- 发布者（pub）发送消息
- 订阅者（sub）接受消息



`Redis 客户端`可以订阅任意数量的`频道`。





发布：

![image-20220307175345830](https://gitee.com/cder123/note-drawing-bed-01/raw/master/image-20220307175345830.png)



订阅：

![image-20220307175431929](https://gitee.com/cder123/note-drawing-bed-01/raw/master/image-20220307175431929.png)





## 2、订阅与发布的实现案例



### 2.1、Redis 客户端1 订阅频道



```shell
# 订阅频道(channel_1)
subscribe channel_1
```



![image-20220307180709571](https://gitee.com/cder123/note-drawing-bed-01/raw/master/image-20220307180709571.png)



### 2.2、Redis 客户端2 发布消息



```shell
# 向redis客户端1所订阅的频道channel_1 发布消息
publish channel_1 hello—world
```



![image-20220307181136474](https://gitee.com/cder123/note-drawing-bed-01/raw/master/image-20220307181136474.png)





### 2.2、Redis 客户端1 接收消息



![image-20220307181245499](https://gitee.com/cder123/note-drawing-bed-01/raw/master/image-20220307181245499.png)







# 五、Jedis 操作



## 1、Maven 依赖



```xml
		<dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
            <version>4.0.1</version>
        </dependency>
```





## 2、Jedis 的连通



（1）修改Redis的设置：

（注释掉`/etc/redis.conf`配置文件中的bind语句，protected-mode设为no，重启 Redis 服务）

```bash
# 关闭Redis服务
kill -9 6379


# 打开Redis服务
/usr/local/bin/redis-server /etc/redis.conf
```





（2）关闭CentOS 的防火墙

```bash
# 查看防火墙
systemctl status firewalld


# 关闭防火墙
 systemctl stop firewalld
```



（3）测试Jedis能否连通Redis服务器：

```java
package com.cyw;

import redis.clients.jedis.Jedis;

public class TestJedis {
    public static void main(String[] args) {

        // 根据ip地址、端口，创建Jedis对象
        Jedis jedis = new Jedis("192.168.220.137",6379);

        // 测试能否连通Redis服务器
        String ping = jedis.ping();
        
        // 打印 pong 表示已经连通
        System.out.println(ping);
        
        jedis.close();

    }
}

```





## 3、Jedis 的 API-keys



```java
 	@Test
    public void demo1(){
        Jedis jedis = new Jedis("192.168.220.137", 6379);

        // 查询Redis的当前数据库中的所有键
        // 相当于Redis原生的命令：keys *
        Set<String> keys = jedis.keys("*");
        System.out.println(keys);
    }
```





## 4、Jedis 的 API-String



其他类型与String类型的用法类型，都是将原生的命令改为方法。



```java
    @Test
    public void demo1(){
        Jedis jedis = new Jedis("192.168.220.137", 6379);
		
        // 添加str类型的数据
        String ok = jedis.set("k1", "abc");
        System.out.println(ok);
        
        
        // 获取str类型的数据
        String k1 = jedis.get("k1");
        System.out.println(k1);
        
        
        // 判断某个键是否存在
        boolean isExist = jedis.exists("k1");
        System.out.println(isExist);
        
                
        // 设置有效期（秒）
        jedis.expire("k1", 30);
        
        
        // 获取剩余有效期（秒）：-1永不过期，-2已过期
        long ttl = jedis.ttl("k1");
        System.out.println(ttl);
        
        
        // 批量设置
        jedis.mset("k1","a1","k2","a2");
        System.out.println(jedis.get("k1"));
        System.out.println(jedis.get("k2"));
        
        
        // 释放资源
        jedis.close();
        
    }
```







# 六、案例-模拟手机验证码





## 1、要求



- 输入手机号，点击发送，随机生成6位数字的验证码，2分钟有效
- 输入验证码，点击验证，返回成功或失败
- 每个手机号每天只能输入3次





## 2、实现思路



- 随机生成6位数字的验证码：Java中的`Random`类的`nextInt()`方法。
- 2分钟有效：使用 Jedis操作Redis设置验证码的有效期（`jedis.expire("键",有效秒数)`）
- 返回成功或失败：使用Jedis的`jedis.get("键")`获取Redis中的验证码，并将取出的值与用户的输入值比较
- 每天只能输入3次：使用Redis的`incrby`命令





## 3、实现



### 3.1、生成验证码



```java
 	// 生成6位数的验证码的方法
    public static String getCode(){
        Random rd = new Random();
        String str = "";
        for (int i = 0; i < 6; i++) {
            str += rd.nextInt(10);
        }
        return str;
    }
```





### 3.2、限制每个手机每天只能发送三次



```java
 	// 限制每个手机每天只能发送3次，
    // 存储验证码到Redis，设置过期时间
    public static void sendCode(String phone){

        Jedis jedis = new Jedis("192.168.220.137", 6379);

        // 拼接用户验证码生成次数的key
        String countKey = "verifyCode"+phone+":count";

        // 尝试去Redis中获取验证码的生成次数
        String count = jedis.get(countKey);
        
        if (count == null){
            // 为null表示没有验证码（第一次生成验证码）
            // 有效期：一天
            jedis.setex(countKey,24*60*60,"1");
        }else if (Integer.parseInt(count)<3){
            jedis.incr(countKey);
        }else if (Integer.parseInt(count)>=3){
            System.out.println("今天的发送次数已达3次，不能再发送了");
            jedis.close();
			return;
        }

        // 拼接用户验证码的key
        String codeKey = "verifyCode"+phone+":code";

        // 后端生成验证码
        String code = PhoneCode.getCode();

        // 设置验证码的有效期为2分钟
        jedis.setex(codeKey,120,code);

        jedis.close();        
    }
```



### 3.3、校验验证码



```java
 	// 校验验证码
    public static void verifyCode(String phone,String code){

        Jedis jedis = new Jedis("192.168.220.137", 6379);

        // 拼接用户验证码的key
        String codeKey = "verifyCode"+phone+":code";

        String redisCode = jedis.get(codeKey);

        if (redisCode.equalsIgnoreCase(code)){
            System.out.println("验证通过！");
        }else{
            System.out.println("验证失败！");
        }
        jedis.close();
    }
```





### 3.4、测试



```java
    public static void main(String[] args) {

//        sendCode("123456");
        verifyCode("123456","844525");
    }
```





### 3.5、完整版代码



```java
package com.cyw;

import redis.clients.jedis.Jedis;

import java.util.Random;

public class PhoneCode {
    public static void main(String[] args) {

//        sendCode("123456");
        verifyCode("123456","844525");
    }

    // 生成6位数的验证码
    public static String getCode(){
        Random rd = new Random();
        String str = "";
        for (int i = 0; i < 6; i++) {
            str += rd.nextInt(10);
        }
        return str;
    }


    // 限制每个手机每天只能发送3次，
    // 存储验证码到Redis，设置过期时间
    public static void sendCode(String phone){

        Jedis jedis = new Jedis("192.168.220.137", 6379);

        // 拼接用户验证码生成次数的key
        String countKey = "verifyCode"+phone+":count";

        // 尝试去Redis中获取验证码的生成次数
        String count = jedis.get(countKey);

        if (count == null){
            // 为null表示没有验证码（第一次生成验证码）
            // 有效期：一天
            jedis.setex(countKey,24*60*60,"1");
        }else if (Integer.parseInt(count)<3){
            jedis.incr(countKey);
        }else if (Integer.parseInt(count)>=3){
            System.out.println("今天的发送次数已达3次，不能再发送了");
            jedis.close();
            return;
        }

        // 拼接用户验证码的key
        String codeKey = "verifyCode"+phone+":code";

        // 后端生成验证码
        String code = PhoneCode.getCode();

        // 设置验证码的有效期为2分钟
        jedis.setex(codeKey,120,code);

        jedis.close();
    }


    // 校验验证码
    public static void verifyCode(String phone,String code){

        Jedis jedis = new Jedis("192.168.220.137", 6379);

        // 拼接用户验证码的key
        String codeKey = "verifyCode"+phone+":code";

        String redisCode = jedis.get(codeKey);

        if (redisCode.equalsIgnoreCase(code)){
            System.out.println("验证通过！");
        }else{
            System.out.println("验证失败！");
        }
        jedis.close();
    }
}

```







# 七、Redis 整合 SpringBoot





## 1、Maven依赖



```xml
		 <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>

        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-pool2</artifactId>
            <version>2.10.0</version>
        </dependency>
```







## 2、SpringBoot 的配置文件中的 Redis 部分



`application.properties`

```properties
spring.redis.host=192.168.220.137			# redis 服务器的地址
spring.redis.port=6379						# redis 服务器的端口
spring.redis.database=0						# redis服务器的数据库索引
spring.redis.timeout=1800000				# 连接超时时间（毫秒）
spring.redis.lettuce.pool.max-active=20		# 最大连接数，负值表示无限制
spring.redis.lettuce.pool.max-wait=-1		# 最大阻塞等待时间，负值表示无限制
spring.redis.lettuce.pool.max-idle=5		# 最大连接
spring.redis.lettuce.pool.min-idle=0		# 最小连接
```





## 3、SpringBoot 的 Redis 配置类



```java
@EnableCaching
@Configuration
public class RedisConfig extends CachingConfigurerSupport {

    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory factory) {
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        RedisSerializer<String> redisSerializer = new StringRedisSerializer();
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);
        template.setConnectionFactory(factory);
//key序列化方式
        template.setKeySerializer(redisSerializer);
//value序列化
        template.setValueSerializer(jackson2JsonRedisSerializer);
//value hashmap序列化
        template.setHashValueSerializer(jackson2JsonRedisSerializer);
        return template;
    }

    @Bean
    public CacheManager cacheManager(RedisConnectionFactory factory) {
        RedisSerializer<String> redisSerializer = new StringRedisSerializer();
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
//解决查询缓存转换异常的问题
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);
// 配置序列化（解决乱码的问题）,过期时间600秒
        RedisCacheConfiguration config = RedisCacheConfiguration.defaultCacheConfig()
                .entryTtl(Duration.ofSeconds(600))
                .serializeKeysWith(RedisSerializationContext.SerializationPair.fromSerializer(redisSerializer))
                .serializeValuesWith(RedisSerializationContext.SerializationPair.fromSerializer(jackson2JsonRedisSerializer))
                .disableCachingNullValues();
        RedisCacheManager cacheManager = RedisCacheManager.builder(factory)
                .cacheDefaults(config)
                .build();
        return cacheManager;
    }
}
```





## 4、测试类



```java
@RestController
@RequestMapping("/redisTest")
public class RedisTestController {
    @Autowired
    private RedisTemplate redisTemplate;

    @GetMapping
    public String testRedis() {
        //设置值到redis
        redisTemplate.opsForValue().set("name","学习java可看码农研究僧的博客地址，https://blog.csdn.net/weixin_47872288");
        //从redis获取值
        String name = (String)redisTemplate.opsForValue().get("name");
        return name;
    }
}
```





