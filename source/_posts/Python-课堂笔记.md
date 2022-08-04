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

<img src="https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220228110242019.png"/>





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



<img src="https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220228113906469.png"/>







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



<img src="https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220228114744950.png"/>









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

<img src="https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220228121646183.png"/>







#### 5.1.2 三角函数

<img src="https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220228121757370.png"/>



<img src="https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220228121817160.png"/>



#### 5.1.3 随机函数

导入`import random`包

<img src="https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220228122137965.png"/>



```python
import random

arr = [1,3,5,7]
random.shuffle(arr) # 打乱次序，arr = [3,7,5,1]

```



<img src="https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220302102847939.png"/>





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







# 二、Tkinter



## 1、Tkinter 基本使用



- https://zhuanlan.zhihu.com/p/75872830

- [Tkinter教程（非常详细） (biancheng.net)](http://c.biancheng.net/tkinter/)





### 1.1、主窗口



```python
# Tkinter 教程：https://zhuanlan.zhihu.com/p/75872830

from tkinter import *


def main():
    # 创建一个主窗体的对象
    win = Tk()
    
    # 设置主窗体的左上角的标题
    win.title("主窗体")
    
    # 设置主窗体的大小
    win.geometry("400x300")
    
    # 让主窗体持续显示
    win.mainloop()
    

if __name__ == '__main__':
    main()
```





### 1.2、标签



```python
# Tkinter 教程：https://zhuanlan.zhihu.com/p/75872830

from tkinter import *


def main():
    win = Tk()
    win.title("主窗体")
    win.geometry("400x300")

    # 创建一个标签对象，指定主窗体、显示文本、背景色、前景色（字体颜色）、字体及字体大小
    lbl = Label(win,text="这是标签",bg="blue",fg="red",font=("Arial Bold", 25))    
    
    # 指定标签的存放位置
    # ！！注意: 只有调用该方法后标签才能显示
    lbl.grid(row=0,column=0)
    
    win.mainloop()


if __name__ == '__main__':
    main()

```





### 1.3、按钮





```python
# Tkinter 教程：https://zhuanlan.zhihu.com/p/75872830

from tkinter import *


def main():
    win = Tk()
    win.title("主窗体")
    win.geometry("400x300")

    lbl = Label(win,text="这是标签",bg="blue",fg="red",font=("Arial Bold", 25),)
    lbl.grid(row=0,column=0)

	# 定义一个函数（用作按钮的事件）
    def hello():
        lbl.configure(text="按钮被点击了！")

   	# 创建按钮，绑定点击事件
    btn = Button(win,text="点击",bg="orange",command=hello)
    btn.grid(row=1,column=2)

    win.mainloop()




if __name__ == '__main__':
    main()

```





### 1.4、输入框



```python
# Tkinter 教程：https://zhuanlan.zhihu.com/p/75872830

from tkinter import *


def main():
    win = Tk()
    win.title("主窗体")
    win.geometry("400x300")

    lbl = Label(win,text="这是标签",bg="blue",fg="red",font=("Arial Bold", 25),)
    lbl.grid(row=0,column=0)

    def hello():
        lbl.configure(text="按钮被点击了！")
    btn = Button(win,text="点击",bg="orange",command=hello)
    btn.grid(row=1,column=2)

    
    # 创建一个输入框
    entry = Entry(win,width=30)
    # 设置放置的位置
    entry.grid(row=1,column=0)

    win.mainloop()

if __name__ == '__main__':
    main()

```



输入框+按钮+标签-案例：

(1)效果：

<img width="400px" src="https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220801085020401.png" />

(2)代码：

```python
# Tkinter 教程：https://zhuanlan.zhihu.com/p/75872830

from tkinter import *


def main():
    # 主窗体
    win = Tk()
    win.title("主窗体")
    win.geometry("400x300")

    # 标签
    lbl = Label(win,text="请输入：",bg="white",fg="black",font=("Arial Bold", 16),)
    lbl.grid(row=0,column=0)

    # 标签
    rst_lbl = Label(win,text="结果为：",bg="white",fg="black",font=("Arial Bold", 16),)
    rst_lbl.grid(row=1,column=1)

    # 输入框
    myEntry = Entry(win,width=30,)
    myEntry.focus() # 自动聚焦
    myEntry.grid(row=0,column=1)

	# 按钮及其点击事件
    def hello():
        res = "结果为："+entry.get()
        rst_lbl.configure(text=res)
    btn = Button(win,text="点击",bg="orange",command=hello)
    btn.grid(row=0,column=2)
    
    win.mainloop()

if __name__ == '__main__':
    main()

```





### 1.5、下拉框



```python
# Tkinter 教程：https://zhuanlan.zhihu.com/p/75872830

from tkinter import *
from tkinter.ttk import Combobox

def main():
    win = Tk()
    win.title("主窗体")
    win.geometry("400x300")

    # 创建一个下拉框
    combo = Combobox(win)
    
	# 将数据元组添加到下拉框
    # ！！注意：'values'是固定写法，不能改
    combo['values'] =(1,2,3,"下拉框")
    
    # 设置下拉框的默认选中值
    combo.current(0)
    # 获取当前被选中的值
    curSelectVal = combo.get()
    combo.grid(row=2,column=1)


    win.mainloop()




if __name__ == '__main__':
    main()

```



### 1.6、复选框



```python
# Tkinter 教程：https://zhuanlan.zhihu.com/p/75872830

from tkinter import *


def main():
    win = Tk()
    win.title("主窗体")
    win.geometry("400x300")

    # 创建选中状态
    checkedState = BooleanVar() # 也可用 IntVar()，设置状态值时用0或1
    checkedState.set(True)
    
    # 创建复选框
    chk = Checkbutton(win,text="请选择",var=checkedState)
    chk.grid(row=3,column=0)
    
    win.mainloop()

if __name__ == '__main__':
    main()

```



### 1.7、单选框



```python
# Tkinter 教程：https://zhuanlan.zhihu.com/p/75872830

from tkinter import *


def main():
    win = Tk()
    win.title("主窗体")
    win.geometry("400x300") 

    
    # 创建选中值的对象
    selectValObj = StringVar()
    # 设置默认的选中值
    selectValObj.set("male")
    
    
    # 创建被选中时，点击按钮调用的函数
    def choosed():
        print("单选框被点击了！",end=" ")
        print(", 当前的选中的值为："+selectValObj.get())	

        
    # 创建2个单选框
    radio1 = Radiobutton(win,text="男",value="male",variable=selectValObj,command=choosed)
    radio2 = Radiobutton(win,text="女",value="female",variable=selectValObj,command=choosed)
    radio1.grid(row=4,column=1)
    radio2.grid(row=4,column=2)

    
    win.mainloop()

if __name__ == '__main__':
    main()

```





### 1.8、文本域



```python
from tkinter import *
from tkinter import scrolledtext


def main():
    win = Tk()
    win.title("主窗体")
    win.geometry("400x300")

    # 创建文本域
    txt = scrolledtext.ScrolledText(win,width=30,height=10)
    txt.grid(row=5, column=0)
    # 插入文本（）
    txt.insert(INSERT, "在此插入文本！")
    # 删除所有文本
    txt.delete(1.0,END)


    win.mainloop()

if __name__ == '__main__':
    main()

```





### 1.9、弹窗



```python
from tkinter import *
from tkinter import messagebox

def main():
    win = Tk()
    win.title("主窗体")
    win.geometry("400x300")

    def getInfo():
        messagebox.showinfo("弹窗标题", "弹窗内容")
    btn2 = Button(win,text="开始弹窗",command=getInfo)
    btn2.grid(row=5,column=0)


    win.mainloop()

if __name__ == '__main__':
    main()

```



### 1.10、数字输入框



相当于HTML标签中的 `<input type="number" />`



```python
# Tkinter 教程：https://zhuanlan.zhihu.com/p/75872830

from tkinter import *


def main():
    win = Tk()
    win.title("主窗体")
    win.geometry("400x300")

    # 创建数字输入框
    # sp = Spinbox(win,from_=1,to=100,width=10)
    
    # 创建数字输入框,指定点击上下箭头时切换到的值
    # sp = Spinbox(win,values=(1,3,5,10),width=10)
    # sp.grid(row=0,column=2)
    
    
    # 指定默认值
    defaultVal = IntVar()
    defaultVal.set(25)
    sp = Spinbox(win,from_=1,to=100,width=10,textvariable=defaultVal)
    sp.grid(row=0,column=2)


    win.mainloop()

if __name__ == '__main__':
    main()

```





### 1.11、进度条



```python
# Tkinter 教程：https://zhuanlan.zhihu.com/p/75872830

from tkinter import *
from tkinter.ttk import Progressbar


def main():
    win = Tk()
    win.title("主窗体")
    win.geometry("400x300")

    # 创建进度条的样式
    style1 = ttk.Style()
    style1.theme_use('default')
    style1.configure("black.Horizontal.TProgressbar", background='black')

    
    # 创建进度条
    bar = Progressbar(win,length=200,style="black.Horizontal.TProgressbar")

    # 进度条到70%
    bar['value'] = 70
    

    bar.grid(row=0,column=2)

    win.mainloop()

if __name__ == '__main__':
    main()

```





### 1.12、文件对话框



```python
import os
from tkinter import *
from tkinter import filedialog

def main():
    win = Tk()
    win.title("主窗体")
    win.geometry("400x300")

    # 单个文件
    fileName = filedialog.askopenfilename()


    # 单个文件,限制文件类型
    fileName1 =  filedialog.askopenfilename(filetypes=(("文本文件","*.txt"),("所有文件","*.*")))


    # 打开当前程序的所在目录
    fileName2 =  filedialog.askopenfilename(initialdir=os.path.dirname(__file__))


    # 多个文件
    fileNames =  filedialog.askopenfilenames()


    # 打开目录
    dirName = filedialog.askdirectory()


    win.mainloop()

if __name__ == '__main__':
    main()

```





## 2、案例



### 2.1、随机人名生成工具



该工具只完成了两个字的名字的生成。



<img width="480" src="https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220801133456930.png" />



```python
from tkinter import *
from tkinter.ttk import *
import random as rd
from copy import *

# 名
lastNameList = ['而', '何', '乎', '乃', '其', '且', '若', '所', '为', '焉', '也', '以', '因', '于', '与', '则', '者', '之']




def main():
    win = Tk()
    win.config(bg='#88CDD8')
    win.title("主窗口")
    win.geometry("600x500")
    frame = Frame(win, relief=SUNKEN, borderwidth=2, width=450, height=250)
    frame.pack(side=TOP, fill=BOTH, expand=1)

    title = Label(frame, text="随机人名生成器", font=("YaHei", 18))
    title.place(x=200, y=10)

    firstNameInputTip = Label(frame, text="请选择姓氏：", font=("YaHei", 12))
    firstNameInputTip.place(x=80, y=80)
    firstNameCombo = Combobox(frame, width=5)
    firstNameCombo.place(x=200, y=80)
    firstNameCombo['values'] = ("赵", "钱", "孙", "李", "周", "吴", "郑", "王")
    firstNameCombo.current(0)

    sexTip = Label(frame, text="请选择性别：", font=("YaHei", 12))
    sexTip.place(x=290, y=80)
    selectVal = StringVar()
    selectVal.set("male")
    maleRadio = Radiobutton(frame, text="男", value="male", variable=selectVal)
    femaleRadio = Radiobutton(frame, text="女", value="female", variable=selectVal)
    maleRadio.place(x=400, y=80)
    femaleRadio.place(x=450, y=80)

    countTip = Label(frame, text="请选择生成个数：", font=("YaHei", 12))
    countTip.place(x=80, y=110)
    countVal = IntVar()
    countVal.set(5)
    sp = Spinbox(win,from_=1,to=100,width=10,textvariable=countVal)
    sp.place(x=220,y=112)



    resListBox = Listbox(frame,selectmode = MULTIPLE)
    resListBox.place(x=80,y=140)

    resCountLbl = Label(frame,text="总计（个）：0"+", 性别："+selectVal.get())
    resCountLbl.place(x=250,y=150)



    def getName(firstName, lastNameList=lastNameList, count=countVal.get()):
        if count<1:
            return None
        resListBox.delete(0,count)
        rawList = deepcopy(lastNameList)
        res = set([])
        idxList = rd.sample(range(0, len(rawList)), count)
        for id in idxList:
            res.add(firstName + rawList[id])
        for i, item in enumerate(res):
            resListBox.insert(i,item)
        resCountLbl.configure(text="总计（个）：" + str(resListBox.size())+", 性别："+selectVal.get())

    def clearName():
        resListBox.delete(0,resListBox.size())
        resCountLbl.configure(text="总计（个）：" + str(resListBox.size())+", 性别："+selectVal.get())

    createFullNameBtn = Button(frame,text="随机生成",command=lambda: getName(firstNameCombo.get(),lastNameList=lastNameList,count=countVal.get()))
    createFullNameBtn.place(x=340,y=110)

    clearFullNameBtn = Button(frame,text="清空",command=clearName)
    clearFullNameBtn.place(x=440,y=110)



    win.mainloop()


if __name__ == '__main__':
    main()

```









# 三、PyInstaller





在**纯英文目录**下，安装：`pip install pyinstaller`



```bash
pyinstaller -D -p ./ -i ./logo.ico ./myUtils.py --noconsole

-D:打包成多个文件
-p：指定python安装包路径
-i：指定图标，到网上下载一个图标,保存为logo.ico文件，
myUtils.py：要打包的文件

要打包的文件myUtils.py 与 要指定的图标logo.ico 必须放在同一个目录下
-D与-F一一对应，-F是打包成一个单独的文件。
最后一排加上--noconsole，就是无窗口运行。
```

