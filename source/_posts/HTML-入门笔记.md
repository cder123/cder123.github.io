---
title: HTML-笔记
tag: HTML
categories:
  - [前端,基础]
  - [前端,HTML]
---

# HTML—入门笔记
[toc]

## 1. html 的基本结构
### 1.1 整体结构【骨架】
```markup
<!DOCTYPE html>
<html lang="en">
<head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
</head>
<body>
        <!--正文-->
</body>
</html>
```
### 1.2 head 标签
通常只在 head标签中放 以下 几种 标签

+  title
+  meta
+  link
+  style
+  script
+  base

### 1.3 页面中文编码的设置

```markup
<meta charset="UTF-8">
```
### 1.4 meta 标签
> meta 标签一般用于存放提供给 搜索引擎爬虫 看的信息，如：页面关键字，页面描述

#### 1.4.1 meta 标签的重要属性 — name属性（页面关键字 + 页面描述）

```markup
<head>    
    <!--网页关键字-->
    	<meta  name="keywords" content="绿叶学习网"/>
    
    <!--网页描述-->
    	<meta  name="description" content="绿叶学习网是一个富有活力的技术学习网站"/>
    
    <!--本页作者-->
    	<meta  name="author" content="helicopter">
    
    <!--版权声明-->
    	<meta  name="copyright" content="本站所有教程均为原创，版权所有，禁止转载。否则将追究法律责任。"/>
</head>
```
#### 1.4.2 meta 标签的重要属性—— http-equiv属性（ 页面编码 +  刷新跳转）

```markup
<head>
	<!-- 定义页面页面编码 -->
		<meta  http-equiv="content-type" content="text/html; charset=utf-8"/>
		
		
	<!-- 定义页面刷新跳转( 6 秒后跳转到百度) -->	
    	<meta  http-equiv="refresh" content="6;url=https:/www.baidu.com"/>
</head>
```
### 1.5 style 标签 —— 用于设置样式
**样式优先级：** [从   高  => 低]
- ! import
- 行内样式
-  style标签内的样式 
- link外联的样式

```markup
<head>        
     <style>
			/* 样式 */
     </style>
</head>
```
### 1.6 script 标签 
用于写 JavaScript 代码（或导入外部 JS文件），为提高页面加载速度，一般放在页面末尾

### 1.7 link 标签—— 用于导入外部的 css 文件

```markup
<head>        
     <!-- 导入 CSS 文件 -->
       <link rel="stylesheet" href="./abc.css">    	
</head>
```
### 1.8 body标签 
用于存放正文

## 2. 文本标签 （常用）
### 2.1 标题标签（h1~ h6）

```markup
<body>
        <h1>hello</h1>
        <h2>hello</h2>
        <h3>hello</h3>
        <h4>hello</h4>
        <h5>hello</h5>
        <h6>hello</h6>
</body>
```
### 2.2  段落（ p ）

```markup
<body>
        <p>这是一个段落，是一个块级元素 </p>      
</body>
```
### 2.3 换行（ br)

```markup
<body>
	 <!-- 换行 -->
       		<span>这是一个行内元素 </span><br/><span>这是一个行内元素 </span>         
        
    <!-- 注意：如果换行写 br标签，则需要将 p 的font-size改为0，否则除了产生换行效果外，还会产生多余的空格 --> 
            <p>
                <span>这是一个段落，是一个行内元素 </span>
                <br/>
                <span>这是一个段落，是一个行内元素 </span>
            </p>        
</body>
```
### 2.4 水平线（hr）

```markup
<body>
	 <!-- 在两个段落间会产生1条水平线 -->
        <p>这是一个段落，是一个块级元素 </p>        
        <hr/>        
        <p>这是一个段落，是一个块级元素 </p>
</body>
```
### 2.5 转义字符（常用部分）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208134802853.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)

### 2.6 div (块标签)和 span （行内标签）
主要存放 元素，只是容器，没有什么效果

### 2.7 文本修饰样式（常用）
+ font-family    ：  字体   【如：Arial, Helvetica, sans-serif  】
+ font-size        ： 字号     【 如： 30px ，1em(1个中文字符大小)  ,1en(1个英文字符大小)  】
+ font-weight  ：  字重（加粗程度）【 100-300 细 ；400-600 中等 ；700-900 粗】
+ font-style:     ：  倾斜程度   【italic  或 oblique】
+ text-indent: ;      缩进           【如： 30px】
+ text-align       :     对齐方式     【 left  ,  right ,center 】
+ text-decoration : 下划线，删除线，上划线  【 underline  ，line-through  ，overline】
+ text-transform: 英文大小写转换          【capitalize，uppercase，lowercase】

```markup
 <span 
 	style="font-size: 30px;
 		   font-weight: 900;
 		   font-family: Arial, Helvetica, sans-serif;
 		   font-style: italic;
 		   text-decoration: overline;"
 		   >这是一个和span
 </span>
```
## 3. 图片、音频、视频
### 3.1 图片（img）
属性：

+ src  : 图片路径
+ alt   : 图片无法显示时的代替文字
+ title : 鼠标悬浮在图片上时跳出的文字
+ width ：宽度（px）
+ height：高度（px）

```markup
<img src="images/car1.jpg" alt="小车图片无法显示" title="小车图片" width="100px" height="100px" />
```

### 3.2 音频+视频（ embed ，html5新增）

```markup
<!-- 插入音频 -->
	<embed src="./media/西班牙舞曲.mp3" width="400px" height="80px"/>

<!-- 插入视频 -->
	<embed src="./media/小苹果.mp4" width="400px" height="80px"/>
```
### 3.3 视频（video）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208135318724.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
```markup
<video width="320" height="240" controls>
    <source src="movie.mp4" type="video/mp4">
    <source src="movie.ogg" type="video/ogg">
    您的浏览器不支持 video 标签。
</video>
```
## 4. 盒子模型
### 4.1 margin-border-padding-content
前三个都支持 **上下左右**  4个方向 单独设置
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208135504124.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)

### 4.2 box-sizing(盒模型的大小设置)：
#### 4.2.1 box-sizing  :border-box   
 > IE盒模型，设置padding时，padding 往里面挤，不会撑大盒子


#### 4.2.2 box-sizing  :content-box    
> 默认的盒模型，设置padding时，padding 往外面撑，会撑大盒子

## 5. 标题的图标
### 5.1  在网页标题左侧显示

```markup
<link rel="icon" href="图标地址" type="image/x-icon">
```

### 5.2  在收藏夹显示图标

```markup
 <link rel="shortcut icon" href="图标地址" type="image/x-icon"> 
```
## 6. 表格
- tr : 行;
- td: 单元格 ； 
- th: 表头单元格;

table的常用属性： 
-  cellpadding：单元格内边距   ；
-  cellspacing：单元格间距
### 6.1 表格-实例1
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208140427220.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)

```markup
<table border="1">
		<caption>表格标题</caption>
		<thead>
			<tr>
				<th>（表头）表格每一列的标题</th>
				<th>（表头）表格每一列的标题</th>
				<th>（表头）表格每一列的标题</th>
			</tr>
		</thead>
		<tbody>
			<tr>
				<td>data</td>
				<td>data</td>
				<td>data</td>
			</tr>
		</tbody>
		<tfoot>
			<tr>
				<td>表尾</td>
				<td>表尾</td>
				<td>表尾</td>
			</tr>
		</tfoot>
</table>
```
**单元格合并（colspan ，x轴单元格减少）**
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020120814051417.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
```markup
<table border="1">
		<tr>
			<td  colspan="2">这是colspan</td>
		</tr>
		<tr>
			<td>data</td>
			<td>data</td>
		</tr>
</table>
```

**单元格合并（rowspan ，y轴单元格减少）**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208140550543.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)

```markup
<table border="1">
		<tr>
			<td  rowspan="2" >这是rowspan</td>
			<td>data</td>
		</tr>
		<tr>			
			<td>data</td>
		</tr>
		<tr>
			<td>data</td>
			<td>data</td>
		</tr>
</table>
```
## 7. 有序列表（ol + li）
无论是 ul 还是 ol  内部第一层必须是 li

- 实例1：默认ol ， 默认type:1
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208140933384.png)
实例2
修改type属性值：
- 1
- a
- A
- i
- I
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208141037242.png)

```markup
    <ol type="A">
         <li>first</li>
         <li>second</li>
         <li>third</li>
    </ol>
```
## 8. 无序列表
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208141407277.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)

```markup
//默认，实心圆
<ul type="disc">
	<li>Coffee</li>
	<li>Milk</li>
</ul>


//空心圆
<ul type="circle">
	<li>Coffee</li>
	<li>Milk</li>
</ul>

//正方形
<ul type="square">
	<li>Coffee</li>
	<li>Milk</li>
</ul>
```
## 9. 定义列表（dl+dt/dd  ,层级列表，会缩进）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208141455567.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)

```markup
<dl>
		<dt>动物</dt>
		<dd>猴子</dd>
		<dd>狗</dd>
</dl>
```
## 10. datalist
常与其他表单控件连用
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208141630583.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
```markup
	<input type="search" list="id1">
	<datalist id="id1">
		<option value="hello"></option>
		<option value="world"></option>
	</datalist>
```
## 11. 常用 表单标签
- 所有表单标签都要写在`<form> </form>`内

```markup
<form >
 单行文本框：<input type="text" name="firstname"> <br>
 
 多行文本框：<textarea rows="10" cols="30">123</textarea> <br>
 
 密码框：<input type="password" name="pwd"> <br>
 
 普通按钮: <input type="button" name="btn" onclick="click_me()"> <br>
 
 单选按钮: <input type="radio" name="sex" value="male">Male <br>
		  <input type="radio" name="sex" value="female">Female <br>
		  
 复选框：   <input type="checkbox" checked name="vehicle" value="Bike">bike <br>
		   <input type="checkbox" name="vehicle" value="Car">car <br>
		   
 提交按钮：  <input type="submit" value="Submit">

 下拉列表：
 			<select>
    			<option value="volvo">小浩</option>
    			<option value="saab">小白</option>
    			<option value="mercedes">小李</option>
    			<option value="audi">小张</option>
			</select>
			
 重置：<input type="reset" name="button" id="button" value="重置">
</form>
```

## 12. 时间（input类型）：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208141715713.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)

```markup
	月份：<input type="month" name="" id=""><br>
	
	星期：<input type="week" name="" id=""><br>
	
	日期：<input type="date" name="" id=""><br>
	
	时间：<input type="time" name="" id=""><br>
```
## 13. 范围
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208141927516.png)

```markup
<input type="range">
```
## 14. 进度条
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208142022113.png)
```markup
<progress value="80" max="100" min="0"></progress>
```

