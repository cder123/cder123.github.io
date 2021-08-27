---
title: Fiddler-抓包工具
tag: 软件测试
categories:
  - [软件测试,抓包工具,Fiddler]
  
---

# Fiddler-抓包工具

[toc]







-   [Fiddler抓包工具实战-视频](https://www.bilibili.com/video/BV1c4411c7zH?p=2)



## 0、常用快捷键



>   -   <kbd>ctrl+X</kbd> ：清空所有记录
>   -   <kbd>Ctrl+F</kbd>：查找
>   -   <kbd>F12</kbd>：启动或者停止抓包





## 1、Fiddler 设置



### 1.1 Fiddler 的页面布局

![](https://z3.ax1x.com/2021/08/23/h97y5Q.png)





### 1.2 修改端口

<a href="https://imgtu.com/i/h97Z4J"><img src="https://z3.ax1x.com/2021/08/23/h97Z4J.png" alt="h97Z4J.png" border="0" /></a>



### 1.3 减少干扰包-设置



<a href="https://imgtu.com/i/h9HPGd"><img src="https://z3.ax1x.com/2021/08/23/h9HPGd.png" alt="h9HPGd.png" border="0" /></a>





## 2、工具栏



### 2.1 添加注释

注释的作用：数据包保存为1个文件时，给别人看。

<a href="https://imgtu.com/i/h9HvSs"><img src="https://z3.ax1x.com/2021/08/23/h9HvSs.png" alt="h9HvSs.png" border="0" /></a>



### 2.2 重放

快捷键：（选中数据包）

-   重放一次：<kbd>R</kbd>

-   重放多次：<kbd>shift + R</kbd>，输入重放次数，确认。

<a href="https://imgtu.com/i/h9bHj1"><img src="https://z3.ax1x.com/2021/08/23/h9bHj1.png" alt="h9bHj1.png" border="0" /></a>





### 2.4 删除请求

步骤：选中请求，键盘按 <kbd>delete</kbd>键

删除未选中的请求：选中需要保留的请求， <kbd> shift + delete</kbd>





### 2.5 跳过断点

在下方的状态栏中左部点1次：设置请求断点；点2次，设置响应断点

<a href="https://imgtu.com/i/h9LQRH"><img src="https://z3.ax1x.com/2021/08/23/h9LQRH.png" alt="h9LQRH.png" border="0" /></a>





### 2.6 选择监听的进程

注意：选择进程时，鼠标要按住不放！！

<a href="https://imgtu.com/i/h9OcjA"><img src="https://z3.ax1x.com/2021/08/23/h9OcjA.png" alt="h9OcjA.png" border="0" /></a>



### 2.7 清除浏览器缓存

<a href="https://imgtu.com/i/h9XbGD"><img src="https://z3.ax1x.com/2021/08/23/h9XbGD.png" alt="h9XbGD.png" border="0" /></a>





### 2.8 编码、解码

<a href="https://imgtu.com/i/h9jEss"><img src="https://z3.ax1x.com/2021/08/23/h9jEss.png" alt="h9jEss.png" border="0" /></a>





### 2.9 Fiddler 增加IP列

1.  运行fiddler，菜单，Rules->Customize Rules，打开“Fiddler ScriptEditor”
2.  Ctrl+F查找“static function Main()”字符串，然后在函数体内，添加以下代码：

```c#
FiddlerObject.UI.lvSessions.AddBoundColumn("Server-目的IP", 120, "X-HostIP");
```





## 3、状态栏



### 3.1 命令行：

<a href="https://imgtu.com/i/h9zffK"><img src="https://z3.ax1x.com/2021/08/23/h9zffK.png" alt="h9zffK.png" border="0" /></a>



<a href="https://imgtu.com/i/hCS540"><img src="https://z3.ax1x.com/2021/08/23/hCS540.png" alt="hCS540.png" border="0" /></a>



### 3.2 统计

<a href="https://imgtu.com/i/hC9PJ0"><img src="https://z3.ax1x.com/2021/08/23/hC9PJ0.png" alt="hC9PJ0.png" border="0" /></a>



### 3.3 检测

点击报文中的Raw，可以查看原始报文。

<a href="https://imgtu.com/i/hCCC0H"><img src="https://z3.ax1x.com/2021/08/23/hCCC0H.png" alt="hCCC0H.png" border="0" /></a>







### 3.4 自动响应器

功能：重定向到本地资源、自定义响应报文



2种方式：

-   手动编写

<a href="https://imgtu.com/i/hAZnG8"><img src="https://z3.ax1x.com/2021/08/24/hAZnG8.png" alt="hAZnG8.png" border="0" /></a>

-   GUI下，拖动请求报文到 AutoResponser 中，选中AutoResponser 下的请求，右键 Edit Response







### 3.5 Composer 设计者

Composer 用于自定义请求报文。（简单的接口测试、抓包工具）

<a href="https://imgtu.com/i/hAnkqJ"><img src="https://z3.ax1x.com/2021/08/24/hAnkqJ.png" alt="hAnkqJ.png" border="0" /></a>

scratchpad：拖动左侧的报文到该选项卡的内容页，可以显示请求报文的头部信息





### 3.6 Filter 过滤器



过滤条件为：选择需要展示的内容

<a href="https://imgtu.com/i/hAuc0H"><img src="https://z3.ax1x.com/2021/08/24/hAuc0H.png" alt="hAuc0H.png" border="0" /></a>







## 4、断点

<a href="https://imgtu.com/i/hKNb2q"><img src="https://z3.ax1x.com/2021/08/27/hKNb2q.png" alt="hKNb2q.png" border="0" /></a>







## 5、Https 抓包



![启用HTTPS](https://z3.ax1x.com/2021/08/27/hKa4hj.png)











## 6、Android 设备抓包

(目前未成功)

-   Fiddler勾选HTTPS功能-

-   在 tools => options => connections 中，勾选“允许远程抓包”

-   重启Fiddler、关闭 PC的防火墙。

-   打开安卓设备，将安卓设备与PC接入同一个局域网

-   安卓设备的设置代理（设置=》wifi=>手动配置代理=> PC的ip+8888）

    

