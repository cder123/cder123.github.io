---
title: Windows批处理命令—入门
tag: Windows
categories:
  - [后端,操作系统,Windows使用]
---



# Windows批处理命令—入门

@[toc]

### 1. 解决 cmd  中文乱码
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203130339914.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203130429604.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203130651236.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)

cmd中输入：`chcp 65001` 将 编码改为 UTF-8
![在这里插入图片描述](https://img-blog.csdnimg.cn/202012031309099.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)

若以上方法无效，则：

> bat文件右键用“ 编辑”  打开，
> 另存为时，UTF-8保存为ANSI 格式。即可解决运行是乱码问题



###  2. 批处理文件

批处理文件 就是 将多条DOS命令放在一个`.bat` 文件中，运行时，依次执行文件中编写的 DOS命令
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203133633685.png)



### 3. 切换目录

cd【即：change directory 更改当前目录】
- 进入驱动器，如：`d:`
- 进入文件夹，如：`cd d:\abc\Test.txt`

`cls`：清屏
`set` ：查看环境变量



### 4. 获取当前 批处理文件 所在的目录

“d” ： Drive的缩写，即为驱动器
“p”：  Path缩写，即为路径，目录
`cd %~dp0 `：进入批处理所在目录
`cd %~dp0bin\ `：进入批处理所在目录的bin目录
```bash
title 批处理演示
:: 你当前的位置是 %time%
echo   你当前的位置是：%~dp0
pause
```


命令解析： 

- `::` 表示后面内容为批处理文件中**注释**，相当于命令`rem`
- `echo` : 命令是什么，就在控制台打印什么
- `pause`： 中断批处理文件，并等待用户输入任意字符
- `title`：设置当前cmd 窗口的标题
- `%time%`：获取当前的时间





`%cd%`  和 `%~dp`的区别：

 1. 当 批处理文件中 **没有调用 另外的文件夹**中的批处理文件时，两者取出的值**一样**
 2. 如果有调用别的目录下的批处理文件，则 `%cd%`仍然是当前批处理文件的目录，但`%~dp0`变为**被调用的**批处理文件所在的**目录** 





**小结：** 

 1. `%~dp0`：当前正在执行的批处理文件所在的目录
 2. `%cd%`： 当前主文件的目录，一般不变







**示例：**

```bash
echo   你当前的位置是：%~dp0
pause

echo   你当前的工作目录是：%cd%
pause
```









### 5. 杀进程：

```bash
taskkill -f /im notepad.exe
echo 杀死记事本进程
pause
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203141751336.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)







### 6. 获取帮助

例如：`taskkill  /?`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203142030518.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)







### 7. 常用命令

```bash
//查看系统版本的命令的语法【/？】
ver /?

//启动1个cmd实例
cmd /?

//查看环境变量
set /?

//注释，相当于::
rem /?

//条件判断
if /?

//echo后是什么就打印什么
echo /?

//跳转，可与:标签名 结合形成循环
goto /?

//循环
for /?

//更改批处理文件中可替换参数的位置
shift /?

//调用别的批处理文件
call /?

//显示文本内容
type /?

//查找,find "要找的字符串" 文件名
find /?

//查找,find "要找的字符串" 文件名
findstr /?

//将文件复制到另外的位置，copy src dis
copy /?
```






### 8. 运行的时候 传参

例如：

```bash
//在批处理文件 a.bat中输入下列内容
call b.bat "hello" "haha"
```


命令解释：

- 批处理文件的参数最多10个【0~9】
- 参数0 表示本身，如上面代码中：`call   b.bat    "hello"`表示调用`b.bat`文件，并将”hello“ 作为参数传给`b.bat`文件。在`b.bat`文件内，执行`echo %0`表示打印当前文件名，执行`echo %`表示打印传入的第一个参数【即：”hello“】







### 9. echo

语法：
1. `echo   on/off  `: 打开或关闭提示【不显示输入的过程，只显示执行结果】



![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203144846250.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203145037860.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)





### 10. @符号

@与echo off 相似，用于隐藏 带@的命令的输入 
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203145545385.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203145641769.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)



**在回显的同时将内容覆盖输入文件**

```bash
echo hello  > d:\Test.txt
```




**在回显的同时将内容追加输入文件**

```bash
echo hello  >> d:\Test.txt
```






### 11. goto

实例：
```bash
set a=0

:label_1
if a == 0 
(echo "hello")
else(goto label_1)
```
命令解析：
- 设1个变量`a`，`a`的值为`0`时，打印`hello`，否则跳转到 `label_1`位置重新执行语句