---
title: Java Web-入门笔记
tag: Java
categories:
  - 后端
  - Java
  - Java Web
---



# Java Web-笔记

[toc]



## 1. 概述



什么是Java Web？

>   使用Java 语言开发 互联网应用程序。







软件架构：

>   -   C/S
>   -   B/S







B/S 架构的资源分类：

>   -   `静态资源`：所有用户看到的效果一样。
>
>       如：文本、图片、视频、html、css、js等。
>
>       当客户端请求静态资源时，服务端直接发送，浏览器中内置的解析引擎会解析静态资源。
>
>    
>
>    
>
>   
>
>   -   `多态资源`：所有用户访问的结果可能不一样。
>
>       如：jsp，serverlet，php，asp等
>
>       当客户端请求动态资源时，服务端会将动态资源转化为静态资源，并返回给浏览器。







## 2. JS语法补充

-   [JavaScript-笔记 | Cyw的笔记栈 (cder123.github.io)](https://cder123.github.io/2021/06/02/JavaScript-笔记/#JavaScript-笔记)



### 2.1 RegExp对象：

RegExp对象有2种定义的方式，可使用 函数` test( )` 来判断字符串`是否`符合`正则表达式`。



>   -   `^`：开始
>   -   `$`：结尾
>   -   `\d`：数字
>   -   `\D`：不包括数字
>   -   `\w`：字母
>   -   `\W`：不包括字母
>   -   `[abc]`：a、b、c中的任意一个
>   -   `[a-z]`：a到z的26个字母中的一个
>   -   `[0-9]`：0到9的10个数字中的一个



-   方式 1：【此方式的正则必须使用双斜杠】

```js
/*
*	格式：
*			var reg = new RegExp("正则表达式");	// 注意该方式的正则必须使用双斜杠
			
			reg.test("字符串");	// 如果字符串符合正则表达式，返回true
*/


// 匹配1~3个数字
	var a = new RegExp("\\d{1,3}");

	a.test(123456)	// true

```





-   方式 2：

```js
/*
*	格式：
*			var reg = /正则表达式/;	
			
			reg.test("字符串");	// 如果字符串符合正则表达式，返回true
*/

	var s = /\w{1,4}/;

	s.test("zhangSan");	


```





案例：

```js
// 匹配：字母a开头，数字0结尾，中间至少1个字母的字符串	
	var a = /^a\w+0$/

	a.test("a123b")		// false

	a.test("ac0")		// true
```







### 2.2 Global对象：



无需定义，对象中的`方法`可以`直接调用`。



>   -   `encodeURI()`：uri 的编码
>   -   `decodeURI()`：uri 的解码
>   -   `encodeURIComeonent()`：uri 的编码【对整个网址】
>   -   `decodeURIComponent()`：uri 的解码
>   -   `parseInt()`：将参数字符串从第0位开始的连续的数字字符串转为数字
>   -   `isNaN()`



需要对`URI `进行编码和解码的原因：

>   `URI` 默认`不支持中文`传输，以`%hh`表示`1个字节`。



演示：【`utf-8编码：中文3字节，英文1字节`】

```js
	var url_1 = "富强民主"

	encodeURI(a);	// "%E5%AF%8C%E5%BC%BA%E6%B0%91%E4%B8%BB"

	decodeURI("%E5%AF%8C%E5%BC%BA%E6%B0%91%E4%B8%BB") // "富强民主"

```





### 2.3 Bom对象：

JS中的对象【有大到小】：

>   -   Navigator：浏览器
>   -   window：一个浏览器的标签【包括地址栏（Location对象）、历史记录（history对象）、前进按钮、后退按钮等】
>   -   screen对象
>   -   Dom：页面

小结：

Bom对象由以下几个对象组成：

-   Navigator

-   Window

-   Location

-   History

-   Screen

    

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>

</head>
<body>
	
	<input id="openBtn" type="button" value="打开">
	<input id="closeBtn" type="button" value="关闭">

    <script>
    var openBtn = document.getElementById("openBtn");
    var closeBtn = document.getElementById("closeBtn");
    var newWin = null;

// 打开新窗口
    openBtn.onclick = function () {
        newWin = open("http://www.baidu.com");
    }

// 关闭新窗口
    closeBtn.onclick = function () {
        newWin.close();
    }

    </script>

</body>
</html>
```





### 2.4 定时器：

-   `setTimeout(“js代码/函数”,毫秒数)`：定时器，返回唯一标识
-   `clearTimeout()`：
-   `setInterval("js代码/函数",周期毫秒数)`：循环定时器，返回唯一标识
-   `clearInterval()`

```js
// setTimeout(js代码/函数,毫秒数)

	var id_1 = setTimeout("alert('你好');",3000);	// 3秒后弹出”你好“

	clearTimeout(id_1);


// setInterval(js代码/函数,周期毫秒数)

	var id_2 = setInterval("alert('你好')",3000);	// 每隔3秒弹出1次”你好“

	clearInterval(id_2);




// 例如：
setTimeout(function(){
	window.location.href = "http://v.qq.com";
},3000);
```





案例：定时跳转

```js
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>

</head>
<body>
	
	<span style="color:red;" id="seconds">5</span>秒后跳转

	<script>
		var SecTag = document.getElementById("seconds");
		var sec = 5;

		function showSec() {	
		
			sec--;
		
			
			if(sec<=0){

				window.location.href = "http://cder123.gitee.io/";
			}

			
			SecTag.innerHTML=sec;
		
		}

		setInterval("showSec()",1000);


	</script>

</body>
</html>
```



### 案例-表格-增、删、全选

#### HTML-部分：

```js
	<div>
		编号<input id="in_id" type="text">
		姓名<input id="in_name" type="text">
		性别<input id="in_sex" type="text">
		<input id="in_addBtn" type="button" value="添加">
	</div>

	
	<table id="tb" border="1" style="margin: 10% auto;">
		<caption>名单</caption>
		<tr>
			<td><input type="checkbox"></td>
			<td>编号</td>
			<td>姓名</td>
			<td>性别</td>
			<td>操作</td>
		</tr>
	</table>
	
```



#### JS 公共部分：

```js
		var in_id = document.getElementById("in_id");
		var in_name = document.getElementById("in_name");
		var in_sex = document.getElementById("in_sex");
		var in_addBtn = document.getElementById("in_addBtn");

		var tb = document.getElementById("tb");

```





#### JS 添加/删除-功能

```js
// 添加一行
		in_addBtn.onclick = function(){
			// 创建表格的1行
			var tr = document.createElement("tr");

			// 创建表格中的5个单元格【复选框、编号、姓名、性别、操作】
			var td0 = document.createElement("td");
			var td1 = document.createElement("td");
			var td2 = document.createElement("td");
			var td3 = document.createElement("td");
			var td4 = document.createElement("td");

			// 复选框
			var chkbox = document.createElement("input");
			chkbox.type="checkbox";
			chkbox.name = "chkbox";
			td0.appendChild(chkbox);
			tr.appendChild(td0);

			// 编号
			var id_txt = document.createTextNode(in_id.value);
			td1.appendChild(id_txt);
			tr.appendChild(td1);

			// 姓名
			var name_txt = document.createTextNode(in_name.value);
			td2.appendChild(name_txt);
			tr.appendChild(td2);

			// 性别
			var sex_txt = document.createTextNode(in_sex.value);
			td3.appendChild(sex_txt);
			tr.appendChild(td3);

			var operbtn = document.createElement("a");
			var optxt = document.createTextNode("删除");
			operbtn.appendChild(optxt);
			operbtn.href="javascript:void(0);";
			

// 删除一行
			operbtn.addEventListener('click',function(e){
				//console.log(e);				
				e.path[3].removeChild(e.path[2]);
			});
				
			td4.appendChild(operbtn);
			tr.appendChild(td4);

			tb.appendChild(tr);
            
            in_id.value ="";
			in_name.value ="";
			in_sex.value ="";
			
			
		}
```



#### 全选-功能：

```js

		var chkAll = document.getElementById("chkAll");

		chkAll.addEventListener('click', function (e) {
// 判断全选框是否选中？所有行勾选：所有行取消勾选
			if (e.target.checked) {

				for (let i = 2; i < e.path[4].children.length; i++) {

					e.path[4].children[i].firstChild.firstChild.checked = true;
					console.log(e.path[4].children[i].firstChild.firstChild);
				}
			} else {
				for (let i = 2; i < e.path[4].children.length; i++) {

					e.path[4].children[i].firstChild.firstChild.checked = false;
					console.log(e.path[4].children[i].firstChild.firstChild);
				}
			}
		})

```















## 3. Bootstrap UI框架使用



什么是框架？

>   框架 就是 为加快开发而使用的半成品。



Bootstrap是twitter基于HTML、CSS、JS推出的框架。它定义了许多样式、js插件







### 3.1 快速入门



步骤：

-   下载 Bootstrap
-   放入到 项目文件夹下
-   引入 Bootstrap
-   在 HTML中使用







### 3.2 响应式布局



Bootstrap的响应式布局：

>   将一行分割为12个格子，可以指定元素占几个格子【不论是什么设备，都是12个格子】



例如：

PC上某个div占3个格子，在手机上占12个格子。





#### 3.2.1 使用步骤：

-   定义` 容器`，【类名：`container` （固定宽度，有留白）或 `.container-fluid` （100% 宽度）】
-   定义 `行`，【类名：`row`】只有 “列（column）” 可以作为 “行（row）” 的直接子元素。
-   定义` 元素`，指定元素在不同的设备上占据的格子数，【类名：`col-设备代号-格子数`】



设备代号：

-   `xs`：手机，< 768 px，例如：`col-xs-12`
-   `sm`：平板，>= 768 px
-   `md`：笔记本，>= 992 px
-   `lg`：台式机，>= 1200 px



注意：

>   如果1行中的元素所占的格子数`大于12`，则自动换行。



```html
<!doctype html>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>Bootstrap 演示-入门</title>

    <!-- Bootstrap -->
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css"
          integrity="sha384-HSMxcRTRxnN+Bdg0JdbxYKrThecOKuH5zCYotlSAcp1+c8xmyTe9GYg1l9a69psu" crossorigin="anonymous">

    <!-- jQuery (Bootstrap 的所有 JavaScript 插件都依赖 jQuery，所以必须放在前边) -->
    <script src="https://cdn.jsdelivr.net/npm/jquery@1.12.4/dist/jquery.min.js"
            integrity="sha384-nvAa0+6Qg9clwYCGGPpDQLVpLNn0fRaROjHqs13t4Ggj3Ez50XnGQqc/r8MhnRDZ"
            crossorigin="anonymous"></script>
    <!-- 加载 Bootstrap 的所有 JavaScript 插件。你也可以根据需要只加载单个插件。 -->
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/3.4.1/js/bootstrap.min.js"
            integrity="sha384-aJ21OjlMXNL5UyIl/XNwTMqvzeRMZH2w8c5cRVpzpU8Y5bApTppSuUkhZXN0VxHd"
            crossorigin="anonymous"></script>

</head>
<body>

<!--定义容器-->
	<div class="container-fluid">
<!--		定义行-->
		<div class="row">
<!--			定义元素，指定在笔记本md上占4个格子，平板sm上占12个格子-->
			<div class="col-md-4 col-sm-12" style="border:1px solid;">你好1</div>
			<div class="col-md-4 col-sm-12" style="border:1px solid;">你好2</div>
			<div class="col-md-4 col-sm-12" style="border:1px solid;">你好3</div>
		</div>
	</div>

</body>
</html>
```





#### 3.2.2 全局CSS样式

-   按钮：`class="btn btn-default"`
-   图片：`class="img-responsive"`
-   表格：
-   表单：【将多个表单项`form-control` 放在同一个`form-group`里】



注意：

>   表单的水平排列可以在`form`标签上加类名`form-horizontal`，然后给`表单项`指定`所占的列的格子数`。
>
>   如：`class="col-sm-2"`





#### 3.2.3 组件

##### 导航条



汉堡按钮【缩小屏幕时出现】

```html
 <div class="navbar-header">
	// 汉堡按钮
      <button type="button" class="navbar-toggle collapsed" 
              				data-toggle="collapse" 
              				data-target="#bs-example-navbar-collapse-1" 
              				aria-expanded="false">
          
        <span class="sr-only">Toggle navigation</span>
      // 汉堡按钮的3条横线    
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
      </button>
     
      <a class="navbar-brand" href="#">logo-首页</a>     
</div>
```



下拉列表：

```html
 <li class="dropdown">
          <a href="#" class="dropdown-toggle" 
             			data-toggle="dropdown" 
             			role="button" 
             			aria-haspopup="true" 
             			aria-expanded="false">下拉列表 <span class="caret"></span></a>
          <ul class="dropdown-menu">
            <li><a href="#">Action</a></li>
            <li><a href="#">Another action</a></li>
            <li><a href="#">Something else here</a></li>
            <li role="separator" class="divider"></li>
            <li><a href="#">Separated link</a></li>
            <li role="separator" class="divider"></li>
            <li><a href="#">One more separated link</a></li>
          </ul>
</li>
```



搜索框+提交按钮：

```html
<form class="navbar-form navbar-left">
    //搜索框
        <div class="form-group">
          <input type="text" class="form-control" placeholder="Search">
        </div>
    // 提交按钮
        <button type="submit" class="btn btn-default">Submit</button>
</form>
```







##### 分页条



禁用（类名）：`disable`

选中：`active`

```html
<nav aria-label="Page navigation">
  <ul class="pagination">
    <li>
// 显示“前一页”
      <a href="#" aria-label="Previous">
        <span aria-hidden="true">&laquo;</span>
      </a>
    </li>
    <li><a href="#">1</a></li>
    <li><a href="#">2</a></li>
    <li><a href="#">3</a></li>
    <li><a href="#">4</a></li>
    <li><a href="#">5</a></li>
// 显示“后一页”      
    <li>
      <a href="#" aria-label="Next">
        <span aria-hidden="true">&raquo;</span>
      </a>
    </li>
  </ul>
</nav>
```





#### 3.2.3 轮播图

类名：`carousel`

```html
<div id="carousel-example-generic" class="carousel slide" data-ride="carousel">
  <!-- 小圆点：Indicators -->
  <ol class="carousel-indicators">
    <li data-target="#carousel-example-generic" data-slide-to="0" class="active"></li>
    <li data-target="#carousel-example-generic" data-slide-to="1"></li>
    <li data-target="#carousel-example-generic" data-slide-to="2"></li>
  </ol>

  <!-- 轮播图：Wrapper for slides -->
  <div class="carousel-inner" role="listbox">
      
    <div class="item active">
      <img src="..." alt="...">
      <div class="carousel-caption">
        ...
      </div>
    </div>
      
    <div class="item">
      <img src="..." alt="...">
      <div class="carousel-caption">
        ...
      </div>
    </div>
    ...
  </div>

  <!-- 左右按钮：Controls -->
  <a class="left carousel-control" href="#carousel-example-generic" role="button" data-slide="prev">
    <span class="glyphicon glyphicon-chevron-left" aria-hidden="true"></span>
    <span class="sr-only">Previous</span>
  </a>
    
  <a class="right carousel-control" href="#carousel-example-generic" role="button" data-slide="next">
    <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span>
    <span class="sr-only">Next</span>
  </a>
    
</div>
```





















## 4. XML入门



### 4.1 概念：

XML：`可扩展标记语言`

-   可扩展：标签可以自定义



W3C：万维网联盟



功能：

>   -   存储数据：【 配置文件 + 在网络中传输 】





XML 与 HTML 的区别：

>   1.  XML 是自定义的，HTML是预定义的
>   2.  XML 语法严格，HTML语法松散
>   3.  XML 是存储数据的，HTML是展示数据的













### 4.2 语法：

```xml
<?xml version="1.0" encoding="UTF-8"?>

<users>
    
	<user id="1">
		<name>张三</name>
		<age>20</age>
	</user>

	<user id="2">
		<name>李四</name>
		<age>30</age>
	</user>
    
</users>


```



注意：`<?xml version="1.0" encoding="UTF-8"?>`只能写在第一行。







#### 4.2.1 XML的特点：

-   XML 以`.xml`为后缀。
-   XML 的第一行必须定义文档声明：`<?xml version="1.0" encoding="UTF-8"?>` 。
-   XML 有且只有`1个根结点`，如：上面示例中的`users`标签。
-   XML 的`属性值`必须使用`引号`包裹。
-   XML 的标签必须正确关闭：
    -   有子节点=》分成2段的标签；
    -   无子节点=》自闭合标签
-   XML 区分大小写。









#### 4.2.2 XML的组成部分：

>   -   文档声明：必须在第一行，`<?xml version="1.0" encoding="UTF-8"?>`。
>       -   格式：`<?xml 参数列表?>`
>           -   `version`：版本号【必选项】
>           -   `encoding`：编码格式，告诉解析引擎当前 XML文档使用的字符集。默认：ISO-8859-1
>           -   `standalone`：是否独立、不依赖于其他的文件，属性值：`yes`或`no` 。
>
>   
>
>   -   指令（了解）：结合css=》`<?xml-stylesheet type="text/css" href="./123.css"?>`
>
>   
>
>   -   标签：标签名称自定义，
>
>       -   规则：
>
>           -   标签名可以包括字母、数字、其他字符
>           -   只能以字母开头
>           -   不能以数字、符号、空格、xml 开头
>
>           
>
>   -    属性【可以定义约束】
>
>       -   id：唯一
>
>       
>
>   -   文本
>       -   CDATA区：该区域的文本不需要转义，能原样显示，`<![CDATA[需要原样显示的内容]]>`









#### 4.2.3 XML的约束：

约束：XML的书写规则（自定义的说明文档），由程序员（用户）来查看该文档中 什么标签 代表 什么意思。



-   作为框架的使用者【程序员】
    -   能够在 XML中`引入`约束文档
    -   能够 简单的`读懂`约束文档





XML 约束的分类：

-   `DTD约束`：一种简单的约束
-   `Schema约束`：一种复杂的约束





```xml-dtd
<!ELEMENT students (student*)>	  // 定义students标签，子元素为student标签，*表示可以出现多次
<!ELEMENT student (name,age,sex)> // 定义student标签,子元素为name、age、sex标签[只出现1次，按顺序)
<!ELEMENT name (#PCDATA)>		  // 定义name标签，标签体内为字符串
<!ELEMENT age (#PCDATA)>		  // 定义age标签，标签体内为字符串
<!ELEMENT sex (#PCDATA)>		  // 定义sex标签，标签体内为字符串
<!ATTLIST student number ID #REQUIRED> // 定义student标签的属性，属性名：number,属性类型：ID,必选项
```







##### 1. DTD的使用：

后缀：`.dtd`

-   引入DTD文档 到 XML：【2种 DTD格式】
    -   内部DTD【了解】：约束规则直接定义在XML文件中。
    -   外部DTD：单独写1个`.dtd`文件。
        -   本地的 外部DTD：`<!DOCTYPE 根标签名 SYSTEM "dtd文件的位置">`
        -   网络的 外部DTD：`<!DOCTYPE 根标签名 PUBLIC "dtd文件名" "dtd文件的url">`



内部DTD：

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!-- 内部dtd的引入、定义 --> 
<!DOCTYPE users [
	<!ELEMENT users (user+)>
	<!ELEMENT user (name,age)>
	<!ELEMENT name (#PCDATA)>
	<!ELEMENT age (#PCDATA)>
	<!ATTLIST user number ID #REQUIRED>

]>


<!-- xml示例  -->
<users>
	<user number="s1">
		<name>张三</name>
		<age>20</age>
	</user>

	<user number="s2">
		<name>李四</name>
		<age>30</age>
	</user>

</users>


```





DTD约束的缺陷：

>   无法约束标签内部的文本值







##### 2. Schema的使用：

后缀：`.xsd`



```xml
<?xml version="1.0"?>
<xsd:schema xmlns="http://www.itcast.cn/xml"
        xmlns:xsd="http://www.w3.org/2001/XMLSchema"
        targetNamespace="http://www.itcast.cn/xml" elementFormDefault="qualified">
        
    <xsd:element name="students" type="studentsType"/>

    <xsd:complexType name="studentsType">
        <xsd:sequence>
            <xsd:element name="student" type="studentType" minOccurs="0" maxOccurs="unbounded"/>
        </xsd:sequence>
    </xsd:complexType>

    <xsd:complexType name="studentType">
        <xsd:sequence>
            <xsd:element name="name" type="xsd:string"/>
            <xsd:element name="age" type="ageType" />
            <xsd:element name="sex" type="sexType" />
        </xsd:sequence>
        <xsd:attribute name="number" type="numberType" use="required"/>
    </xsd:complexType>

    <xsd:simpleType name="sexType">
        <xsd:restriction base="xsd:string">
            <xsd:enumeration value="male"/>
            <xsd:enumeration value="female"/>
        </xsd:restriction>
    </xsd:simpleType>

    <xsd:simpleType name="ageType">
        <xsd:restriction base="xsd:integer">
            <xsd:minInclusive value="0"/>
            <xsd:maxInclusive value="256"/>
        </xsd:restriction>
    </xsd:simpleType>

    <xsd:simpleType name="numberType">
        <xsd:restriction base="xsd:string">
            <xsd:pattern value="heima_\d{4}"/>
        </xsd:restriction>
    </xsd:simpleType>

</xsd:schema> 

```



使用Schema约束的步骤：

>   1.  写 根标签
>   2.  引入 xsi 前缀
>   3.  引入 xsd命名空间
>   4.  声明每个xsd的前缀

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!-- 

	1.填写xml文档的根元素
	2.引入xsi前缀【固定格式】.  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	3.引入xsd文件命名空间.  xsi:schemaLocation="http://www.itcast.cn/xml  student.xsd"
	4.为每一个xsd约束声明一个前缀,作为【引入多个约束文件时的标识】  xmlns="http://www.itcast.cn/xml" 
	
	
 -->
 <students   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 			
 		   	 xsi:schemaLocation="
                                 http://www.itcast.cn/xml_1  student1.xsd
                                 http://www.itcast.cn/xml_2  student2.xsd
             "
 		     xmlns="http://www.itcast.cn/xml_1" 
             xmlns:a="http://www.itcast.cn/xml_2"
 		    >
 	<student number="heima_0001">
 		<a:name>tom</name>	// a: 表示的是当前标签使用的是a这个命名空间的scema约束文档
 		<a:age>18</age>
 		<a:sex>male</sex>
 	</student>
		 
 </students>
```











### 4.3 解析：



操作XML文档，将XML文档中的数据读入内存。



-   操作XML文档：
    -   解析（读取）：将XML文档中的数据读入内存。
    -   写入：将内存中的数据写入XML文档，持久化存储。



解析 XML 的方式：

-   DOM：将XML文档的内容加载进内存，形成DOM树
    -   优点：操作方便，可以对文档CRUD
    -   缺点：占内存。
-   SAX：逐行读取【读一行，释放一行】，基于 事件驱动。【适合小设备】
    -   优点：占用内存少（只占1行)
    -   缺点：只能用于读取。







#### 4.3.1 XML的常见的解析器：

XML 解析器：根据以上的2种XML解析方式而编写的工具包。



-   JAXP：sun公司提供的XML解析器。【DOM + SAX】性能差，很少使用。
-   DOM4J：一款性能优秀的解析器。
-   Jsoup：一种HTML解析器，也可以用于解析XML。
-   PULL：一款安卓内置的解析器，采用SAX解析的方式。





#### 4.3.2 Jsoup解析器的使用：



快速入门：



-   步骤：

>   1.  导入`JAR包`：`jsoup-1.11.2.jar`
>   2.  获取`DOM对象`。
>   3.  获取`所需的标签`。
>   4.  获取`数据`。



```java
import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.select.Elements;

import java.io.File;
import java.io.IOException;

public class JsoupDemo {
    public static void main(String[] args) throws IOException {

        // 1. 获取 xml 文件的路径
        String path = JsoupDemo.class.getClassLoader().getResource("users.xml").getPath();

        // 2. Jsoup解析XML,得到DOM对象
        Document dom = Jsoup.parse(new File(path),"utf-8");

        // 3. 获取标签对象的数组
        Elements Elements = dom.getElementsByTag("name");

		// 4. 获取标签
        Element element = Elements.get(0);

        // 5. 得到标签包裹的文本
        String name = element.text();


    }
}

```

小结：

>   以上示例代码中的对象。
>
>   -   `Jsoup工具类`，解析HTML、XML，返回 Document 对象
>
>       -   `parse(File in ,String charSetName)`：解析HTML、XML**文件**
>       -   `parse(String html)`：解析HTML、XML**字符串**
>       -   `parse(Url 网址,int 超时)`
>
>       
>
>   -   `Document`：用于获取`Elements`对象。【`getElementsByXXXXXXX()`】
>
>   
>
>   -   `Elements`：Element对象的集合，相当于`ArrayList<Element>`
>       -   获取子元素：`getElementsByXXX( )`
>       -   获取属性值：`String attr(String Key)`,根据属性名来获取属性值
>       -   获取文本（被包裹的内容）：
>           -   `String text()`：获取纯文本
>           -   `String html()`：获取innerHTML的内容（即：包括子标签）
>   -   `Element`：元素对象
>   -   `Node`：结点对象。





##### 示例-1：

-   XML部分：

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE users [
	<!ELEMENT users (user+)>
	<!ELEMENT user (name,age)>
	<!ELEMENT name (#PCDATA)>
	<!ELEMENT age (#PCDATA)>
	<!ATTLIST user id ID #REQUIRED>

]>



<users>
	<user id="s1">
		<name>张三</name>
		<age>20</age>
	</user>

	<user id="s2">
		<name>李四</name>
		<age>30</age>
	</user>

</users>


```

-   Jsoup解析：

```java
import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.select.Elements;

import java.io.File;
import java.io.IOException;

public class JsoupDemo {
    public static void main(String[] args) throws IOException {

        String path = JsoupDemo.class.getClassLoader().getResource("users.xml").getPath();

        Document dom = Jsoup.parse(new File(path), "utf-8");

        Elements userList = dom.getElementsByTag("user");

        Element element = userList.get(0);
// 获取属性值
        String el_id = element.attr("id");
        System.out.println(el_id);

        System.out.println("-------");
// 获取纯文本
        String txt = element.text();
        System.out.println(txt);

        System.out.println("-------");

// 获取innerHTML
        String html = element.html();
        System.out.println(html);

        System.out.println("-------");
        
// 获取css选择器-标签名为 xxx 的元素
        Elements t = dom.select("age");
        System.out.println(t);
        
        Elements t = dom.select("");
        System.out.println(t);

// 获取id值为 s1 的元素         
        System.out.println("-------");
        Elements t2 = dom.select("#s1");
        System.out.println(t2);

    }
}

```











##### 快捷查询到元素：【2种方式】

-   `selector`：`Elements select(String css选择器)` =》 见上1个示例



-   `XPath`：[XPath 语法](https://www.runoob.com/xpath/xpath-syntax.html)

    -   导入`jsoup-1.11.2.jar` + `JsoupXpath-0.3.2.jar`
    -   获取DOM对象
    -   获取JXDom对象
    -   获取结点集【利用Xpath的语法】

    








#####  示例-2：

XML部分：见上面的 `示例-1`

Xpath部分：

```java
import cn.wanghaomiao.xpath.model.JXDocument;
import cn.wanghaomiao.xpath.model.JXNode;
import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;

import java.io.File;
import java.util.List;

public class JsoupDemo2 {
    public static void main(String[] args) throws Exception{

// 获取XML文件的路径
        String path = JsoupDemo2.class.getClassLoader().getResource("users.xml").getPath();

// 创建DOM对象       
        Document dom = Jsoup.parse(new File(path),"utf-8");
        
// 创建JXDom对象        
        JXDocument jxdoc = new JXDocument(dom);
        
// 以下的 “//user” 表示获取所有user结点
        List<JXNode> jxNodes = jxdoc.selN("//user");
        for (JXNode jxNode : jxNodes) {
            System.out.println(jxNode);
        }

        System.out.println("-------");

// 以下的 “//user/age” 表示获取所有user结点下的age标签
        List<JXNode> jxNodes2 = jxdoc.selN("//user/age");
        for (JXNode jxNode2 : jxNodes2) {
            System.out.println(jxNode2);
        }


        System.out.println("-------");


// 以下的 “//user[@id]” 表示获取的所有user结点中，具有属性名id的元素
        List<JXNode> jxNodes3 = jxdoc.selN("//user[@id]");
        for (JXNode jxNode3 : jxNodes3) {
            System.out.println(jxNode3);
        }

        System.out.println("-------");


// 以下的 “//user[@id=‘s1’]” 表示获取的所有user结点中，具有属性名为id,且属性id的值为s1的元素
        List<JXNode> jxNodes4 = jxdoc.selN("//user[@id='s1']");
        for (JXNode jxNode4 : jxNodes4) {
            System.out.println(jxNode4);
        }
        
        
    }
}

```









## 5. Web-Tomcat服务器





### 5.1 Web基础回顾：



1、软件架构：

>   -   C/S
>   -   B/S



2、资源分类：

>   -   静态资源：所有用户访问后的结果都一样，如：HTML、CSS、JS、图片【可`直接被浏览器解析`】
>   -   动态资源：每个用户访问后的结果可能不同，如：Serverlet / jsp、php、asp【先`转换为静态资源`】





3、网络通信三要素：

>   -   IP地址：网络中的唯一标识。
>   -   端口号：应用程序的唯一标志，0~65535
>   -   协议：规定数据传输的规则。【TCP、UDP】







### 5.2 Web服务器-概述：



-   服务器：安装了服务器软件的计算机。

-   服务器软件：接收、处理请求，做出响应。
-   web服务器软件【web容器】：部署Web项目，使用户可以用浏览器访问。



**注意**：动态资源必须要在Web容器中才能运行。





`JavaEE`：Java在企业级开发中的`规范的总和`。共有`13项`大的规范。



创建的Java相关的Web服务器：

>   -   webLogic：oracle公司，大型JavaEE服务器，收费，支持JavaEE的所有规范(13种)。
>   -   webSphere：IBM公司，大型JavaEE服务器，收费，支持JavaEE的所有规范(13种)。
>   -   JBoss：JBoss公司，大型JavaEE服务器，收费，支持JavaEE的所有规范(13种)。
>   -   `Tomcat`：Apache基金会，中小型JavaEE服务器，免费，仅支持`部分JavaEE的规范`。





### 5.3 Tomcat的使用：

部分笔记：[Nginx-入门-5.1小节中 | Cyw的笔记栈 (cder123.github.io)](https://cder123.github.io/2021/06/02/Nginx-入门笔记/#5-1-步骤一：安装Tomcat)



Tomcat：web服务器软件



#### 5.3.1 Tomcat需要掌握的操作：

>   1.  下载:
>
>   2.  安装：
>
>   3.  启动
>
>   4.  关闭
>
>   5.  卸载
>
>   6.  配置+部署





1.  下载：

版本：`Tomcat-8.5-windows版`  =》 [Apache Tomcat下载-windows版](https://tomcat.apache.org/download-80.cgi)





2.  安装：

解压压缩包，解压后的路径中`不要有中文、空格`。



Tomcat的目录结构：



![tomcat-目录结构](https://z3.ax1x.com/2021/06/19/RCTzQI.png)



Tomcat的各个目录的作用：

>   -   bin：可执行的二进制文件。
>   -   conf：配置文件。
>   -   lib：依赖 Jar 包。
>   -   logs：日志文件
>   -   temp：临时文件
>   -   webapps：存放、部署web项目
>   -   work：存放运行的时候的数据





3.  启动：

>   -   windows环境下双击运行：`bin/startup.bat`
>   -   打开浏览器，输入服务器的 IP地址+端口：`localhost:8080`  (访问自己)
>
>   
>
>   注意：可以在`conf/server.xml`中将`Connector标签`中的`8080`该为`80`，这样可以省略端口号



启动时，可能遇到的问题：

>   -   黑窗口一闪而过：
>       -   原因：JAVA_HOME环境变量配置错误。
>       -   解决：正确配置JAVA_HOME环境变量。
>
>   
>
>   -   启动报错：
>
>       -   原因：端口已经被占用【查看端口占用情况：netstat -ano】
>
>       -   解决：关闭占用8080端口的进程 或 修改自身的端口号【`conf/server.xml`，将`Connector标签`中的`8080`该为`别的端口`，保存，重新运行`bin/startup.bat`】
>
>           
>
>       





4.  关闭：

>   -   正常关闭：`bin/shutdown.bat`  或 ` ctrl + c`
>   -   强制关闭：点击启动窗口的x









5.  卸载：

>   直接删除目录。









6.  配置：



部署项目的方式【3种】：

-   背景：

>   项目：`hello/hello.html`



-   第一种：

>   1.  直接将`项目文件夹`放入`webapps`目录下。
>   2.  简化部署：将项目打成`.war`包，运行`bin/startup.bat`，将war包拖入`webapps`目录下，则自动解压缩。



-   第二种：【无需复制项目到`webapps`目录下】

>   -   打开`conf/server.xml`文件，拉到最后，在`Host`标签中，添加`<Context docBase="E:\hello" path="/abc" />`。【假设项目放在`E:\hello`中】
>
>       -   其中：`docBase`为物理路径，`path`为虚拟路径，浏览器访问时`/abc/hello.html`
>
>       
>
>   -   重新启动服务器【运行`bin/startup.bat`】



-   第三种：【推荐，无需重启Tomcat，】

>   -   在`conf/Catalina/localhost/`目录下，新建XML配置文件,`123.xml`，输入`<Context docBase="E:\hello" path="/abc" />`。【假设项目放在`E:\hello`中】
>
>       -   其中：`docBase`为物理路径，`path`为虚拟路径。浏览器访问时，直接输入XML配置文件名+html页面名：`/123/hello.html`就可访问。
>
>       如果不想让配置文件生效，可以将配置文件名最后面加上`_bak`









#### 5.3.2 静态项目、动态项目：



1.  目录结构：

动态项目：

>   – 项目根目录
>
>   ​	– WEB-INF 目录
>
>   ​			– web.xml：存放 项目的`核心配置文件`。
>
>   ​			– classes 目录：存放`字节码文件`的目录。
>
>   ​			– lib 目录：存放`依赖Jar包`的目录。
>
>   



2.  在 IDEA 中集成、使用 Tomcat：

-   `run -> Edit Configuration -> 左边 default【或+号】 -> Tomcat Server -> Local ` 
-   `右边 configure -> 选中Tomcat的安装目录 -> 确定 -> 应用`
-   创建JavaEE项目，测试 Tomcat是否集成成功。







## 6. 创建第一个 Java EE项目



-   [IEAD 2020.2 创建web、Spring项目](https://blog.csdn.net/qq_45738810/article/details/107842532)



创建第一个 Java EE项目-步骤：

>   1.  创建一个普通的 JavaSE工程。
>   2.  右键 `工程名` -》`add FrameWork Support` -》`WebApplication ` -》OK。
>   3.  在IDEA 中`集成 Tomcat服务器`。
>   4.  `启动` JavaWeb，查看 Tomcat  是否 集成成功、可用。



上面的第三步：集成Tomcat服务器：

-   `Run -> Edit Configuration `

![集成Tomcat服务器-1](https://z3.ax1x.com/2021/06/19/RPd978.png)

-   添加Tomcat：

![添加Tomcat](https://z3.ax1x.com/2021/06/19/RPdwND.png)



<img src="https://z3.ax1x.com/2021/06/19/RPdogs.png" alt="RPdogs.png" border="0" />

-   部署项目到Tomcat：

<img src="https://z3.ax1x.com/2021/06/19/RPwMrt.png" alt="RPwMrt.png" border="0" />

-   运行项目：

<img src="https://z3.ax1x.com/2021/06/19/RPwgz9.png" alt="RPwgz9.png" border="0" />























## 7. Serverlet

