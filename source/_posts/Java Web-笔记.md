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
-   web服务器软件【web容器】：部署Web项目，使得用户可以用浏览器访问。



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























## 7. Servlet



### 7.1 Servlet 概述：

Servlet： Server Applet，运行在服务器端的小程序。【`一种 由Tomcat等服务器解析 .class文件`】



`Servlet `实际上就是一组`接口`，接口中`定义了`一些 让`Java类`能够`被 浏览器访问、Tomcat 识别的规则`。将来我们需要`自定义一个类`，`实现`Servlet`接口`，`重写`接口中的`方法`。



`import javax.servlet.*;`



### 7.2 Servlet快速入门：

步骤：

1.  创建JavaEE项目。【右键项目名 -> 添加Framework -> webApplication 】，【集成 Tomcat】
2.  自定义一个类，实现Servlet接口。【需先导入`Tomcat安装目录 -> lib -> servlet-api.jar`】
3.  重写 Servlet接口中的方法。
4.  配置Servlet。【在项目中的 `web -> WEB-INF -> web.xml`里配置】





配置Servlet：

【访问顺序：`web.xml` =》 `ServletDemo1.class`】



`web.xml`：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    
<!--    1. 配置Servlet-->
    <servlet>
        <servlet-name>demo_1</servlet-name>
// 全类名【包名+类名】
        <servlet-class>cn.demo.web.servlet.ServletDemo1</servlet-class>
// 0或负数：Tomcat启动时，执行Servlet接口的init() 
// 正数：浏览器访问 资源页面时，执行Servlet接口的init()         
        <load-on-startup>5</load-on-startup>
    </servlet>
<!--    2. 配置servlet的映射-->
    <servlet-mapping>        
        <servlet-name>demo_1</servlet-name>
// 在浏览器地址栏中的显示        
        <url-pattern>/demo1</url-pattern>
    </servlet-mapping>
    
</web-app>
```



`cn.demo.web.servlet.ServletDemo1.class`：

```java
package cn.demo.web.servlet;

import javax.servlet.*;
import java.io.IOException;

public class ServletDemo1 implements Servlet {
    @Override
    public void init(ServletConfig servletConfig) throws ServletException {

    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

// 提供服务的方法
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        // 浏览器一访问【localhost/demo1】,IDEA的控制台就打印 “Hello Servlet !!”
        System.out.println("Hello Servlet !!");
    }

    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {

    }
}

```







### 7.3 Servlet的执行原理：

以`ServletDemo1.class`为例：

-   浏览器输入URL
-   找 项目下的 `web.xml`下的`<url-pattern>`标签。
-   `Tomcat `利用`反射`【Class.forName( )】，将`<servlet-class>` 标签中的`全类名`所对应的`字节码 加载 入内存`。
-   创建对象 ：`ServletDemo1.newInstance( )`
-   调用 `ServletDemo1.service( )`，执行方法中的代码。

![servlet的原理](https://z3.ax1x.com/2021/06/20/RiJnZF.png)







### 7.4 Servlet的生命周期-方法：



1、Servlet的生命周期-方法：

-   `public void init(ServletConfig )`： 创建 Servlet 时执行，只执行一次。

-   `public void service(servletRequest, ServletResponse )`：提供服务，每次访问servlet时，执行，执行多次。

-   `public void destroy( )`：销毁 Servlet 时【服务器 **正常关闭** 时】，执行。

--------

2、其他：

-   `public ServletConfig getServletConfig(   )`：获取Servlet的配置对象。

-   `public String getServletInfo( )`：获取配置信息。【版本、作者】



---------



创建-详解【Init（）】：

-   `public void init(ServletConfig )`： 创建 Servlet 时执行，只执行一次。



 `<load-on-startup>5</load-on-startup>`：指定创建的时间。【`web.xml`中的`<servlet>`标签下】

>   -   负数：在第一次**访问**时，调用`init()`
>   -   0或正数：服务器**启动**时，调用`init()`



问题：

>   init( )在多个用户同时访问时，出现`线程安全`问题。



解决：

>   不要在 Servlet 中定义成员变量。【即使定义了成员变量，也不要对该变量进行修改】



-----



### 7.5 Servlet 3.0 ：

好处：支持使用`注解` 来配置 Servlet，无需`web.xml`



步骤：

>   1.  创建JavaEE项目，选择Servlet3.0版本以上。
>   2.  自定义一个类，实现Servlet接口。
>   3.  重写Servlet接口的方法。
>   4.  在自定义的类上，使用`@WebServlet`注解来配置 Servlet 。



```java
import javax.servlet.*;
import javax.servlet.annotation.WebServlet;
import java.io.IOException;

// 使用注解配置Servlet
//【等价于 @WebServlet("/demo") 】
    @WebServlet(urlPatterns={"/demo"}) 
    public class Demo1 implements Servlet {
        @Override
        public void init(ServletConfig servletConfig) throws ServletException {

        }

        @Override
        public ServletConfig getServletConfig() {
            return null;
        }

        @Override
        public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {

        }

        @Override
        public String getServletInfo() {
            return null;
        }

        @Override
        public void destroy() {

        }
    }

```





### 7.6 IDEA和Tomcat的相关配置：

-   IDEA会为每个Tomcat项目单独分配一份配置文件，存放位置可以查看运行的时候控制台的日志【Catalina_Base】



-   **IDEA工作空间下的项目** 和 **Tomcat部署的项目**：
    -   Tomcat 部署的项目 来自 IDEA工作空间下的项目的Web目录，但不是同一个【相当于 复制了一份】
    -   IDEA 项目的`WEB-INF目录`下的文件无法直接被`浏览器访问`。





-   断点调试：以 `debug`的形式启动。





-   解决 IDEA中的 Tomcat控制台的`乱码`：[解决Tomcat日志-控制台乱码](https://www.jianshu.com/p/b438332d1069)















### 7.7 Servlet的体系结构：



Servlet接口的两个子抽象类：

-   GenericServlet：只需重写抽象的`service()`方法，该类的其他方法都已做了空实现【方法体 内无代码】
-   HttpServlet【常用】：简化了书写判断【Get、POST】等请求方式的的代码。【doGet( )、doPost( )】



【使用`子类的原因`：为了`无需`再`重写`Servlet接口的`所有方法`（只`按需`重写）】





HttpServlet的使用步骤：

>   -   自定义一个类，extends 继承 HttpServlet类
>   -   覆盖重写 `doGet( )`、`doPost( )`



#### 1. Servlet：

```java
package cn.my.servletdemo;

import javax.servlet.*;
import javax.servlet.annotation.WebServlet;
import java.io.IOException;

@WebServlet("/demo1")
public class Demo1 implements Servlet {
    @Override
    public void init(ServletConfig servletConfig) throws ServletException {

    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("This is Demo1");
    }

    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {

    }
}

```



#### 2. GenericServlet：

```java
package cn.my.servletdemo;

import javax.servlet.GenericServlet;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.annotation.WebServlet;
import java.io.IOException;

@WebServlet("/demo2")
public class Demo2 extends GenericServlet {
    
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("This is Demo2");
    }
    
}

```



#### 3. HttpServlet:

```java
package cn.my.servletdemo;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;


@WebServlet("/demo3")
public class Demo3 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doGet(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}

```







### 7.8 Servlet的配置 ：

注解`@WebServlet(参数1，参数2.......)` 中的参数：

-   `urlpattens`：路径，可以有多个`@WebServlet({"/d4","/dd4","ddd4"})`

>   urlpattens路径的3种规则：
>
>   -   `/xxx`
>   -   `/xxx/xxx`：多层路径【目录结构】
>   -   `*.do`：拓展名匹配【注意：此方式没有斜杠，do为自定义的拓展名】









## 8. HTTP



### 8.1 HTTP概述：

概念：超文本传输协议。



传输协议：定义了通信时传输的格式。



特点：

>   -   基于TCP/IP协议
>   -   默认端口：80
>   -   请求/响应模型：1请求，1响应
>   -   无状态：每次请求之间相互独立，请求与请求之间不能交换数据。





HTTP协议的版本：

>   -   1.0版：建立连接、请求、响应、断开连接【每一次请求都需重新建立连接】
>   -   1.1版：建立连接、请求、响应【复用连接】





### 8.2 请求报文：



Request报文的格式：【请求行 + 请求头 + 空格 + 请求体】

>   -   请求行
>
>       ​	格式：
>
>       ​				请求方式	请求url	请求协议/版本号
>
>       ​	例如：	GET http://localhost/login.html	HTTP/1.1
>
>        	请求方式：
>     	     	     	     	     	     	     	     	     	     	     	     	     	     	     	     	     	     	     	     	     	     	
>        		GET：参数放在url后，不安全，url的长度有限制
>        		POST：参数放在请求体里，url长度无限制，相对安全
>
>   
>
>   -   请求头：
>
>       ​	格式：
>
>       ​				请求头名：请求头值，请求头值
>
>       ​	常见的请求体：
>
>       		- HOST：主机名，IP地址
>       		- User-Agent【常用】：浏览器版本信息（可在服务端获取该信息，并做一些处理来解决兼容性问题）
>       		- Referer【常用】：来自哪里（作用：防盗链，统计）
>       		- Accept：浏览器允许接收的信息。
>       		- Connection：连接状态
>
>   
>
>   -   空行
>   -   请求体





#### 8.2.1 Request 原理：

![req + res](https://z3.ax1x.com/2021/06/26/R3old0.png)

注意：

-   Request 和 Response对象是由Tomcat服务器创建的，
-   Request对象：获取消息
-   Response对象：响应消息





#### 8.2.2 Request对象的继承体系结构：

-   ServletRequest 接口
    -   HttpServletRequest 接口
        -   org.apache.catalina.connector.RequestFacade 类：由Tomcat来实现







#### 8.2.3 Request对象的概念：

-   获取请求数据：

    -   请求行： 格式 =》`GET	/day1/demo1?username=zhangsan	HTTP/1.1`

        >    获取请求方式：GET
        >
        >   ​			`String getMethod( )`
        >
        >   
        >
        >   获取虚拟目录：【常用】 /day1
        >
        >   ​			`	String getContextPath( )`
        >
        >    
        >
        >   获取Servlet的路径：/demo1
        >
        >   ​			`	String getServletPath( )`
        >
        >    
        >
        >   获取GET请求的参数：username=zhangsan 
        >
        >   ​			`String getQueryString( )`
        >
        >    
        >
        >   获取URI：【常用】
        >
        >   ​			`String getRequestURI( )`：`/day1/demo1`
        >
        >   ​			`StringBuffer getRequestURL( )`：`http://localhost/day1/demo1`
        >
        >    
        >
        >   获取协议、版本号：Http/1.1
        >
        >   ​			`String getProtocol( )`
        >
        >    
        >
        >   获取客户机的IP：
        >
        >   ​			`String getRemoteAddr( )`
        >
        >   

    

    -   请求头： 格式 =》`UserAgent:Chrome`

        >   获取请求头：
        >
        >   ​				`String getHeader(String name)`：【常用】，其中的name不区分大小写。
        >
        >   ​				`Eumeration<String> getHeaderNames()`：返回1个迭代器
        >
        >   							+ hasMoreElements( )
        >   							+ nextElement( )
        >
        >   

    ```java
    // 获取请求头
    
    package cn.mydemo;
    
    import javax.servlet.ServletException;
    import javax.servlet.annotation.WebServlet;
    import javax.servlet.http.HttpServlet;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    import java.io.IOException;
    import java.util.Enumeration;
    
    
    @WebServlet("/demo2")
    public class Demo2 extends HttpServlet {
        @Override
        protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    
            // get / post
            String method = req.getMethod();
            System.out.println(method);
    
            // 虚拟目录
            String contextPath = req.getContextPath();
            System.out.println(contextPath);
    
            // 目录
            String servletPath = req.getServletPath();
            System.out.println(servletPath);
    
            // http/1.1
            String protocol = req.getProtocol();
            System.out.println(protocol);
    
            // /demo2
            String requestURI = req.getRequestURI();
            System.out.println(requestURI);
    
            // http://localhost/demo2
            StringBuffer requestURL = req.getRequestURL();
            System.out.println(requestURL);
    
            // 客户端ip
            String remoteAddr = req.getRemoteAddr();
            System.out.println(remoteAddr);
    
            // 请求头的所有键
            Enumeration<String> headerNames = req.getHeaderNames();
            while(headerNames.hasMoreElements()){
                System.out.println("--------");
                // 键
                String s = headerNames.nextElement();
                // 值
                String header = req.getHeader(s);
                System.out.println(s+" = "+header);
            }
    
    
    
        }
    }
    
    ```

    

    -   请求体：

        >   只有POST方式才有请求体，请求体中封装了请求的参数。
        >
        >    
        >
        >   步骤：
        >
        >   -   Request 获取`流对象`：
        >       -   `BufferedReader getReader()`获取字符输入流，只操作字符。
        >       -   `ServletInputStream getInputStream()`：获取字节输入流。
        >   -   从`流对象`中获取`数据`。

    



-   其他：

    -   获取请求参数：【GET、POST】

        ​		解决参数的中文乱码：

        ​						GET：参数中的中文乱码在Tomcat8以上的版本中已被解决。

        ​						POST：中文乱码可以在获取参数前通过设置Request的编码`req.setCharacterEncoding(“utf-8”)`

        >   `String getParamter(String name)`根据参数名获取参数值，user = lisi&psw=123。
        >
        >   
        >
        >   `String[] getParamterValues(String name)`根据参数名获取参数值的数组，hobby=aaa&hobby=bbb
        >
        >    
        >
        >   `Eumeration<String> getParamterNames(String name)` 获取所有参数名
        >
        >    
        >
        >   `Map<String,String[]> getParamterMap()`获取所有参数的集合

        ```java
        // 通用方法获取参数【以post为例】
        
        // 以下省略了导包语句
        
        @WebServlet("/demo5")
        public class Demo5 extends HttpServlet {
        
            @Override
            protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // POST方式 , 获取请求体里的参数     
                String usr = req.getParameter("usr");
                System.out.println(usr);
        
            }
        }
        
        ```

        

    -   资源转发：在服务器内部资源跳转。

        ​				特点：

        ​							1、 浏览器地址栏不变

        ​							2、只能访问当前服务器内部的资源

        ​							3、转发是1次请求

        >   `RequestDispatcher  rd =  req.getRequestDispatcher(String path) `
        >
        >   
        >
        >   `rd.forward(req,res)`

        ```java
        // demo5页面的请求转发到demo6
        
        // 以下省略了导包语句
        
        @WebServlet("/demo5")
        public class Demo5 extends HttpServlet {
            
            @Override
            protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        
                // 转发请求        
                System.out.println("This is demo 5");
                req.getRequestDispatcher("/demo6").forward(req,resp);
                
                
            }
        
        
        }
        
        ```

        

    
    -   共享数据：【域对象：在作用范围内共享数据】

        >   Request域：【用于一次请求在多次转发时共享数据】
        >
        >   -   setAttribute(String name，Object obj)
        >   -   getAttribute(String name)
        >   -   removeAttribute(String name)

    -   获取ServletContext：`getServletContext(String name)`





#### 8.2.4 Request案例：登录

![案例需求](https://z3.ax1x.com/2021/06/26/R8Astf.png)



![分析](https://z3.ax1x.com/2021/06/26/R8ZaX4.png)



步骤：

1、创建项目，导入Jar包，创建login.html页面，【表单的action：`虚拟目录 + servlet的资源路径`】

2、创建数据库。

3、创建User类

4、创建UserDao类，提供login方法，测试login( )



以下为简化版示例【未连数据库、未创建User和UserDao】

Servlet01部分：

```java



@WebServlet(name = "Servlet01", value = "/Servlet01")
public class Servlet01 extends HttpServlet {

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        
        String username = request.getParameter("username");
        String psw = request.getParameter("psw");
        
        if("123".equals(psw) && "abc".equals(username)){

            System.out.println("servlet01... => 开始跳转到Servlet02");

            response.setStatus(302);
            response.setHeader("location","/JavaWeb/Servlet02");
            //response.sendRedirect("/JavaWeb/Servlet02");
        }else{
            response.getWriter().write("密码错误！！");
        }


    }
}

```

JSP部分:

```java


// action后应该为 URI（包括虚拟目录的路径）
 <form action="/JavaWeb/Servlet01" method="post">
    <input type="text" name="username" placeholder="用户名">
    <input type="text" name="psw" placeholder="密码">
    <input type="submit">
</form>
```

Servlet02部分：

```java
@WebServlet(name = "Servlet02", value = "/Servlet02")
public class Servlet02 extends HttpServlet {

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        
        System.out.println("servlet02...");
        
    }
}

```







#### 8.2.5 BeanUtils

Spring的一个工具类。



Java Bean：用于封装数据的Java类。

-   类名：public
-   成员变量：private，使用getter、setter来访问。





BeanUtils的常用方法：

>   getProperty( )
>
>   setProperty( )
>
>   populate( obj )







### 8.3 响应报文：

Response对象-功能：设置响应消息（响应行、响应头、响应体）。

​        

-   设置响应行：

>   1、格式：`HTTP1.1  200  OK`
>
>   2、设置状态码：`setStatus( int 状态码)`



-   设置响应头：

>   `setHeaders(String name,String value)`
>
>    =》` response.setHeader("content-type","text/html;charset=utf-8");`

```java
package demo1;

import javax.servlet.*;
import javax.servlet.http.*;
import javax.servlet.annotation.*;
import java.io.IOException;

// value后的路径不包括虚拟目录
@WebServlet(name = "Servlet01", value = "/Servlet01")
public class Servlet01 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request,response);
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("servlet01... => 开始跳转到Servlet02");

        // 重定向【方式1】：
        //		先设置状态码，再设置location = URI(包括虚拟目录)
        response.setStatus(302);
        response.setHeader("location","/JavaWeb/Servlet02");
        response.setHeader("content-type","text/html;charset=utf-8");
        
        // 重定向【方式2】
        response.sendRedirect("/JavaWeb/Servlet02");
    }
}

```


-   设置响应体：

>   1、获取输出流
>
>   ​			=》字符输出流：`PrintWriter getWriter();`
>
>   ​			=》字节输出流：`ServletOutputStream getOutputStream()`
>
>   2、使用输出流，输出到客户端







**重定向 redirect **与 **转发 forward** 的区别：( 转发在8.2.3小节 )

>   转发的特点：
>
>   ​		1、地址栏不变。
>
>   ​		2、只能访问到当前这台服务器内部的资源。
>
>   ​		3、转发无论转多少次，都只是在对同1个请求在转发。（只有1次请求）
>
>    
>
>   重定向的特点：
>
>   ​		1、地址栏发生变化
>
>   ​		2、可以访问其他服务器的资源
>
>   ​		3、重定向是2次请求











响应头的格式：

>   -   响应头名：值
>   -   ContentType：告诉客户端编码格式
>   -   Content-disposition：告诉服务器当前的客户端是使用什么格式打开的。







#### 1、路径的写法：

[java web中的各种servlet路径](https://blog.csdn.net/linghuainian/article/details/82227177)



-   绝对路径：`/abc/1.html`
-   相对路径：`../2.html`   |   `./3.html`





什么时候需要在`路径中加虚拟目录`?

>   -   `客户端`和服务器之间使用时，需要有虚拟目录（客户端发出：表单）
>
>       虚拟目录一般使用动态获取路径：`request.getContextPath()` => `<a>`、`<form>`、重定向。
>
>   
>
>   -   `服务器`与服务器之间使用时，不需要有虚拟目录：（request对象的路径转发）





#### 2、解决响应报文的中文乱码：【3种】

>   1、`response.setCharacterEncoding (“GBK”)`
>
>   2、`response.setContentType("text/html;charset=utf-8");`
>
>   3、`response.setHeader("content-type","text/html;charset=utf-8");`

```java
 protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("servlet02...");
        response.setContentType("text/html;charset=utf-8");
        // 发送响应报文
        response.getWriter().write("你好，world");
    }
```









#### 3、验证码的实现：

>   本质：图片
>
>   目的：防止恶意的注册

验证码-后端代码：

```java
package demo1;

import javax.imageio.ImageIO;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.awt.*;
import java.awt.image.BufferedImage;
import java.io.BufferedInputStream;
import java.io.IOException;
import java.util.Random;


@WebServlet("/CheckCode")
public class CheckCode extends HttpServlet {

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        int width =100;
        int height =50;

        // 创建缓存图片对象
        BufferedImage bimg = new BufferedImage(width,height,BufferedImage.TYPE_INT_RGB);



        // 设置验证码图片的背景色-粉色
        Graphics g = bimg.getGraphics();
        g.setColor(Color.PINK);
        g.fillRect(0,0,width,height);



        // 设置验证码图片的边框
        g.setColor(Color.black);
        g.drawRect(0,0,width-1,height-1);



        // 生成验证码字符
        String str = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";

        Random rd = new Random();
        for (int i = 0; i < 4; i++) {
            //生成随机角标
            int idx = rd.nextInt(str.length());

            //获取随机字符
            char ch = str.charAt(idx);

            // 写验证码
            g.drawString(ch+"",(width+2)/5*i,height/2);
        }


        // 画验证码的干扰线
        g.setColor(Color.green);
        
        for (int i = 0; i <10 ; i++) {
            int x1 = rd.nextInt(width);
            int x2 = rd.nextInt(width);
            int y1 = rd.nextInt(height);
            int y2 = rd.nextInt(height);
            g.drawLine(x1,y1,x2,y2);
        }

        // 输出验证码
        ImageIO.write(bimg,"jpg",resp.getOutputStream());
    }

}

```

验证码前端代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script>
        window.onload = function () {
            //获取图片
            var img = document.getElementById("checkcode");
            
            img.onclick = function () {
                // 为了保证点击图片可以切换验证码，可以将src的地址 = servlet的地址
                // 因为浏览器有缓存，所以必须在src后的地址中拼接1个时间戳，让验证码刷新
                var time = new Date().getTime();
                img.src = "/JavaWeb/CheckCode?" + time;
            }

        }
    </script>
</head>
<body>
    <img id="checkcode" src="/JavaWeb/CheckCode" alt="">
    <a id="changecode" href="">看不清，换一张</a>
</body>
</html>
```











### 8.4 ServletContext

概念： `ServletContext接口` 代表整个Servlet应用，可以和程序的容器（Tomcat）通信。



功能：

>   -   获取 MIME类型
>       -   `MIME类型`是一种文件数据类型，格式为：`大类型/小类型`，如：`text/html`，`image/jpeg`
>       -   获取MIME类型的方法：`String getMimeType(文件名)`
>       -   获取MIME的目的：设置 ContentType
>   -   域对象：共享数据（req.getAttr）
>   -   获取文件在服务器上的真实路径。





获取 ServletContext（2种）

-   `request.getServletContext( )`
-   `this.getServletContext( )`







获取MIME类型：

```java
        ServletContext context = this.getServletContext();
        String type = context.getMimeType("123.jpg");
        System.out.println(type);
```







获取域对象：

-   `request.setAttribute(key,value);`
-   `request.getAttribute(key,value);`
-   `request.removeAttribute(key);`

ServletContext可以共享所有用户的数据。







获取真实的文件路径：

-   `getRealPath(路径);`



配置文件的存放位置：

>   -   项目的src下
>   -   web目录下
>   -   web/WEB-INF目录下

演示-获取`项目下的web/b.txt`文件：

```java
 ServletContext context = this.getServletContext();
 String path = context.getRealPath("/b.txt");
 File file = new File(path);
```







### 8.5 案例：文件下载

需求：任何文件，只要点击，就弹出下载框



思路：使用 response对象的`content-disposition:attachment;filename=xxx`，其中 filename 后的值为弹出框要显示的名称。



步骤：

>   1、创建html页面，将超链接的 href 指向 servlet地址：`href="/servlet01?filename=abc.jpg"`。
>
>   2、创建servlet，获取文件名，使用`字节输入流`将文件加载入内存。
>
>   3、指定response的`ContentType`和`content-disposition:attachment;filename=xxx`
>
>   4、利用`response输出流`，将数据写出。



前端：

```html
  <a href="/JavaWeb/downloadServlet?filename=1.jpg">
        下载图片1
  </a>
```



后端：[文件路径、读写部分 可能有误]

```java
package demo1;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.ServletOutputStream;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.FileInputStream;
import java.io.IOException;

@WebServlet("/downloadServlet")
public class ImgDownload extends HttpServlet {

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

  
		// 获取前端传过来的参数filename
        String filename = req.getParameter("filename");

        // 利用ServletContext对象来获取：路径、文件类型
        ServletContext context = this.getServletContext();

        // 获取文件的物理路径
        String realPath = context.getRealPath("/img/"+filename);


		// 设置要下载的文件的类型、以附件形式下载、下载的文件名
        String mimeType = context.getMimeType(filename);
        resp.setContentType(mimeType);
        resp.setHeader("content-disposition","attachment;filename="+filename);

        
        // 将图片读入内存
        //【经验证发现，此处代码查找的是out目录下的文件，但编写代码时，是在web/img下】，可能存在错误
        // 要想在调试时出现效果，可将/img目录复制到out目录下
        FileInputStream fis = new FileInputStream(realPath);

        byte[] buff = new byte[1024*22];
        ServletOutputStream sos = resp.getOutputStream();

        // 浏览器下载
        int len=0;
        while((len=fis.read(buff))!=-1){
            sos.write(buff,0,len);
        }

        fis.close();
    }

}

```



解决文件下载案例中，中文文件名错乱的问题。

>   思路：
>
>   -   获取浏览器的版本信息
>   -   根据浏览器版本，选择中文文件名的编码



工具类：

![解决java下载文件名的中文乱码](https://z3.ax1x.com/2021/07/12/WF8gHA.png)









### 8.6 会话技术（Cookie、Session）



**会话**：一次会话包含了多次请求+响应。



**会话的功能**：在1次会话的范围内共享数据。



**分类**：

-   客户端会话：cookie，将数据保存在客户端。
-   服务端会话：session





#### 1、cookie：

使用步骤：

>   1.  创建Cookie对象，设置数据：`new Cookie(key,value)`
>   2.  发送Cookie：`response.addCookie(cookie对象)`
>   3.  接收Cookie，获取数据：`Cookie[] request.getCookies()`



---



cookie的设置过程：

![cookie的设置过程](https://z3.ax1x.com/2021/07/13/WkZEW9.png)

---



cookie的实现原理：

>   -   在 Response的响应头中，使用 `set-Cookie：键=值`
>   -   在 Request的请求头中，使用 `Cookie:cookie值`

---



Cookie的使用细节：

>   1.  cookie可以一次使用`多个`
>
>   2.  cookie在浏览器中`保存时间`：默认关闭浏览器就销毁（可以手动设置保存时间）
>
>       -   cookie的持久化存储：
>           -   `setMaxAge(int seconds)`：正数(持久化存储时间)；负数(默认值)；0(删除cookie)
>
>   3.  cookie支持`中文`（`Tomcat8` 以及之后的版本支持中文；8之前的版本需要URL编码）
>
>   4.  cookie`获取的范围`：
>
>       -   假设`同一台Tomcat服务器`中部署了`多个web项目`，则多个项目之间`默认无法共享Cookie`
>       -   设置指定路径下的项目才能访问到某个cookie：`cookie对象.setPath(path);`
>       -   设置所有的项目都能访问到某个cookie：`cookie对象.setPath(“/");`
>       -   `不同的Tomcat服务器`之间共享Cookie可以通过`设置一级域名`：`cookie对象.setDomain(path)`
>
>   5.  浏览器对`单个Cookie`大小有限制（`不超过4KB`）;同一域名下的`Cookie个数`也有限制（`20`个）。
>
>       ​	
>
>       

---





案例：【页面显示上一次的登录时间】

```java
/**
*	获取request中的cookies数组，遍历cookie数组，如果当前遍历到的cookie的名字为“lastLoginTime”，
	则获取该cookie的值，并在页面上输出该cookie的值，然后重新生成时间并更新原来的时间【时间需要使用URL编码才能确保不乱码】
*/


package demo3;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.net.URLDecoder;
import java.net.URLEncoder;
import java.text.SimpleDateFormat;
import java.util.Date;

@WebServlet("/RemenberLoginTime")
public class RemenberLoginTime extends HttpServlet {

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        

        Cookie[] cookies = req.getCookies();
        resp.setContentType("text/html;charset=utf-8");

        if(cookies!=null && cookies.length>1){
            for (Cookie cookie : cookies) {
                if("lastLoginTime".equals(cookie.getName())){

                    
                    String value = cookie.getValue();

                    resp.getWriter().write("<h1>上一次的登录时间为："+URLDecoder.decode(value,"utf-8")+"</h1>");
                    Date date = new Date();
                    SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");
                    String format_date = sdf.format(date);
                    String s = URLEncoder.encode(format_date, "utf-8");
                    cookie.setValue(s);
                    cookie.setMaxAge(60*3);
                    resp.addCookie(cookie);
                    System.out.println("上一次的登录时间为："+format_date);

                }
            }
        }else{

            
            Date date = new Date();
            SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");
            String format_date = sdf.format(date);
            String s = URLEncoder.encode(format_date, "utf-8");
            Cookie lastLoginTime = new Cookie("lastLoginTime", s);
            lastLoginTime.setMaxAge(60*3);

            resp.setCharacterEncoding("utf-8");
            resp.addCookie(lastLoginTime);
            System.out.println("首次的登录时间为："+format_date);

            resp.getWriter().write("<h1>首次的登录时间为："+format_date+"</h1>");
        }

    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doPost(req, resp);
    }

}

```





#### 2、JSP：

-   概念：JSP（Java Server Page）：Java服务端的页面。页面中既可以写HTML，也可以写Java代码。

-   格式：
    -   格式-1：`<% java代码 %>`：转化成`.java`文件后，出现在`service方法`中
    -   格式-2：`<%! java代码 %>`：定义的是Java类的`成员`（变量、方法等）
    -   格式-3：`<%= java代码 %>`：转化成`.java`文件后，出现在`service方法`中，定义的内容会`输出`到页面上。

-   作用：简化书写。

-   实质：JSP就是一种 Servlet，自动帮我们输出了一些HTML代码。

---



下图：【JSP的解析过程】

![jsp的解析过程](https://z3.ax1x.com/2021/07/15/WeqTP0.png)



-----



JSP的内置对象：

-   Request
-   Response
-   out：输出，类似：`response.getWriter();`【需要导入：`jsp-api.jar`】



---



`out.write() ` 与 `response.getWriter().write()`的区别：

>   out对象：定义在哪个位置，就在哪个位置输出。
>
>   response对象的输出：永远在out对象前面先输出。（因为Tomcat先找Reponse对象的缓冲区，再找out对象的缓冲区）

【建议】：统一使用 `out` 对象来输出到页面。





---





#### 3、session：



-   概念：Session是将 **一次会话的多次请求** 中的`任意类型、任意大小的数据`保存在服务器的一种方式，类似Cookie的作用。

---

使用：

1.  获取Session：`request.getSession(session的名字)；`
2.  使用Session：

>   HttpSession对象的常用方法：
>
>   ​	1、`getAttribute(String name)`
>
>   ​	2、`setAttribute(String name,Object value)`
>
>   ​	3、`removeAttribute(String name)`





---



Session是依赖Cookie的，当Client请求Session时，Server如果没有Session，就在Server中开辟内存来存放Session。将开辟出来的Session设置1个ID（JSessionID），在返回Request时，在Request对象的请求头中`Set-Cookie:具体的SessionID`。

---



保证2次请求的Session是同一个Session的过程：

![Session的使用过程](https://z3.ax1x.com/2021/07/15/Wm3yx1.png)



---



Session的细节：

>   1、Client关闭、Server开启：两次请求的Session默认不是同一个。( 原因：client关闭后，清除了cookie)
>
>   ```java
>   // 解决不是同一个的问题：
>   
>   HttpSession session = request.getSession();
>   
>   Cookie c = new Cookie("JSessionID",session.getId());
>   
>   c.setMaxAge(60 * 60); // 保存1小时
>   
>   response.addCookie(c);
>   ```
>
>    
>
>   
>
>   2、Client开启、Server关闭：两次请求的Session不是同一个。
>
>   ```java
>   /*
>   *	解决Server关闭后Session丢失的问题：
>   
>   		1、应用场景：购物车
>   
>   		2、解决方案：
>           		- Session的钝化：服务器关闭前，将 Session序列化 到硬盘上。
>           		- Session的活化：服务器启动后，将Session文件转化为内存中的Session对象。
>   *
>   */
>   ```
>
>    
>
>   
>
>   3、Session的销毁  ：
>
>   -   服务器 关闭时销毁。
>   -   Session对象调用`inValidate()`
>   -   默认的失效时间：`30分钟`【可以在 `Tomcat -> conf -> web.xml->sessionConfig标签`中修改失效的时间】
>
>   





Session 与Cookie的区别：

>   1、Session存放在服务端，Cookie存放在客户端。
>
>   2、Session可以存放任意类型、大小的数据；Cookie的大小和数量由浏览器来限制。
>
>   3、Session相对安全，Cookie相对不安全。





---







#### 【 案例】：Session来实现验证码：

![验证码-案例需求](https://z3.ax1x.com/2021/07/15/WmdhIx.png)

![验证码登录-案例思路](https://z3.ax1x.com/2021/07/15/WmB5iq.png)





依赖包：

-   `jsp-api.jar`：从Tomcat的lib中复制
-   `servlet-api.jar`：从Tomcat的lib中复制
-   `mysql-connector-java-5.0.4-bin.jar`：百度下载



注意：

>   -   此案例的`MySQL驱动`需要放在`web/WEB-INF/lib`目录下，然后再`关联添加到项目`依赖中。



`DBUtils.class`：

```java
package dbutils;

import java.sql.*;

public class DBUtils {

    private static final String  url = "jdbc:mysql://localhost:3306/school_1?serverTimezone=Asia/Shanghai";
    private static final String user = "javaUser";
    private static final String psw = "javaUser";
    private static Connection conn = null;


    static {
        try {
            Class.forName("com.mysql.jdbc.Driver");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }

// 建立连接、
    public static Connection getConn() {

        try {
            conn = DriverManager.getConnection(url,user,psw);
        }catch (Exception e){
            e.printStackTrace();
        }
        return conn;
    }
// 释放连接
    public static void releaseConn(Connection conn, PreparedStatement ps, ResultSet rs) throws SQLException {

        if (rs != null) {
            rs.close();
        }

        if (ps != null) {
            ps.close();
        }

        if (conn != null) {
            conn.close();
        }
    }

}

```





`index.jsp`页面：

```jsp
<%--
  Created by IntelliJ IDEA.
  User: y
  Date: 2021/7/16
  Time: 17:56
  To change this template use File | Settings | File Templates.
--%>

<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <meta charset="UTF-8">
    <title>登录页</title>
  </head>
  <body>
    <form action="/demo/loginServlet" method="post">
        <table border="0">
          <tr>
            <td>用户名：</td>
            <td><input type="text" name="username"></td>
          </tr>
          <tr>
            <td>密码：</td>
            <td><input type="text" name="psw"></td>
          </tr>
          <tr>
            <td>验证码</td>
            <td><input type="text" name="checkcode"></td>
            <td><img id="checkcode" src="/demo/checkcodeServlet?" alt="验证码"></td>
            <td style="color: red;">
              <%=
                request.getAttribute("checkcode_error")==null?"":request.getAttribute("checkcode_error")
              %>&nbsp;&nbsp;
              <%=
              request.getAttribute("login_error")==null?"":request.getAttribute("login_error")
              %>

            </td>
          </tr>
          <tr>
            <td><input type="submit" value="登录"></td>
          </tr>
        </table>

    </form>


    <script>
      let img = document.getElementById("checkcode");
        
      img.onclick = ()=>{
         let time = new Date().getTime();
         img.src = "/demo/checkcodeServlet?"+time;
      }

    </script>
  </body>
</html>

```



`checkCodeServlet`页面：

```java
package login;

import javax.imageio.ImageIO;
import javax.servlet.*;
import javax.servlet.http.*;
import javax.servlet.annotation.*;
import java.awt.*;
import java.awt.image.BufferedImage;
import java.io.IOException;
import java.util.Random;

@WebServlet("/checkcodeServlet")
public class checkcodeServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doPost(request, response);
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        
        int width = 100;
        int height = 40;
        
        BufferedImage image = new BufferedImage(width,height,BufferedImage.TYPE_INT_RGB);

        Graphics g = image.getGraphics();
        
		// 画背景色
        g.setColor(Color.pink);
        g.fillRect(0,0,width,height);

		// 画边框
        g.setColor(Color.black);
        g.drawRect(0,0,width,height);

        // 画验证码
        g.setColor(Color.black);
        String str = "ABCDEFGHIJKLMOPQRSTUVWXYZ0123456789";
        Random rd = new Random();
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < 5; i++) {
            int id = rd.nextInt(str.length());
            char ch = str.charAt(id);
            g.drawString(ch+"",15+15*i,height/2);
            sb.append(ch);
        }
        HttpSession session = request.getSession();
        session.setAttribute("checkcode",sb);

        
        // 画干扰线
        g.setColor(Color.lightGray);
        for (int i = 0; i < 10; i++) {
            int x1 = rd.nextInt(width-1);
            int x2 = rd.nextInt(width-1);
            int y1 = rd.nextInt(height-1);
            int y2 = rd.nextInt(height-1);
            g.drawLine(x1,y1,x2,y2);
        }

        ImageIO.write(image,"jpg",response.getOutputStream());



    }
}

```







`loginServlet`页面：

```java
package login;

import javax.servlet.*;
import javax.servlet.http.*;
import javax.servlet.annotation.*;
import java.io.IOException;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import dbutils.DBUtils;


@WebServlet("/loginServlet")
public class loginServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doPost(request, response);
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        // 客户端发送的用户名、密码、验证码
        String username = request.getParameter("username");
        String psw = request.getParameter("psw");
        String u_checkcode = request.getParameter("checkcode");


        response.setCharacterEncoding("utf-8");

        // 服务端的验证码Servlet页面发送的验证码
        HttpSession session = request.getSession();
        StringBuilder checkcode1 = (StringBuilder)session.getAttribute("checkcode");
        String checkcode2= new String(checkcode1);
        session.removeAttribute("checkcode");

        // 如果验证码不为空且用户输入的验证码正确，则向数据库发送用户名、密码
        // 数据库中有符合该用户名、密码的记录，则跳转页面，否则返回登录页
        if(u_checkcode!=null && checkcode2.equalsIgnoreCase(u_checkcode)){
            try {
                Connection conn = DBUtils.getConn();
                String sql = "select * from stu where sname=? and spsw =?";
                PreparedStatement ps = conn.prepareStatement(sql);
                ps.setString(1,username);
                ps.setString(2,psw);

                ResultSet rs = ps.executeQuery();

                if(!rs.next()){
                    request.setAttribute("login_error","用户名或密码错误！！");

                    request.getRequestDispatcher("/").forward(request,response);

                }else {

                    response.sendRedirect(request.getContextPath()+"/home.jsp");

                }
            } catch (Exception e) {
                e.printStackTrace();
            }

        }else{

            request.setAttribute("checkcode_error","验证码错误");
            request.getRequestDispatcher("/").forward(request,response);

        }



    }
}

```



`home.jsp`页面：

```jsp
<%@ page import="java.text.SimpleDateFormat" %>
<%@ page import="java.util.Date" %>
<%@ page import="java.net.URLEncoder" %>
<%@ page import="java.net.URLDecoder" %><%--
  Created by IntelliJ IDEA.
  User: y
  Date: 2021/7/16
  Time: 19:22
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>主页</title>
</head>
<body>
  <%
    Cookie[] cookies = request.getCookies();

    if(cookies!=null &&cookies.length>1){
      for (Cookie cookie : cookies) {
        if(cookie.getName().equalsIgnoreCase("lastLoginTime")){
          String value = cookie.getValue();
          String decode_time = URLDecoder.decode(value,"utf-8");
          response.getWriter().write("欢迎回来，上一次的登录时间为："+decode_time);

          Date date = new Date();
          SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日，HH：mm：ss");
          String format_time = sdf.format(date);
          String encode_time = URLEncoder.encode(format_time,"utf-8");

          cookie.setValue(encode_time);
          cookie.setMaxAge(60*3);
          response.addCookie(cookie);
        }

      }
    }else{
      Date date = new Date();
      SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日，HH：mm：ss");
      String format_time = sdf.format(date);
      String encode_time = URLEncoder.encode(format_time,"utf-8");
      Cookie lastLoginTime = new Cookie("lastLoginTime", encode_time);
      lastLoginTime.setMaxAge(60*3);
      response.addCookie(lastLoginTime);
      response.getWriter().write("首次登录的时间为："+format_time);

    }
  %>
</body>
</html>

```









## 9. JSP



部分内容在：`8.6-1 会话技术小节`.



-   格式-1：`<% java代码 %>`：转化成`.java`文件后，出现在`service方法`中
-   格式-2：`<%! java代码 %>`：定义的是Java类的`成员`（变量、方法等）
-   格式-3：`<%= java代码 %>`：转化成`.java`文件后，出现在`service方法`中，定义的内容会`输出`到页面上。

---



### 9.1 JSP-指令：



-   作用：在页面中导入资源文件。
-   格式：`<%@ 指令名  属性名1=属性值1 属性名2=属性值2 %>`



JSP指令的示例：`<%@ page contentType="text/html;charset=UTF-8" language="java" %>`



JSP的指令分类：

-   `page`：配置页面
-   `include`：导入页面资源
-   `taglib`：导入资源，标签库



---



**Page指令：**

​	1、ContentType属性：等价于`response.setContentType(MIME类型和字符集)`

>   -   功能：设置response的MIME类型和字符集；设置页面的字符集（高级的iDE，低级的工具使用pageEncoding属性来设置页面的字符集）



​	2、PageEncoding属性：设置页面字符集编码。

​	3、buffer属性：设置页面缓冲区大小(需要带上单位)。

​	4、import属性：导入Java的包。

​	5、errorPage属性：页面发生异常时，自动跳转到该属性所指定的页面。

​	6、isErrorPage属性：标识是否为出错时跳转到的页面，true/false。指定为true时，允许在页面中用jsp的语法显示exception对象的相关消息。

---



**include指令：**

​	1、file属性：资源的路径。如：`<%@incluse file="top.jsp" %>`

---



**taglib指令：**

​	1、使用任何标签库都需要：

>   -   `web->WEB-INF->lib`中放入`JSTL.jar`包，
>   -   导入`JSTL.jar`到项目中
>   -   页面中导入标签库：`<%@taglib  prefix="自定义的标签名"  uri="http://java.sun.com/jsp/jstl/core" %>`
>   -   使用时：`<上面自定义的标签名: if >`



---





###  9.2 JSP-注释：



-   HTML的注释：`<!-- -->`，注释会发送到浏览器。
-   JSP的注释【推荐】：`<%-- --%>` ，可以注释JSP代码和HTML代码，不会发送到浏览器。



---





### 9.3 JSP-内置对象：



JSP的 9个内置对象：

| 序号 | 变量名（JSP的内置对象名） | 实际类型            | 备注                                              |
| :--: | :-----------------------: | ------------------- | ------------------------------------------------- |
|  1   |           page            | Object              | 当前页面 this                                     |
|  2   |        pageContext        | PageContext         | 当前页面内 共享数据，可获取其他8个内置对象        |
|  3   |          request          | HttpServletRequest  | 一次请求获取多个资源（request转发）               |
|  4   |         response          | HttpServletResponse | 响应对象                                          |
|  5   |          session          | HttpSession         | 一次会话、多次请求。                              |
|  6   |        application        | ServletContext      | 所有用户共享数据                                  |
|  7   |            out            | JspWriter           | 输出数据到页面上                                  |
|  8   |          config           | ServletConfig       | Servlet配置对象                                   |
|  9   |         exception         | Throwable           | 只有`page指令`的` isErrorPage=true`时，才会生效。 |



pageContext、request、session、application：都可用于共享数据。



---



### 9.4 MVC开发模式简介：



JSP的演变历史：

>   1、一开始只有Servlet，往前端输出只能借助Presonse对象，很不方便。
>
>   2、出现JSP，简化了操作，但出现滥用JSP现象，造成代码的可读性差、难维护。
>
>   3、借鉴了MVC开发模式，使得程序设计更合理。



---



MVC开发模式：

>   -   M：mdel，模型，业务处理，如：操作数据库
>   -   V：view，视图，展示数据。
>   -   C：controller，控制器，获取输入、调用模型、将数据传给视图。



---



MVC的优点：

>   -   解耦
>   -   可重用



MVC的缺点：

>   -   使得项目的架构变复杂，对开发者要求更高



<font style="color:red;">使用了MVC模式后，JSP里不写Java代码，java代码写在EL表达式、JSTL标签中。</font>



---





### 9.5 EL表达式：



-   功能：替换、简化 JSP中的Java代码的书写。
-   语法：`${ 表达式的内容 }`
-   注意：JSP 默认支持 EL表达式。



忽略EL表达式（2种方式）：

>   -   `page指令`上的`isElIgnored属性的值设置为true`
>   -   在EL表达式前加`\`，如：`\${表达式内容}`



---



EL表达式中的运算符：

>   -   算术运算符
>   -   关系运算符
>   -   逻辑运算符
>   -   空运算符empty：`${ empty list2 }`，判断`数组、字符串、集合`是否`为空 或 长度为0`。
>   -   非运算符 not empty：`${ not empty list2 }`，判断`数组、字符串、集合`是否`为空 或 长度为0`。





---

**EL表达式获取值：**



【注意】：EL表达式<font style="color: red;">只能获取域中的值</font>。



1、EL_语法-1：`${ 域名称.键名 }`

>   El表达式语法中的`域名称`:
>
>   -   pageScope：从 pageContext 中获取数据
>   -   requestScope：从 request 对象中获取数据
>   -   sessionScope：从 session 对象中获取数据
>   -   applicationScope：从 servletContext 对象中获取数据



例如：

```jsp
// 假设request域中已经有了数据 ： name="张三"
<%
	request.setAttribute("name","张三");
	session.setAttribute("name","李四");


	// 域中设置对象
	Person user = new Person();
	user.setName("张三");
	user.setBirth(new Date());
	request.setAttribute("u",user);
	
%>

// 利用EL表达式获取数据：[第一种]

	${ requestScope.name }

	${ sessionScope.name }




// EL获取域中的对象：【域名.对象名.属性名】
//		属性：将get方法名，去掉get，再全部转换成小写字母
//			如：user对象的getBirth()方法  =》 user.birth

	${ requestScope.u.birth }




// EL获取域中的 List集合：【 域名.集合名[下标] 】
	${ requestScope.list2 }
	${ requestScope.list2[3] }


// EL获取域中的 Map集合：【 域名.集合名.键名 】
	${ requestScope.map1 }
	${ requestScope.map1.age }
```





2、EL_语法-2：`${ 键名 }`

-   该语法：先从最小的域开始找数据，没有数据再向上一级域找数据，一直找到。

```jsp
// 假设request域中已经有了数据 ： name="张三"
<%
	request.setAttribute("name","张三");
	session.setAttribute("name","李四");
	
%>

// 利用EL表达式获取数据：[第二种]	

	${ name }



```





---

EL 表达式的隐式对象【11个】：



其中的` pageContext `隐式对象同时也是 JSP的9大内置对象之一。

用法：

```jsp
// 动态获取请求路径：
/${ pageContext.request.contextPath }

    <form action="${ pageContext.request.contextPath }/login.jsp">

    </form>
```





---





### 9.6 JSTL：



**概念：** Java Server Tag Lib，Java标准标签库，是Apache免费提供的JSP标签库。

**作用：** 简化和替换部分JSP页面中的Java代码。



---





**使用步骤：**

>   1、项目中引入JSTL的JAR包：[jar包的下载地址](http://static.runoob.com/download/jakarta-taglibs-standard-1.1.2.tar.gz)
>
>   2、JSP页面中引入标签库：利用Taglib指令。`<%@ taglib prefix="自定义标签名" uri="标签库地址" %>`
>
>   3、页面中使用标签。







---



**常见的JSTL标签：**

>   -   if：必选的属性`test`，一般与`EL表达式`配合使用。
>   -   choose：相当于Java中的 `switch `语句
>   -   when：相当于`case`语句
>   -   otherwise：相当于`default`语句
>   -   forEach：相当于Java中的`for`语句，【常用属性：begin、end、var、step，varStatus】



if 标签：

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>

// 页面导入标签库
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
   
    
	// test=true时，才显示标签中的内容(类似vue)，一般与`EL表达式`配合使用。
    <c:if test="false"> 我是test属性为true时显示的内容(为false时不显示) </c:if>   
    
    
    // EL表达式与JSTL标签联合使用。
     <%
    	List arr = new ArrayList();
    	arr.add("123");
    	request.setAttribute("list_1",arr);
    
    %>
    <c:if test="${ not empty list_1 }"> 
    	arr不为空时，我才显示
    </c:if>
    
    
    
    <c:if test="${ num_1 % 2 ==1 }"> 
    	num_1为奇数时，我才显示
    </c:if>

</body>
</html>

```

---







chhose-when-otherwise标签配合使用：

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>

// 页面导入标签库
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
   
    
   
     <%
    	List arr = new ArrayList();
    	arr.add(1);	    
    	request.setAttribute("list_1",arr);
    
    %>
    <c:choose> 
    	<c:when test="${ list_1[0] == 1 }">星期一 </c:when> 
        <c:when test="${ list_1[1] == 2 }">星期二 </c:when> 
        <c:when test="${ list_1[2] == 3 }">星期三 </c:when> 
        <c:when test="${ list_1[3] == 4 }">星期四 </c:when> 
        <c:when test="${ list_1[4] == 5 }">星期五 </c:when> 
        <c:when test="${ list_1[5] == 6 }">星期六 </c:when>
        <c:when test="${ list_1[5] == 6 }">星期日 </c:when>
        <c:otherwis> 输入有误</c:otherwise>     
    </c:choose>
    


</body>
</html>


```



---



forEach标签：

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>

// 页面导入标签库
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
   
    
   
     <%
    	List arr = new ArrayList();
    	arr.add(1);	    
    	request.setAttribute("list_1",arr);
    
    %>
    
    // 【普通for循环】
    // 打印变量i, 范围：[0,9]，步长=1，
    // varStatus的属性：index，索引，从0开始
    // varStatus的属性：count，计数器，从1开始
    <c:forEach begin="0" end="9" var="i" step="1" varStatus="s" > 
    	  ${ i }  ===> ${ s.index }  ====> ${ s.count }
    </c:forEach>
    
    // 【集合的for循环】
    <c:forEach items="${list_1}"  var="i" varStatus="s"> 
    	  ${ i }  ===> ${ s.index }  ====> ${ s.count }
    </c:forEach>


</body>
</html>
```





---



### 9.7 三层架构：



-   界面层（web 层）：与用户界面交互。包的命名：`域名.项目名`
-   业务逻辑层（service 层）：处理业务逻辑。包的命名：`域名.service`
-   数据访问层（dao 层）：控制数据库的访问。包的命名：`域名.dao`



![三层架构图](https://z3.ax1x.com/2021/07/18/W3wPte.png)





项目步骤：

-   **需求分析**：要完成什么功能
-   **设计**：
    -   技术选型：例如，`JSP + Servlet + MySQL + Druid + JdbcTemplete + BeanUtils + Tomcat`
    -   数据库的设计
-   **开发**：
    -   搭建环境
        -   搭建数据库环境
        -   导入JAR包
    -   编写代码
-   **测试**：
-   **部署 + 运维**



>   使用三层架构的技巧：
>
>   -   0、创建：`jdbc工具类`
>
>   -   1、创建：`实体类`（成员变量与数据表完全对应）。
>   -   2、创建：`dao接口 + 实现类`：JDBC操作。
>   -   3、创建：`service接口 + 实现类`：new出dao实现类，调用dao实现类的方法来完成一些功能。
>   -   4、创建：`servlet`：执行页面访问时的操作。
>   -   5、创建：`JSP页面`





---





### 9.8 案例：【用户查询】



![用户列表信息查询-整体逻辑图](https://z3.ax1x.com/2021/07/18/W320Zd.png)



**目标：**

![案例目标](https://z3.ax1x.com/2021/07/19/WGiqHO.png)



`分页的格式：页数= 总记录数%每页显示数==0？总记录数%每页显示数：总记录数%每页显示数+1`



**步骤：**

>   0、创建数据库
>
>   1、创建项目
>
>   2、在`web/WEB-INF`目录下新建目录`lib`，将所有的`Jar`都放入`lib`目录下。
>
>   3、在`src`目录下创建项目所需的所有包：`实体包、web包、service包、dao包、util包`
>
>   4、在包下创建各种接口、实现类、Servlet。
>
>   5、在 `dao实现类`中使用`Druid + JdbcTemplete`获取数据，并返回给 `service实现类`
>
>   6、 `service实现类` 中`new`出`dao对象`，调用`dao对象`中的获取数据的方法，返回数据给`Servlet`
>
>   7、`servlet`获取数据到`集合`中，将`集合`存放到 `Request对象的域中`，调用`Request对象`的`请求转发` 
>
>   8、`JSP页面` 引用`JSTL库`，`JSTL和 EL表达式`获取`Request域中的数据`，`JSTL的forEach`显示数据。







#### 1、创建数据库：

```sql
create database exercise_2;

-- ---------------------
use exercise_2;

-- ---------------------
create table `user`(
	`id` int primary key auto_increment,
	`name` varchar(20) not null,
	`gender` varchar(5),
	`age` int,
	`address` varchar(32),
	`QQ` varchar(20),
	`email` varchar(50),
	`username` varchar(20) not null,
	`psw` varchar(20) not null
);

-- -------------
insert into `user`(`name`,`gender`,`age`,`address`,`QQ`,`email`,`username`,`psw`)
values('张三','男','23','浙江省杭州市西湖区','123445','123445@qq.com','zs','123');

insert into `user`(`name`,`gender`,`age`,`address`,`QQ`,`email`,`username`,`psw`)
values('李四','男','21','浙江省杭州市上城区','223445','223445@qq.com','ls','123');

insert into `user`(`name`,`gender`,`age`,`address`,`QQ`,`email`,`username`,`psw`)
values('王五','男','27','浙江省金华市','223445','323445@qq.com','ww','123');

insert into `user`(`name`,`gender`,`age`,`address`,`QQ`,`email`,`username`,`psw`)
values('马燕','女','22','浙江省台州市黄岩区','423445','423445@qq.com','my','123');
```



#### 2、目录结构：



-   所需JAR包：

![所需JAR包](Java Web-笔记/WG0DPO.png)

---



-   整体的包结构：

![整体包结构](https://z3.ax1x.com/2021/07/18/W8CpSP.png)

---



-   详细包结构：

![详细包结构](https://z3.ax1x.com/2021/07/18/W8Cskd.png)



---



#### 3、Utils包：

DBUtils.java

```java
package com.cyw.util;

import com.alibaba.druid.pool.DruidDataSourceFactory;
import javax.sql.DataSource;
import java.io.InputStream;
import java.sql.SQLException;
import java.util.Properties;

public class DBUtils {
    private static DataSource dataSource = null;

    static {
        Properties properties = new Properties();
        InputStream is = DBUtils.class.getClassLoader().getResourceAsStream("druid.properties");
        try {
            properties.load(is);
            dataSource = DruidDataSourceFactory.createDataSource(properties);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    // 返回 静态的数据库连接池
    public static DataSource getDataSource() {
        return dataSource;
    }
}

```





---



#### 4、实体包：

User.java：

```java
package com.cyw.domain;

import java.util.Objects;

public class User {
    private int id;
    private String name;
    private String gender;
    private int age;
    private String address;
    private String qq;
    private String email;
    private String username;
    private String psw;

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", gender='" + gender + '\'' +
                ", age=" + age +
                ", address='" + address + '\'' +
                ", qq='" + qq + '\'' +
                ", email='" + email + '\'' +
                ", username='" + username + '\'' +
                ", psw='" + psw + '\'' +
                '}';
    }

    public User() {
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getGender() {
        return gender;
    }

    public void setGender(String gender) {
        this.gender = gender;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public String getQq() {
        return qq;
    }

    public void setQq(String qq) {
        this.qq = qq;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPsw() {
        return psw;
    }

    public void setPsw(String psw) {
        this.psw = psw;
    }

    public User(int id, String name, String gender, int age, String address, String qq, String email, String username, String psw) {
        this.id = id;
        this.name = name;
        this.gender = gender;
        this.age = age;
        this.address = address;
        this.qq = qq;
        this.email = email;
        this.username = username;
        this.psw = psw;
    }
}

```



---





#### 5、Dao包：

UserDaoImpl.java

```java
package com.cyw.dao.impl;

import com.cyw.dao.UserDao;
import com.cyw.domain.User;
import com.cyw.util.DBUtils;
import org.springframework.dao.DataAccessException;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.JdbcTemplate;
import javax.sql.DataSource;
import java.util.List;

public class UserDaoImpl implements UserDao {
    DataSource dataSource= DBUtils.getDataSource();

    private JdbcTemplate jt = new JdbcTemplate(dataSource);

    @Override
    public List<User> findAll() {
       String sql = "select * from user";
        List<User> users = jt.query(sql, new BeanPropertyRowMapper<User>(User.class));
        return users;
    }

    @Override
    public User findUserByUsernameAndPsw(String username,String psw) {

        String sql = "select * from user where username=? and psw=?";        
        User user = null;
        
        try {
            user = jt.queryForObject(sql, new BeanPropertyRowMapper<User>(User.class), username, psw);
        } catch (DataAccessException e) {
            e.printStackTrace();
        }
        
        return user;
    }


}

```



---



#### 6、Service包

UserServiceImpl.java

```java
package com.cyw.service.impl;

import com.cyw.dao.UserDao;
import com.cyw.dao.impl.UserDaoImpl;
import com.cyw.domain.User;
import com.cyw.service.UserService;

import java.util.List;

public class UserServiceImpl implements UserService {
    private UserDao dao = new UserDaoImpl();
    /**
     * 调用 dao层的方法
     * @return
     */
    @Override
    public List<User> findAll() {
        return dao.findAll();
    }

    @Override
    public User login(User user) {
        try {
            return  dao.findUserByUsernameAndPsw(user.getUsername(), user.getPsw());
        } catch (Exception e) {
            e.printStackTrace();
            return  null;
        }
    }
}

```











#### 7、Web包（Servlet包）：



---



##### 验证码Servlet：

```java
package com.cyw.web.servlet;

import javax.imageio.ImageIO;
import javax.servlet.*;
import javax.servlet.http.*;
import javax.servlet.annotation.*;
import java.awt.*;
import java.awt.image.BufferedImage;
import java.io.IOException;
import java.util.Random;

@WebServlet("/checkCodeServlet")
public class checkCodeServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doPost(request, response);
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        // 禁用验证码的缓存
        response.setHeader("pragma", "no-cache");
        response.setHeader("cache-control", "no-cache");
        response.setHeader("expires", "0");

        int width = 80;
        int height = 30;
        BufferedImage image = new BufferedImage(width, height, BufferedImage.TYPE_INT_RGB);

        Graphics g = image.getGraphics();

        // 画背景色
        g.setColor(Color.gray);
        g.fillRect(0, 0, width, height);

        // 画边框
        g.setColor(Color.black);
        g.drawRect(0, 0, width, height);


        // 画验证码
        g.setColor(Color.yellow);
        String checkCode = getRandomCheckCode();
        HttpSession session = request.getSession();
        session.setAttribute("checkCode", checkCode);
        g.setFont(new Font("黑体", Font.BOLD, 24));
        g.drawString(checkCode, 15, 25);

        ImageIO.write(image, "jpg", response.getOutputStream());

    }


    // 随机生成验证码
    private String getRandomCheckCode() {
        String str = "ABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890";
        Random rd = new Random();
        String rst = "";
        for (int i = 0; i < 4; i++) {
            int idx = rd.nextInt(str.length());
            char ch = str.charAt(idx);
            rst += ch;
        }
        return rst;
    }
}

```



---



##### 登录Servlet:

```java
package com.cyw.web.servlet;

import com.cyw.domain.User;
import com.cyw.service.impl.UserServiceImpl;
import org.apache.commons.beanutils.BeanUtils;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;
import java.lang.reflect.InvocationTargetException;
import java.util.Map;

@WebServlet("/loginServlet")
public class loginServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 设置编码
        resp.setCharacterEncoding("utf-8");

        // 获取表单数据
        String verifycode = req.getParameter("verifycode");

        // 验证码校验
        HttpSession session = req.getSession();
        String checkCode = (String)session.getAttribute("checkCode");
        session.removeAttribute("checkCode");
        
        if(!checkCode.equalsIgnoreCase(verifycode)){
            // 验证码不正确
            req.setAttribute("login_err","验证码错误！！");
            req.getRequestDispatcher("/login.jsp").forward(req,resp);
            return;

        }else{
            
            
			// 【注意】：表单的name属性与User实体类的属性必须完全一致b
            Map<String, String[]> map = req.getParameterMap();
            // 封装User对象
            User user = new User();
            try {
                BeanUtils.populate(user,map);
            } catch (IllegalAccessException e) {
                e.printStackTrace();
            } catch (InvocationTargetException e) {
                e.printStackTrace();
            }

            // 调用Service
            UserServiceImpl service = new UserServiceImpl();
            User loginUser = service.login(user);

            // 判断是否登录成功
            if (loginUser!=null){
                // 登录成功，用户存入session,跳转页面
                session.setAttribute("user",loginUser);
                resp.sendRedirect(req.getContextPath()+"/index.jsp");

            }else{
                // 登录失败，提示失败，返回登录页
                req.setAttribute("login_err","用户名或密码错误！！");
                req.getRequestDispatcher("/login.jsp").forward(req,resp);

            }

        }
    }
}

```



---





##### 用户列表查询Servlet:

```java
package com.cyw.web.servlet;

import com.cyw.domain.User;
import com.cyw.service.UserService;
import com.cyw.service.impl.UserServiceImpl;
import javax.servlet.*;
import javax.servlet.http.*;
import javax.servlet.annotation.*;
import java.io.IOException;
import java.util.List;

@WebServlet( "/userListServlet")
public class userListServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doPost(request, response);
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        UserService service = new UserServiceImpl();
        List<User> users = service.findAll();
        request.setAttribute("users",users);
        request.getRequestDispatcher("/list.jsp").forward(request,response);
    }
}

```



---





#### 8、登录页

login.jsp

```java
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="utf-8"/>
    <meta http-equiv="X-UA-Compatible" content="IE=edge"/>
    <meta name="viewport" content="width=device-width, initial-scale=1"/>
    <title>管理员登录</title>

    <!-- 1. 导入CSS的全局样式 -->
    <link href="css/bootstrap.min.css" rel="stylesheet">
    <!-- 2. jQuery导入，建议使用1.9以上的版本 -->
    <script src="js/jquery-2.1.0.min.js"></script>
    <!-- 3. 导入bootstrap的js文件 -->
    <script src="js/bootstrap.min.js"></script>

    <script type="text/javascript">
        let img = document.getElementById("vcode");
        img.onclick = () => {
            let time = new Date().getTime();
            img.src = "${pageContext.request.contextPath}/checkCodeServlet?time=" + time;
        }

    </script>
</head>
<body>
<div class="container" style="width: 400px;">
    <h3 style="text-align: center;">管理员登录</h3>
    <form action="${ pageContext.request.contextPath}/loginServlet" method="post">
        <div class="form-group">
            <label for="user">用户名：</label>
            <input type="text" name="username" class="form-control" id="user" placeholder="请输入用户名"/>
        </div>

        <div class="form-group">
            <label for="password">密码：</label>
            <input type="password" name="psw" class="form-control" id="password" placeholder="请输入密码"/>
        </div>

        <div class="form-inline">
            <label for="vcode">验证码：</label>
            <input type="text" name="verifycode" class="form-control" id="verifycode" placeholder="请输入验证码"
                   style="width: 120px;"/>
            <a href="javascript:refreshCode()"><img src="${pageContext.request.contextPath}/checkCodeServlet"
                                                    title="看不清点击刷新" id="vcode"/></a>
        </div>


        <hr/>
        <div class="form-group" style="text-align: center;">
            <input class="btn btn btn-primary" type="submit" value="登录">
        </div>
    </form>

    <!-- 出错显示的信息框 -->
    <div class="alert alert-warning alert-dismissible" role="alert">
        <button type="button" class="close" data-dismiss="alert">
            <span>&times;</span></button>
        <strong>${ login_err }</strong>
    </div>
</div>
</body>
</html>
```





#### 9、首页

index.jsp

```java

<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="utf-8"/>
  <meta http-equiv="X-UA-Compatible" content="IE=edge"/>
  <meta name="viewport" content="width=device-width, initial-scale=1"/>
  <title>首页</title>

  <!-- 1. 导入CSS的全局样式 -->
  <link href="css/bootstrap.min.css" rel="stylesheet">
  <!-- 2. jQuery导入，建议使用1.9以上的版本 -->
  <script src="js/jquery-2.1.0.min.js"></script>
  <!-- 3. 导入bootstrap的js文件 -->
  <script src="js/bootstrap.min.js"></script>
  <script type="text/javascript">
  </script>
</head>
<body>
<div align="center">

  <div>${ user.name}，欢迎你！</div>

  <a
          href="${ pageContext.request.contextPath}/userListServlet" style="text-decoration:none;font-size:33px">查询所有用户信息
  </a>
</div>
</body>
</html>
```



#### 10、用户查询页

list.jsp

```java

<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<!-- 网页使用的语言 -->
<html lang="zh-CN">
<head>
    <!-- 指定字符集 -->
    <meta charset="utf-8">
    <!-- 使用Edge最新的浏览器的渲染方式 -->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <!-- viewport视口：网页可以根据设置的宽度自动进行适配，在浏览器的内部虚拟一个容器，容器的宽度与设备的宽度相同。
    width: 默认宽度与设备的宽度相同
    initial-scale: 初始的缩放比，为1:1 -->
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>用户信息管理系统</title>

    <!-- 1. 导入CSS的全局样式 -->
    <link href="css/bootstrap.min.css" rel="stylesheet">
    <!-- 2. jQuery导入，建议使用1.9以上的版本 -->
    <script src="js/jquery-2.1.0.min.js"></script>
    <!-- 3. 导入bootstrap的js文件 -->
    <script src="js/bootstrap.min.js"></script>
    <style type="text/css">
        td, th {
            text-align: center;
        }
    </style>
</head>
<body>
<div class="container">
    <h3 style="text-align: center">用户信息列表</h3>
    <table border="1" class="table table-bordered table-hover">
        <tr class="success">
            <th>编号</th>
            <th>姓名</th>
            <th>性别</th>
            <th>年龄</th>
            <th>籍贯</th>
            <th>QQ</th>
            <th>邮箱</th>
            <th>操作</th>
        </tr>

        <c:forEach items="${ users }" var="user" varStatus="s">
            <tr>
                <td>${ s.count }</td>
                <td>${ user.name }</td>
                <td>${ user.gender}</td>
                <td>${ user.age}</td>
                <td>${ user.address}</td>
                <td>${ user.qq}</td>
                <td>${ user.email}</td>
                <td><a class="btn btn-default btn-sm" href="update.html">修改</a>&nbsp;<a class="btn btn-default btn-sm" href="">删除</a></td>
            </tr>
        </c:forEach>

        <tr>
            <td colspan="8" align="center"><a class="btn btn-primary" href="add.html">添加联系人</a></td>
        </tr>
    </table>
</div>
</body>
</html>


```





---



## 10 Filter 过滤器

>   [Filter+Listener视频](https://www.bilibili.com/video/BV1vt411R72s?from=search&seid=12014235413821499147)



JavaWeb的三大组件：`Servlet + Filter + Listener`



-   **概念**：Request请求访问服务器时，可以被过滤器拦截，并实现一些特殊的功能。

-   **作用**：完成通过的操作。如：登录验证、统一编码校验、敏感词过滤。

步骤：

>   1、定义类，实现 Filter接口。
>
>   2、覆盖 重写方法。
>
>   3、配置 <font style="color:red;">拦截路径</font> 。





---



### 1、过滤器的实现方式（2种）：

-   `注解`
-   `web.xml`









#### 1.1 注解-实现过滤器：

```java
package com.cyw.filterdemo;

import javax.servlet.*;
import javax.servlet.annotation.WebFilter;
import java.io.IOException;

// 配置过滤路径
@WebFilter("/*")
public class FilterDemo implements Filter {		// 实现Filter接口，重写3个生命周期方法
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {

        System.out.println("过滤器实现类-doFilter方法执行");

        // 过滤器放行
        filterChain.doFilter(servletRequest,servletResponse);
    }

    @Override
    public void destroy() {

    }
}

```



#### 1.2 过滤器中 web.xml配置：



web.xml 实现过滤器：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    
    
<!--
	1、定义过滤器名、设置过滤器的全类名
 	2、
-- >
    <filter>
        <filter-name>demo1</filter-name>
        <filter-class>com.cyw.filterdemo.FilterDemo</filter-class>
    </filter>
    
    <filter-mapping>
        <filter-name>demo1</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>


</web-app>
```



---

### 2、Filter的生命周期：

-   `init()`：服务器启动后，创建Filter对象，执行一次 init 方法。
-   `doFilter()`：request请求被拦截的资源时，执行。
-   `destroy()`：服务器正常关闭时，执行 destroy 方法。



---



### 3、Filter的执行流程：

-   执行过滤器
-   执行过滤后的放行资源
-   再次执行过滤器方向代码下的代码。





---





### 4、过滤器的配置：



 【位置：`@webFilter("拦截路径")` 或者 `we.xml`】



1、拦截路径：

>   -   拦截具体的资源：
>       -   例如：拦截路径配为：`/index.jsp`，则访问`index.jsp`页面时，执行过滤器。
>
>    
>
>   -   拦截某个目录：
>
>       -   例如：拦截路径配为：`/user/*`，访问 user 目录下的所有资源时，执行过滤器。
>
>        
>
>   -   拦截文件的后缀名：
>
>       -   例如：拦截拦截配为：`*.jsp`，访问所有以`.jsp`结尾的文件时，执行过滤器。





2、拦截方式（资源被访问的方式）：

>   -   资源被访问的方式：
>       -   重定向
>       -   请求转发
>
>       
>
>   -   拦截方式的配置：
>
>       -   注解：设置@WebFilter注解的dispatcherType属性
>           -   `REQUEST`：默认值，拦截请求的资源
>           -   `FORWARD`：
>           -   `INCLUDE`：
>           -   `ERROE`：
>           -   `ASYNC`：拦截异步访问资源。
>       -   `web.xml`：`<dispatcher>  </dispatcher>`
>
>   





### 5、 过滤器的执行顺序：



假设有2个过滤器，执行顺序：

>   -   过滤器1
>   -   过滤器2
>   -   资源
>   -   过滤器2
>   -   过滤器1



具体顺序：

>   -   注解形式-配置过滤器的过滤路径：比较过滤器的类名，名字字母小的先执行。
>   -   web.xml 形式-配置过滤器：在上面的先执行。









---





## 11、Listener 监听器：

监听 Session、Request、ServletContext的创建、销毁。

使用：类似Servlet和Filter。











## 12、代理模式:



设计模式（23种）：解决某种问题的固定格式。

---



-   代理模式：【真实对象（手机厂家） + 代理对象（手机店）】

>   代理模式的分类：
>
>   -   静态代理：有一个专门的类文件
>   -   动态代理：在内存中生成一个类文件



---





实现步骤：

>   1、真实对象和代理对象`实现相同的接口`。
>
>   2、获取代理对象的实例，【该方法来自`java.lang.reflect.Proxy`】
>
>   ```java
>   
>   代理对象 = Proxy.newProxyInstance(类加载器，真实对象的接口，new InvocationHandler(){
>       
>       // 传入代理对象，代理对象调用的方法，调用方法时真实传入的参数    
>       public object invoke( Object proxy ，Method method ，Object[] args){
>           
>   		Object obj = method.invoke(真实对象,args);
>           
>           return obj;
>       }
>       
>   })
>   ```
>
>   
>
>   3、代理对象`调用方法`。
>
>   4、增强方法。

