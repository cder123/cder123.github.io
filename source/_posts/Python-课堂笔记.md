---
title: Python—课堂笔记
tag: Python3
categories:
  - 后端
  - Python3
---



<h1>Python-笔记</h1>

[toc]





# 一、Python语言基础



**注释：**

在Python 中单行注释使用`# 注释内容`，多行注释使用三引号`“"” 注释内容 """`。



**缩进：**使用四个空格。



**导包：**

- `import random`
- `import random as rd`
- `from request import requests`



换行写代码：

```python
str = " 这是第1行代码 \
	这是第2行代码
"
```







## 1、标准输入输出



### 1.1 输出 `print()`

Python 属于`print()`函数输出，`input()`函数输入。

```python
def main():
    name = input("请输入字符串")
    print(name)


if __name__ == '__main__':
    main()
```



格式化-输出：

```python
# 这是格式化输出：21，张三，32.88
print("这是格式化输出：{0}，{1}，{2:.2f}".format(21,"张三",32.878))
print("这是格式化输出：%d，%s，%.2f"%(21,"张三",32.878))
```



指定分隔符-输出：

```python
# 原始输出：
print("张三","李四","王五",sep="-") # 张三,李四,王五

# 指定分隔符为“-”号 
print("张三","李四","王五",sep="-") # 张三-李四-王五
```





居中-输出：

```python

# *************信息如下*************
print("信息如下".center(30,"*")) # 30表示字符总数，“*”表示填充符号
```





### 1.2 输入`input()`



`input()`函数输入的是字符串，可以使用`eval()`函数将输入的字符串转化为去掉双引号的类型。



```python
# 输入“张三” =转化=》“张三”
# 输入 120 =转化=》120
name = eval(input("请输入:")) 
print(name)
```





## 2、查看数据类型



Python 中的变量在使用的时候，不需要像C、Java那样使用数据类型来声明。使用`type()`函数可以查看数据的数据类型。



8 种数据类型：int、float、str、list列表、turple元组、dict字典、set集合、bool。

```python
# <class 'int'>
type(23)  


# <class 'float'>
type(2.14)


# <class 'str'>
type("张三")  


# <class 'list'>
type([2,5,8,10])  


# <class 'tuple'>
type((2,5,8,10))


# <class 'dict'>
type({"张三":123,44:"李四"})


# <class 'set'>
type({"张三","李四"})


# <class 'bool'>
type(True)

```





## 2、变量与常量



Python 中没有专门的常量，一般将**变量名大写**来区分变量和**常量**。

- 变量：`my_age`
- 常量：`MY_AGE`





**变量的命名规则**：字母、数字、下划线，严格区分大小写，不能为保留字。



一般使用**小驼峰法****或者**下划线法**命名变量：

- 小驼峰法：`myName`、`numOfStu`
- 下划线法：`my_name`、`num_of_stu`



变量的**存储**：

> Python的内存空间分为：`栈、堆`。
>
> - `栈`：存放变量名
> - `堆`：存放内容
>
> 
>
> 变量在内存中存储：
>
> - `id`（相当于内存地址）：指向内存的起始地址。
> - `type`（相当于数据类型）：指向数据类型（变量本身没有数据类型，只能指向Python的数据类型）。
> - `value`（数据的值）：指向数据的值。 
>
> 



变量的**比较**（`==` 与`is`）：

```python
# "=="运算，比较的是值，两个变量的值相等，则type相同，但id不一定相同，返回True
# “is”运算，比较的是地址，两个变量的地址相等，则值和type一定相同，返回True
age = 12
myAge=12
age == myAge # True


x = 2
y = 3
id(x)	# 140728694806208
id(y)	# 140728694806240
x == y	# False
x is y	# False


a = "张三"
b = a
a == b	# True
a is b  # True
```





Python 中，变量地址是以内容为基准的，只要内容变化，地址一定变化：

```python
x = 1
id(x)	# 140728694806176

x = 2
id(x)	# 140728694806208

```







## 3、运算符



Python 中，<font color=red>没有</font> C、Java中的自增（`i++`）、自减运算（`i--`）。



### 3.1 算术运算符

算术运算符有`+`(加)、`-`(减)、`*` (乘)、`/`(除)、`%`(模)、`**`幂、`//`整除



![image-20220228110242019](https://gitee.com/cder123/note-drawing-bed-01/raw/master/image-20220228110242019.png)



### 3.2 赋值运算符



| 运算符 |              实例               |
| :----: | :-----------------------------: |
|  `=`   |            `a = 10`             |
|  `+=`  |  `a += 10` 等价于 `a = a + 10`  |
|  `-=`  |  `a -= 10` 等价于 `a = a - 10`  |
|  `*=`  |  `a *= 10` 等价于 `a = a * 10`  |
|  `/=`  |  `a /= 10` 等价于 `a = a / 10`  |
| `//=`  | `a //= 10` 等价于 `a = a // 10` |
| `**=`  | `a **= 10` 等价于 `a = a ** 10` |
|  `%=`  |  `a % 10` 等价于 `a = a % 10`   |



```python
a = 10
a += 10     # 20

a = 10
a -= 10 	# 0

a = 10
a *= 10 	# 100

a = 10
a /= 10     # 1.0

a = 10
a %= 3      # 1

a = 10
a **= 2     # 100

a = 10
a //= 4     # 2
```









### 3.3 逻辑运算符



```python
True and True  # True
True and False # False

True or True  # True
True or False # True


not True  # False
not False # True

```









### 3.4 位运算符



![image-20220228113906469](https://gitee.com/cder123/note-drawing-bed-01/raw/master/image-20220228113906469.png)







### 3.5 成员运算符



```python

"张三" in ["张三","李四"] 		# True

"张三" not in ["张三","李四"] 	# False


```





### 3.5 身份运算符





```python
a = "张三"

a is  "张三"		# True

a is not  "张三"	# False

```







### 3.6 运算符的优先级



![image-20220228114744950](https://gitee.com/cder123/note-drawing-bed-01/raw/master/image-20220228114744950.png)









## 4、 流程控制



### 4.1、 选择结构



单个 if 语句：

```python
if 条件:
    语句
    语句2......    
```



 if···else 语句：

```python
if 条件:
    语句
    语句2......
    
else:
    语句
    语句2......    
```



if···elif···else 语句：

```python
if 条件:
    语句
    语句2......
    
elif 条件:
    语句
    语句2......
    
else:
    语句
    语句2......
```





### 4.2、 循环结构



循环控制：`break`、`continue`、`pass`





while 循环：

```python
while 条件:
    语句1
    语句2......
```



for 循环：

```python
for 变量 in 可迭代的对象:
    语句1
    语句2......  
```



实例：

```python
for i in range(0,3,1):	# range(start,end,step)
    print(i)
    
# 0 1 2
```





while ···else 循环：【循环结束时，执行else内的语句】

```python
while 条件:
    语句1
    语句2......
else:
    语句1
    语句2......
```





for···else 循环：【循环结束时，执行else内的语句】



```python
for 变量 in 可迭代的对象:
    语句1
    语句2......  
else:
    语句1
    语句2......  
```









## 5、 基本数据类型



创建数据：`变量名 = 值`

删除数据：`del 变量名`





### 5.1、数值类型

不可变的类型

数值类型包括：

- `int`：有符号
- `float`：可以用科学计数法（`+2.5E3`代表`2.5*10^3`）
- `complex`





八进制：`0o37`

十六进制：`0xA0F`





类型转化函数：

- `int()`：int(2.14)转化为了2
- `float()`：float(2)转化为了2.0





数学常数【`import math`】：

- `math.pi`
- `math.e`





#### 5.1.1 数学函数

![image-20220228121646183](https://gitee.com/cder123/note-drawing-bed-01/raw/master/image-20220228121646183.png)







#### 5.1.2 三角函数

![image-20220228121757370](https://gitee.com/cder123/note-drawing-bed-01/raw/master/image-20220228121757370.png)

![image-20220228121817160](https://gitee.com/cder123/note-drawing-bed-01/raw/master/image-20220228121817160.png)





#### 5.1.3 随机函数

导入`import random`包

![image-20220302102847939](https://gitee.com/cder123/note-drawing-bed-01/raw/master/image-20220302102847939.png)



```python
import random

arr = [1,3,5,7]
random.shuffle(arr) # 打乱次序，arr = [3,7,5,1]

```





### 5.2、字符串类型



Python 中只有字符串类型，没有字符类型，字符会被当做长度为 1 的字符串。



切片【`变量名[start : end+1：步长]`】：

```python
str = "hello world"

# 当切片的起始下标为0时，可省略起始下标
# 当切片的最后一个元素为最后一个时，可省略终止下标

str[0:5]	# hello
str[:5]		# hello
str[::]		# hello world
str[::-1]	# dlrow olleh
```





字符串的特殊运算符：

|  运算符  |            说明            |              实例               |
| :------: | :------------------------: | :-----------------------------: |
|   `+`    |            拼接            | `“张三”+“12岁"` =》`“张三12岁"` |
|   `*`    | 重复（其他数据类型也适用） |     `张三*2` =》`张三张三`      |
|   `[]`   |            取值            |        `str[0]` =》`张`         |
|  `[:]`   |            切片            |      `str1[0:2]`=》`张三`       |
|   `in`   |    判断字符串是否是子串    |   `”张“ in ”张三“` =》`True`    |
| `not in` |   判断字符串是否不是子串   | `”张“ not  in ”张三“`=》`False` |
|   `r`    |     原样输出（不转义）     |      `r"张\t三"` =》`张三`      |



字符串的格式化：（实例可见1.1小节的print函数）

```python
"格式字符串"%（参数1,参数2）
```



| 格式化符号 | 说明         |
| ---------- | ------------ |
| `%d`，`%i` | 有符号整数   |
| `%u`       | 无符号整数   |
| `%o`       | 八进制整数   |
| `%x`       | 十六进制整数 |
| `%f`       | 浮点数       |
| `%e`       | 指数         |
| `%s`       | 字符串       |



常用字符串方法（参数均可选）：

- `count(str,start,endLen)`【常用】

- `center(str,start,endLen)`

- `encode(encoding="UTF-8",errors="strict | ignore | replace")`
- `decode(encoding="UTF-8",errors="strict | ignore | replace")`
- `endWith(suffix,start,endLen)`【常用】
- `连接符.join(列表)`【常用】
- `split(str,num)`：分割字符串为 num+1 个子串。【常用】
- `splitLines()`：分割字符串（按回车、换行符）
- `ljust(width,str)`：左对齐，不够宽度时用str填充
- `rjust(width,str)`：右对齐，不够宽度时用str填充
- `find(str,start,,endLen)`：返回目标子串的下标，没找到则返回-1。
- `maketrans(输出表，输出表)`：创建翻译表
- `translate(翻译表，删除表)`：过滤。需要配合`maketrans(输出表，输出表)`使用







### 5.3、列表



列表也就是一个数组，也可以当作栈、队列来用。





```python
# 列表的创建：
list_1 = ['a','b','c',0,1,2]


# 访问列表元素
list_1[0] 		# 结果为'a'
list_1[0:3] 	# 结果为"a",'b','c'


# 修改列表元素
list_1[0] = 'zs'


# 删除列表元素
del list_1[0:3]		# 按索引下标 删除
list_1.remove('c')  # 按索元素值 删除


# 合并
arr1 = ['a','b']
arr2 = [1,2]
arr3 = arr1 + arr2 	# ['a','b',1,2]


# 截取
list_1 = ['a','b','c',0,1,2]
list_1[0:3]		#  ['a','b','c']
list_1[2:]		#  ['c',0,1,2]
list_1[-1]		#  2


# 生成重复元素
arr1 = ['a','b']
arr1*2				# ['a','b','a','b']


```



常用函数：

|         函数          | 说明                       | 实例                    |
| :-------------------: | :------------------------- | :---------------------- |
|      `len(列表)`      | 求列表中元素的个数         | `len([1,2,3])`          |
|      `max(列表)`      | 求最大值                   | `max([1,2,3])`          |
|      `min(列表)`      | 求最小值                   | `min([1,2,3])`          |
|     `list(元组)`      | 将元组转化为列表           | `list((1,2,3))`         |
|     `index(元素)`     | 返回元素的下标             | `arr.index(元素)`       |
|     `count(元素)`     | 统计元素出现的次数         | `arr.count(3)`          |
|    `append(元素)`     | 在末尾添加元素             | `arr.append(4)`         |
|    `extend(列表)`     | 在末尾添加列表中的所有元素 | `arr.extend(['a','b'])` |
|  `insert(下标,元素)`  | 在指定位置插入元素         | `arr.insert(0,'zs')`    |
|        `pop()`        | 删除末尾的一个元素         | `arr.pop`               |
|      `reverse()`      | 反转列表                   | `arr.reverse()`         |
| `sort(reverse=False)` | 排序，默认升序             | `arr.sort()`            |



列表生成式：

```python
# 格式：
#	表达式 for 变量 in 范围 if表达式
# 语义：在范围内，如果符合if表达式，则执行表达式，并将表达式的结果组成一个列表


arr = x**2 for x in range(4) 			# [0,1,4,9]

arr = x**2 for x in range(4) if x%2==0	# [0,4]

```









### 5.4、元组



元组就是无序的列表



```python
tp = (1,2,3)

# 求个数
len(tp) # 3


# 合并
tp = (1,2,3) + (4,5,6) # (1,2,3,4,5,6)


# 重复
tp = (1,2)*2	# (1,2,1,2)


# 是否是成员
1 in (1,2,3) # True


```





### 5.5、字典



字典就是哈希表



```python
dic = {'姓名':'zs','年龄':20}

# 获取元素，不存在，返回None
dic["姓名"] 		# 张三
dic.get("姓名")	# 张三


# 修改元素
dic["姓名"] = 'ls'
dic.update({"姓名":'ls'})


# 删除元素
del dic["姓名"]
dic.pop("姓名")


# 随机删除一个键值对
dic.popitems()


# 删除字典
del dic


# 清空元素
dic.clear()


# 浅拷贝
dic1 = dic.copy()


# 返回所有键的列表
dic.keys()


# 返回所有键值元组的列表
dic.items()


```





### 5.6、集合



集合是无序的，元素不可变。



集合分为：

- set：可变集合
- frozenset：不可变集合



```python
set1 = {1,2,5,4,8,0}
set2 = set([6,1,2,5])


# 转化为集合
set(list_1)
set(tuple_1)
set(dic_1)


# 添加
set1.add(9)
set1.update([15,9,16,50])


# 删除
set1.remove(50)
set1.pop() # 转化前为有序，则删除左边的元素；转化前为无序，则随即删除


# 交集
set1.intersection(set2)


# 并集
set1.union(set2)


# 差集
set1.difference(set2)


# 对称差集
set1.symmertic_difference(set2)


# 判断父集合
set1 >= set2


# 判断子集合
set1 <= set2

```







### 5.7、深浅拷贝



引用类型-赋值：

> 对于可变类型，完全数据共享（改变一个，另一个也变）
>
> 对于不可变类型，不数据共享（改变一个，另一个不变）

```python
arr1 = [1,2]
arr2 = arr1 	#  [1,2]

arr1[1] = 3		# [1,3]
arr2			# [1,3]

```





浅拷贝（部分数据共享）：

> 两层列表，浅拷贝只能拷贝第一层，第二次是数据共享的。

```python
from copy import *

arr1 = [1,2,[3,4]]
arr2 = copy(arr1)		#  [1,2,[3,4]]

arr1[0] = 'a'			# [a,2,[3,4]]
arr2[0]					# [1,2,[3,4]]


arr1[2][0] = 'b'		# [1,2,['b',4]]
arr2					# [1,2,['b',4]]
```





深拷贝：

> 两层列表，深拷贝完全拷贝数据，之后互不干扰

```python
from copy import *

arr1 = [1,2,[3,4]]
arr2 = deepcopy(arr1)		#  [1,2,[3,4]]

arr1[0] = 'a'				# [a,2,[3,4]]
arr2[0]						# [1,2,[3,4]]


arr1[2][0] = 'b'			# [1,2,['b',4]]
arr2						# [1,2,['b',4]]
```



