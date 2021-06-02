---
title: SQL Server-入门笔记
tag: SQL Server
categories:
  - SQL
  - SQL-Server
---

# SQL Server-入门笔记

[toc]

## 1. 登录

2种方式：

-   windows身份验证：登录windows账户后就能使用 数据库
-   混合身份验证：登录windows账户后，还需要知道 数据库的用户名+密码



## 2. 数据库

### 2.1 创建数据库

```sql
CREATE DATABASE school
on
(
	name = 'school',
	filename = 'E:\DB_demo\school.mdf',
	size=5mb,
	maxsize = 50mb,
	filegrowth = 1%


)
-- 后可以跟 其他文件组，文件组与文件组之间使用逗号分隔


log on(
	name = 'school_db',
	filename = 'E:\DB_demo\school_log.ldf',
	size=1mb,
	maxsize = 10mb,
	filegrowth = 1%

)
```

### 2.2 删除数据库

```sql
drop database school
```



### 2.3  数据库文件的修改

```sql
-- 添加 数据库文件
alter database school 
add file(
	name =ciyao,
	filename = 'E:\DB_demo\ciyao.ndf',
	size=5,
	maxsize=10,
	filegrowth=1%
)
go

--*******************************************

-- 修改 数据库文件
alter database school
modify file(
	name =ciyao,
	filename = 'E:\DB_demo\ciyao.ndf',
	size=6,
	maxsize=10,
	filegrowth=1%

)

--*******************************************

-- 删除 数据库文件
alter database school
remove file ciyao
```







## 3. 数据表

### 3.1 创建数据表

创建 3个数据表

```sql
CREATE TABLE student
(
	sno int PRIMARY KEY,
	sname char(10) constraint not_null_cons NOT NULL,
	age int CHECK(age>=0 AND age<150)

)
go


--*******************************************

CREATE TABLE class
(
	cno int PRIMARY KEY,
	cname char(20) ,
	credit float CHECK(credit>=0 AND credit<5)

)
go

--*******************************************

CREATE TABLE sc
(
	sno int FOREIGN KEY REFERENCES student(sno),
	cno int FOREIGN KEY REFERENCES class(cno),
	grade float CHECK(grade>=0 and grade<=100),
	PRIMARY KEY(sno,cno)

)
go
```





### 3.2 删除数据表

```sql
drop table student
```





### 3.3 修改数据表

```sql

-- 添加 属性
alter table student
	add
		sex char(2) default '男',
		address char(20)
go

--*******************************************

-- 修改 属性
alter table student
	alter column
		sname char(12) 
go


```





## 4. 数据

### 4.1 插入数据：insert

```sql
use school


insert into student(sno,sname)
values(1,'张三'),(2,'李四')
go

--*******************************************

insert into class(cno,cname,credit)
values(11,'计算机',2.5),(12,'高数',4.0)
go

--*******************************************

insert into sc
values(1,11,60),(2,12,70)
go

```





### 4.2 删除数据：delete

```sql
delete from s 
where s.sno='A123'
```



### 4.3 修改数据：update

```sql
update sc  
set sc.score = 90
where sc.sno = 'A123'
```



### 4.4 查询数据：select

```sql

-- 查询名字为小明，且分数大于80的学生的学号、姓名、成绩,【按成绩：降序排列】
select s.sno  , s.sname ,  sc.score
from s,sc
where  s.sname='小明' and sc.sc.score>=80
order by sc.score desc
```

select语句的执行顺序：

-   from
-   on
-   join
-   where
-   group by
-   having
-   order by
-   select

## 5. 视图

### 5.1 创建视图

```sql
create view     st_Details_View
as
select  s.sno as ‘学号’, sname as '姓名', 
		sage as '年龄', c,cno as '课程号', 
		cname as '课程名', score as '分数'
		
from s,c.sc

where 
	s,sno = sc,sno and c.cno = sc.cno
	
    
with check option
```



### 5.2 删除视图

```sql
drop view    st_Details_View
```





### 5.3 修改视图

 ```sql
alter view   st_Details_View
as

select  s.sno as ‘学号’, sname as '姓名', 
		sage as '年龄', c,cno as '课程号', 
		cname as '课程名', score as '分数'
		
from s,c.sc
where 
	s,sno = sc,sno 
	and c.cno = sc.cno 
	and sc.score>=80
	
	

 ```





## 6. 索引

### 6.1 创建索引

```sql
create unique clusered index    PK_index
on sc(sno,cno)

with(
	pad_index = on,
    filefactor = 10,
    drop_existing = on
    

)

```



### 6.2 删除索引

```sql
drop index 表名.索引名
```



### 6.3 修改索引

```sql
-- 重新生成 索引
alter index 索引名
on 表名
rebuild


-- 重新组织 索引
alter index 索引名
on 表名
reorganize


-- 禁用 索引
alter index 索引名
on 表名
disable
```









## 7. 存储过程

### 7.1 创建 存储过程

```sql
create procedure ProSeEmp
as
select employee
where sex='女'


-- 创建带输入参数+输出参数的存储过程
--返回指定empid的员工所在的dep
create procedure ProDep
	@Empid int , @Dep varchar(30) output
as 
select @Dep=DepetName
from dept join emp 
	on dep.depid = emp.depid
where empid = @empid	


-- 执行
execute ProDep 参数
```



### 7.2 执行 存储过程

```sql
execute 存储过程名
```





### 7.3 删除 存储过程

```sql
drop procedure 存储过程名
```





### 7.4 修改 存储过程

```sql
alter procedure proSeEmp
as
	select empName,salary,depid
	from emp
```





## 8. 触发器

### 8.1 创建 触发器

```sql
create trigger 触发器名
on 表名
for/instead of/after  [insert/update/delete]
as ...sql语句




-- 创建触发器实例：数据 进行更新操作后，触发器启动：打印修改行数
create trigger emp_Tri
on emp
after update
as
	declare @c int
	select @c=@@rowcount
	print '一共修改了'+char(48+@c)+'行'

```

### 8.2 删除 触发器

```sql
drop trigger 触发器名
```



### 8.3 修改 触发器

```sql
alter trigger 触发器名
on 表名
for / instead of / after   [insert/ update / delete]
as ...sql语句

```





## 9. 数据库安全性

### 9.1 创建账户

步骤：

-   选中 某个数据库 =》展开，选中  ‘安全性’  下的 ‘用户’  =》右键
-   新建用户
-   输入、选择：用户名，登录名，默认架构
-   确定





### 9.2 授予权限

步骤：

-   选中 某个数据库 =》 右键 =》 属性
-   权限 =》 点击 ‘搜索’ 按钮 =》浏览 =》选中 某个 用户
-   勾选 所需权限
-   确定

‘、



### 9.3 备份

备份设备

步骤：

-   选中 ”服务器对象“ =》 展开 =》选中 ”备份设备 “ =》右键 
-   ”新建备份设备 “   =》输入 一个备份名 
-   确认









## 10. T-SQL 批处理

### 10.1 变量：

- **局部变量：** 以@符号开头，先定义后使用。
如：@age
- **全局变量：** 以@@开头，由系统控制，用户只能读取，不能修改。如：@@VERSION



**变量的声明 和 赋值：**

```SQL
DECLARE @age int	//declare 声明变量 @age,该变量的类型为 int


SET @age = 12		//给变量赋值


SELECT @age = stuAge FROM Student WHERE Sno='S001' 
//将查询到的1个值赋给变量
```







### 10.2 流程控制：

begin和and之间的语句是1个整体，类似C语言中的大括号的作用。
```sql
declare @price varchar(2) =10

begin 
	print '价格为：'+@price
end
```







### 10.3 IF...ELSE 语句块

if... else
```sql
declare @price int =10

if @price > 5
	print @price
else
	print '@price<=5'
```







### 10.4 CASE...END 语句块

case...end语句相当于C语言中的 switch 语句

```sql
// 第一种
declare @price int = 10

declare @price2 int = case 
	when @price=10 then 11
	when @price=20 then 21
	when @price=30 then 31
end

print @price2




//第二种
declare @price int =10

declare @price2 int = case @price
	when 10 then 11
	when 20 then 21
	when 30 then 31
end

print @price2
```






### 10.5 while 循环：

while语句中，如果循环体有多条语句，需要结合begin...end，while循环中的变量变化需要有 select 关键字修饰

```sql
// 打印 1~9
declare @num int = 1

while @num < 10
	begin	
		print @num
		select @num = @num+1
	end

```







### 10.6 WAITFOR 语句：

waitfor语句用于：延迟执行



第一种：

```sql

declare @num int = 1

// 延迟 5秒 执行：打印1~9
waitfor delay '00:00:05'

while @num < 10
	begin
			
		print @num
		select @num = @num+1
	end

```



第二种：

```sql
declare @num int = 1

// 在 今天的13：01：55 执行以下操作：打印 1~9
waitfor time '13:01:55'

while @num < 10
	begin
			
		print @num
		select @num = @num+1
	end


```
