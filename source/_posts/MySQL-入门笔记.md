---
title: MySQL-入门笔记
tag: MySQL
categories:
  - SQL
  - MySQL
---



# MySQL-入门笔记【5.6版本】

[toc]

## 1. 概述

### 1.1 MySQL的 安装 + 配置

-   [mysql-笔记-网传](https://blog.csdn.net/dzg_chat/article/details/88619120)
-   [MySQL-学习视频-b站](https://www.bilibili.com/video/BV1BX4y1G7cn)
-   [MySQL下载地址](https://downloads.mysql.com/archives/community/)
-   [MySQL（5.5）安装+配置](https://blog.csdn.net/CharmJay/article/details/94330268)
-   [Navicat 15.x for MySQL激活教程](https://www.jianshu.com/p/70143ef3d4a3)
-   [50道SQL练习题 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/32137597)
-   面试题：https://www.bilibili.com/video/BV14Y411V7eL



安装、配置完毕后，查看端口是否打开：

-   win + R
-   `services.msc`





### 1.2 命令行-连接MySQL

进入MySQL，查看当前MySQL中所有的数据库:

-   打开cmd
-   输入：`mysql -uroot -p`，回车
-   输入密码【刚才安装时，密码设为了：root】
-   `show databases;`【注意：mysql 中所有语句结束，必须在后面加 分号 来表示】







### 1.3 Navicat 连接 MySQL

-   点击 `连接` =》 `MySQL`
-   输入`连接名`【自定义的名字】，主机【ip地址】，端口【默认：3306】，用户名/密码【安装时设置的】
-   双击打开 `连接`





### 1.4 执行SQL

-   选中 `连接名` =》 右键 =》`新建查询`【或 直接点击工具栏的 `新建查询`】
-   编写SQL语句
-   点击 `运行`
-   右键`表` =》 `刷新`



### 1.5 导出数据表的SQL

-   选中 数据表
-   右键 =》 转储SQL文件  =》 结构和数据
-   选择保存的位置



## 2. select 查询-DQL

[练习-数据](https://note.youdao.com/ynoteshare1/index.html?id=45d4298f42397bd52ccf6fc716e27ee9&type=note#/)



```sql
-- 查询 student表的 所有内容
select * 
from student;



-- student表的 Sname列
select Sname 
from student;


```

**注意：** 

-   所有关键字最好大写，以便 与用户的数据部分 区分开

-   任意数据与null一起运算，结果都为 null 。

【由此可知：数据表在设计时，若某列需要参与计算，则最好不要允许插入null】





### 2.1 as 别名 ：

```sql

-- name 作为 Sname列 的别名，其中 as关键字 可以省略
select Sname as name 
from student;


-- 别名 中包含 空格 时，需要使用 反引号
select Sname as `name 1` 
from student;


select SId as `学号`,Sname as `姓名`, Ssex as `性别` 
from student;

```







### 2.2 distinct 关键字：

```sql
select distinct Sname  as `name 1` 
from student;
```







### 2.3 where 关键字

```sql
select SId as `学号`,Sname as `姓名`, Ssex as `性别` 
from student
where SId ="01";		-- 找学号为01的学生



select SId as `学号`,Sname as `姓名`, Ssex as `性别` 
from student
where SId != "01";	-- 找学号不是01的学生



select SId as `学号`,Sname as `姓名`, Ssex as `性别` 
from student
where SId != "01" and SId != "02"; 	-- 找学号 不是01，也不是02 的学生
```

### 2.4 between 关键字

```sql
SELECT * FROM `sc`
where 
	score >= 80 and score <=90;



-- 等价于

SELECT * FROM `sc`
where score between 80 and 90;



```

### 2.5 in 关键字

```sql
SELECT * FROM `sc`
where score in (70,80,90);		-- 找出 分数在 集合(70,80,90) 内的记录
```



### 2.6 like 关键字

like + %或者_ ，可以匹配字符串：【%多个字符，_1个字符】

-   like  “%雷%”：找出所有名字中有“雷”的人
-   like “吴_”：找出姓吴，且名字是2个字的人



```sql
SELECT * 

FROM `s_c_t_sc_view`

where Sname like "吴%";		-- 找出姓吴的人

```

### 2.7 order by 关键字

```sql
SELECT * 	
FROM `s_c_t_sc_view` 	
ORDER BY 
	CId ASC , score DESC; -- cid升序，score降序
		
							--【desc降序 / asc升序】,【不指定des时，默认升序】

```





### 2.8 group by  + having 条件

```sql
-- 无条件的分组
	select SId,Sname 	
	from `s_c_t_sc_view`	
	group by SId			--按学号分组


	select SId,Sname,max(score)	
	from `s_c_t_sc_view`	
	group by SId			--按学号分组,找出每个人的每个科目的最高分

-------------------------------------------------------------------

-- 有条件的分组
	SELECT 	SId,max( score ) AS max_score 
	FROM `sc` 	
	GROUP BY sid	
	HAVING max_score >= 60		-- 按学号分组后，找出最高成绩 > 60 的学生的学号和最高分数


```





### 2.9 子查询

```sql
select Sname,max_score

from student as s
	join
		(
            select	SId,max(score) as max_score from `sc` group by sid
        ) as t
	on s.SId = t.SId
 	
-- 先 按学号分组 并进行 子查询，找出每个学生所有课程的最高成绩；
-- 然后 将子查询的结果作为一张表连接到 学生表
-- 最后找出 学生姓名+最高个人的成绩
```







### 2.10 常用函数

| 函数                                          | 效果                |
| --------------------------------------------- | ------------------- |
| select     upper("aaa") as `str1`;            | “AAA”               |
| select     lower( “AAA”)                      | “aaa”               |
| select     concat('abcdefg','123') as `str1`; | `www-baidu-com`     |
| select     char_length("aaa") as `str1`;      | 3                   |
| select     substring('123456',1,3) as `str1`; | “123”               |
| select     ltrim('   abc   ') as `str1`;      | “abc   ”            |
| select     rtrim('   abc   ') as `str1`;      | “   abc”            |
| select     trim('   abc   ') as `str1`;       | “abc”               |
| select     abs(- 4.2) as `num1`;              | 4.2                 |
| select    ceil(- 4.21) as `num1`;             | -4                  |
| select    floor(- 4.21) as `num1`;            | -5                  |
| select   round(- 4.2138 , 3) as `num1`;       | -4.214              |
| select  curtime( ) as `now_time`;             | 11:59               |
| select  now( ) as `now`;                      | 2021-05-08 12:00:03 |
| select  year(now( )) as `now`;                | 2021                |
| select month(curdate( )) as `now`;            | 5                   |
|                                               |                     |
| count( 属性名 )                               |                     |
| count( * )                                    |                     |
| sum( 属性名)                                  |                     |
| avg( 属性名 )                                 |                     |
| min( 属性名)                                  |                     |
| max( 属性名)                                  |                     |
|                                               |                     |



## 3. 基本表-DDL

### 3.1 创建基本表 + 插入数据

[练习-数据](https://note.youdao.com/ynoteshare1/index.html?id=45d4298f42397bd52ccf6fc716e27ee9&type=note#/)

```sql
create table Student(
    SId varchar(10),
    Sname varchar(10),
    Sage datetime,
    Ssex varchar(10)，
    primary key (SId)
)engine=InnoDB default charset=utf8;

-- 插入 数据
insert into Student values('01' , '赵雷' , '1990-01-01' , '男');
insert into Student values('02' , '钱电' , '1990-12-21' , '男');
insert into Student values('03' , '孙风' , '1990-05-20' , '男');
insert into Student values('04' , '李云' , '1990-08-06' , '男');
insert into Student values('05' , '周梅' , '1991-12-01' , '女');
insert into Student values('06' , '吴兰' , '1992-03-01' , '女');
insert into Student values('07' , '郑竹' , '1989-07-01' , '女');
insert into Student values('09' , '张三' , '2017-12-20' , '女');
insert into Student values('10' , '李四' , '2017-12-25' , '女');
insert into Student values('11' , '李四' , '2017-12-30' , '女');
insert into Student values('12' , '赵六' , '2017-01-01' , '女');
insert into Student values('13' , '孙七' , '2018-01-01' , '女');


```

#### 3.1.1 常见约束

【主要用于 表的定义阶段】

-   主键：primary key( 属性名)   =》 `primary key (SId,CId)`【SC表】
-   外键： foreign key 外键名(属性名) references  表名 (属性名)  =》 `foreign key fk_cid(cid) references Course(CId)`
-   空/非空：null/not null  【判断：is null , is  not null】=》 `Sname char(10) not null`
-   唯一：unique =》`CardID char(10) unique `
-   自定义：check(条件) =》 `age int check(age >=0 and age <=150 )`
-   默认值：default 默认值 =》`age int default 20 `
-   自增：auto_increment =》`id int primary key auto_increment`【不允许手动插入，插的时候填null】



外键实例：

```sql
create table sc(
	sid int ,
    cid int,
    score varchar(20),
    foreign key fk_sid(sid) references s(Sid),
    foreign key fk_cid(cid) references c(Cid),
    primary key (sid,cid)
);
```







自增实例：

```sql
-- 自增实例:
create table person(
	id int primary key auto_increment,
    name varchar(20)   	
)


--自定义自增的初始值
create table person(
	id int primary key,
    name varchar(20)   	
) auto_increment=100		--自增初始值为100
```





### 3.2 删除基本表-数据

```sql
drop table student;

```



### 3.3 修改基本表-数据

```sql
update student 
	set student.Sname="aaa" 
	where student.SId = "01";

```
### 3.4 查询基本表-数据

```sql
select * from student;
```



## 4.视图

### 4.1 创建视图

```sql
create view s_c_t_sc_View
as
select  student.SId,student.Sage,student.Sname,student.Ssex,	--学生表
		course.CId,course.Cname,								--课程表
		teacher.TId,teacher.Tname,								--教师表
		sc.score												--成绩表

from  student,course,teacher,sc
where student.SId = sc.SId 
	and course.CId = sc.CId
	and course.TId  = teacher.TId;





```

### 4.2 删除视图

```sql
drop view s_c_t_sc_View;
```



## 5. 分页查询

limt  start，单个分页长度offset

初始下标start：从0开始

终点下标：(start + offset - 1)

偏移量offset：分页大小（分页后的记录条数）

```sql
SELECT * 
FROM `s_c_t_sc_view`
order by score desc
limit 5;			-- 分页，每页5条数据【前5条，从0开始数】



SELECT * 
FROM `s_c_t_sc_view`
order by score desc
limit 5,5;			-- 分页，每页5条数据，第一个5是 起点的下标，第二个5是 偏移量【第5条~第10条记录】
					-- 初始下标：5，分页长度5，终点下标：9
```





## 6. 索引

索引：在1列或几列，建立索引，可以提高查询速度，相当于给数据表加了个目录。【适合调优】



索引建立后，可以在 “设计表” 界面下的 “索引” 中查看



常用的索引：

-   B -Tree【实际上是B+树】
-   Hash【innoDB引擎 不支持 Hash索引】



索引的 创建、删除：

```sql
-- 创建索引
create index idx_sname 
on student(Sname);



-- 删除索引
drop index idx_sname 
on student
```







## 7. 事务

### 7.1 事务的定义：

事务：【Trans action】

>   一组 要么 全执行成功，要么全执行失败的SQL操作。
>
>   【`默认`：1个**DML**语句(数据的增、删、改)，就是1个**事务**】
>
>   
>
>   事务由 DBMS 中的事务处理子系统控制。
>
>    
>
>   常见的 存储引擎：InnoDB【5.5版本后默认，支持事务】，myISAM【不支持 事务】



### 7.2 事务的特点：【ACID】

-   **原子性** ：事务中所有不可再分的操作，要么全成功，要么全失败
-   **一致性** ：从一个一致的状态  =到=》另一个一致的状态
-   **隔离性** ：多个事务之间 互不干扰
-   **持久性** ：一旦提交，永久存储



### 7.3 事务-实例1：

事务-实例：

```sql
update account set balance= balance - 200 where id=1;
update account set balance= balance + 200 where id=3;	--以上是2个事务


--------------------------------------------------------

--手动开启事务，将上面两条SQL语句放在1个事务里【回滚/提交前,数据是在缓存中修改】
	start transaction;

	update account set balance= balance - 200 where id=1;
	update account set balance= balance + 200 where id=3;

--手动回滚
	rollback;
	
--手动提交
	commit;


--------------------------------------------------------

【案例】：
	-- 关闭自动提交
        set autocommit = 0;	
        start transaction;	-- 开启事务

   --  更改数据
        update account set money = money - 100 where `name` = 'A';
        update account set money = money + 100 where `name` = 'B';
  
  --	成功->提交
        commit;
        
  --   失败-> 回滚       
        rollback;
        
  --   重新打开事务的自动提交
        set autocommit = 1;



```

### 7.4 事务的并发：



#### 7.4.1 脏读：

>   `事务A` 读取了 `事务B`  **尚未提交** 的数据，导致2次读取的数据的不一致。
>
>   
>
>   【读取未提交的数据】



#### 7.4.2 不可重复读：

>   `事务A`  多次读取了数据，在读取还没结束时, `事务B`  修改数据，导致2次读取的数据的不一致。
>
>   
>
>   【数据**更新**导致的 =》多次读取的数据不一致】  
>
>   =》**解决方案**：锁 1行 记录



#### 7.4.3 幻读

>   `事务A` 读取数据后，`事务B` 插入数据，`事务A` 再次读取数据时，两次数据不一致.
>
>    
>
>   【数据**插入**导致的 =》多次读取的数据不一致】
>
>    
>
>   =》**解决方案**：锁 1张 表



### 7.5 事务的隔离级别



 隔离级别：为解决并发问题。



-   读 未提交 ( read uncommited)：会出现 =》脏读+不可重复读+幻读
-   读 已提交 ( read commited)：     会出现 =》不可重复读+幻读
-   可重复读  ( repeatable read)：   会出现 =》幻读
-   序列化      ( serializable )：





```sql
-- 查看默认的事务隔离级别，MySQL默认的隔离级别：可重复读
	select @@transaction_isolation;
	

-- 设置事务的隔离级别
	set session transation isolation level read uncommited;
	set session transation isolation level read commited;
	set session transation isolation level repeatable read; -- MySQL默认的隔离级别
	set session transaction isolation level serializable;
```











## 8. 存储过程



存储过程：相互之间有关联、可以使用流程控制的SQL语句的集合【类似：批处理语句】



使用 `存储过程` 的优点:

-   **提高性能**。每一次执行SQL都要进行语法分析、编译、执行。而**使用存储过程后**，只需进行1次语法分析、编译、执行。
-   **减轻负担**。使用存储过程后，只需client向server传递参数，无需传递SQL语句。
-   **将数据库的处理黑盒化** 。应用程序**无需在意 存储过程内部** 的详细处理。



```sql
-- 定义：1个 无返回值 的存储过程

	select * from emp where ename like "%诸葛%";
	
	create procudure myPro01(name varchar(20))
	begin 
		if name is null or name ="" then
			select * from emp;
        else
        	select * from emp where ename like concat("%",name,"%");
        endif;
	end;
	
	
-- 删除 储过程
	drop procudure myPro01;
	
	
	
--调用 存储过程
	call myPro01(null);
	call myPro01("孔明");
	
	
	
-----------------------------------------------------------		
-----------------------------------------------------------


-- 定义：1个 有返回值 的存储过程
	select * from emp where ename like "%诸葛%";
	
	-- in可省略，out修饰返回值
	create procudure myPro02(in name varchar(20),out num int(3)) 
	begin 
	
		if name is null or name ="" then
			select * from emp;
        else
        	select * from emp where ename like concat("%",name,"%");
        endif;
        
        
        --found_rows()是MySQL中 返回查询结果的条数的函数
        select found_rows() into num;	
	end;
	
	
	
-- 调用 存储过程
	call myPro02("张",@num);
	select @num;

```



## 9. 数据控制语言-DCL



回顾：

-   DDL：数据定义语言  =》create、alter、drop
-   DML：数据操纵语言 =》insert，delete，update
-   DQL：数据查询语言 =》select
-   DCL：数据控制语言 =》grant、revoke





DCL：数据控制语言



DCL的功能：

>   -   管理用户：添加用户、删除用户、修改用户密码、查询用户
>   -   授予用户权限：



用户信息：

>   存储在 `mysql_db -> user表`

### 9.1 管理用户

```sql
// 步骤：
//	1. 切换到mysql数据库
//	2. 查询user表

use mysql;

- 查询用户
	select * from user; 
	
	
- 创建用户【主机名可以用%,表示所有主机】
	create user '用户名'@'主机名' identified by '密码';
	
	
	
- 删除用户
	drop user '用户名'@'主机名';
	

- 修改用户密码【方式-1】	
	update user set password = password('新密码') where user = '用户名'; 
	
- 修改用户密码【方式-2】	
	update password for '用户名'@'主机名' = password('新密码'); 	
	


```



### 9.2 授予权限：

```sql
- 查看权限
	show grants for '用户名'@'主机名';
		
		例如：
			show grants for 'lisi'@'%';
	
	
	
- 授予权限
	grant 权限列表 on 数据库名.表名 to '用户名'@'主机名';
	
			例如：
				grant select,update on school.stu to 'user_1'@'localhost';
				
				// 给用户zhangsan授予所有库的所有权限
				grant all on *.* to 'zhangsan'@'localhost';
		
	
		
	
- 撤销权限
	revoke 权限列表 on 数据库名.表名 from '用户名'@'主机名';
	
			例如：
				revoke insert,update on school.stu from 'lisi'@'localhost';
	
```





### 9.3 解决：忘记Root用户密码的问题：

```sql
- 解决忘记Root用户密码的问题【步骤】：
        1. 管理员身份运行-cmd : 输入 net stop mysql

        2. 以无验证的方式启动 mysql : 输入 mysqld --skip-grant-tables

        3. 再启动1个新的cmd窗口：输入 mysql

        4. 改密码：
                use mysql;
                update password for '用户名'@'主机名' = password('新密码');

        5. 任务管理器 => 关闭 mysqld进程	
        
        6. 重新启动mysql服务：net start mysql
```









## 10. 面试题：



### 10.1 比较效率：

```sql
select * from emp where deptNo=10 and eName like "%A%"

select * from emp where eName like "%A%"and  deptNo=10
```

解答：

>   理论上，第一条语句的效率更好，因为查询 数字 比 查询 字符串 速度更快。
>
>   实际上，数据库会对SQL语句进行优化





### 10.2 主从复制的延迟问题：

主从复制的延迟问题 =》 涉及到MySQL集群



MySQL集群：

-   =》主从复制 
-    =》读写分离【部分DB专门用于‘读’，剩余的DB负责‘写’】 => 需要保持：数据一致
-   =》分库分表





DB 性能 取决于：

-   内存
-   CPU
-   磁盘
-   带宽



DB的优化：

-   监控 SQL
-   查看 连接数
-   设计 数据表时，选择合适的数据类型
-   建立 索引
-   设置 MySQL 参数
-   建立 分布式集群





日志：

-   bin log       =属于=》 mysql 服务		
-   undo log   =属于=》 innodb存储引擎
-   redo log    =属于=》 innodb存储引擎



主从复制的过程：

-   client 对 **master DB** 进行 DML 后，产生**bin log**。

-   bin log 在主从复制时，通过**IO线程**，被 **slave DB**借鉴，在slave上进行**持久化存储**，产生 **中继日志 relay log** 。【效率较高的阶段，顺序读写】

-   通过 **sql 线程** 对 relay log 进行重放【效率较低的阶段，随机读写】，可以在 slave 上恢复 master 的 bin log。

【binlog  需要手动开启】





粒度：【粒度越小，效率越高】

-   库【5.6以上版本支持】
-   表
-   行





主从复制中的延时问题：

>   延时 主要发生在 sql 线程运行阶段【MTS：muti-thread salve】

