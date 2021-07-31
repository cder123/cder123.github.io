---
title: JQuery-入门笔记
tag: JQuery
categories:
  - [后端,Java,Java Web]
  - [前端,基础]
  - [前端,JavaScript]
  
---

# Jquery-入门笔记

[toc]



## 1、Jquery快速入门



基本步骤：

>   -   下载
>   -   导入
>   -   使用



---



### 1.1 Jquery的版本简介



整体版本：

>   -   1.xxx版：（1.12.4版）兼容IE-6、7、8，不在维护（可以满足基本需求）
>   -   2.xxx版：（2.2.4）不兼容IE-6、7、8
>   -   3.xxx版：支持新版的浏览器，部分老的Jquery无法使用。



---



Jquery-xxx.js与Jquery.xxx.min.js的区别：

>   -   Jquery-xxx.js：给人看的版本，有缩进
>   -   Jquery.xxx.min.js：生产版本，经过压缩，无缩进。



---





初步使用：

```js
<!-- head标签中导入 -->

    <script src="https://apps.bdimg.com/libs/jquery/2.1.4/jquery.min.js">
    </script>



<!-- html代码 -->
        
	<div id="box">
		<div id="div1">div1</div>
		<div id="div2">div2</div>
	</div>

	<script>
		let div1 = $("#div1");
		alert(div1.html());
	</script>	
```



---





### 1.2 入口函数、回调函数





入口函数：dom文档全部加载完成时，才调用内部的函数。

```js

$(function(){
	// 入口函数，dom文档全部加载完成时，才调用
    
    // 所有的js都卸载这个位置
})

```









回调函数：将1个函数作为参数传给另一个函数，参数函数在外面的函数调用结束后自动执行。

```js
	<div id="box">
		<div id="div1">div1</div>
		<div id="div2">div2</div>
	</div>


	<script>
		let div1 = $("#div1");
	
		$("#div1").click(function(){
			alert("div1 被点击了")

		})

	</script>	
```











---





### 1.3 Jquery的选择器



基本选择器：

>-   标签：`$("标签名")`
>-   id：`$("#id")`
>-   类名：`$(".类名")`



层级选择器：

>   -   后代选择器：`$("A B")`
>   -   子孙选择器：`$("A > B")`





属性选择器：

>   -   属性名选择器：`$("元素的选择器名")[属性名]`
>   -   属性值选择器（单个属性值）：`$("元素的选择器名[属性名 = 属性值]")`
>   -   复合属性选择器（多个属性值）：`$("元素的选择器名[属性名 = 属性值][属性名 = 属性值]")`



过滤元素选择器：

>   -   首元素选择器：`:first`
>   -   尾元素选择器：`:last`
>   -   奇数选择器：`:odd`，  （从0开始的奇数，即：从1开始的偶数）
>   -   偶数选择器：`:even`，（从0开始的偶数，即：从1开始的奇数）
>   -   大于选择器：`:gt(index)`
>   -   小于选择器：`:lt(index)`
>   -   等于选择器：`:eq(index)`
>   -   非选择器：`:not(选择器)`
>   -   标题选择器：`:header`，固定写法。





表单选择器：

>   -   可用元素选择器：`:enable`
>   -   禁用元素选择器：`:disbale`
>   -   选中选择器：`:selected`，下拉框
>   -   选中选择器：`:checked`，单选、复选框



### 1.4 Jquery改变标签的CSS



-   `$("css选择器").css("css属性名","css属性值");`



```js
<div id="box">
		<div id="div1">div1</div>
		<div id="div2">div2</div>
</div>

<script>
		// 

		$(function(){
			// 入口函数，dom文档全部加载完成时，才调用
			$("#div1").css("background","red");

		})

</script>	
```





### 1.5 Jquery获取表单控件的Value值

```js
<div id="box">
		<input id="username" type="text" value="请输入用户名">
		<input id="mybtn" type="button" value="按钮1">
</div>
<script>

		$(function(){
			// 弹出内容：请输入用户名
			alert($("input[type='text']").val())			
		})

</script>	
```





### 1.6 Jquery的Dom操作：

-   获取标签体的内容（包括标签）：`html()`
-   获取标签的文本（不包括标签）：`text()`
-   获取表单控件的value值：`val()`



### 1.7 改变属性值：

-   `$("css选择器").prop("属性名"，属性值)`

```js
<input id="btn" type="button" value="切换-动画" onclick="change()">
    
    
function change(){
	$("#btn").prop("value","123")
}
```





### 1.8 案例：



1、隔行换色

```js

<ul>
    <li>1</li>
	<li>2</li>
	<li>3</li>
	<li>4</li>
	<li>5</li>
</ul>


<script>
	// 入口函数
    $(function(){

        // 选择偶数个元素的背景色改为红色
        $("ul>li:odd").css("background-color","red");
    
        $("ul>li:even").css("background-color","blued");


	})

</script>	
```



---



2、全选功能

```js
	<div id="box">
		<input  id="all" type="checkbox" value="all" onclick="selectAll(this)">
		<label for="all">全选</label> <br>

		<input class="fruit" id="apple" type="checkbox" value="苹果">
		<label for="apple">苹果</label> <br>

		<input class="fruit" id="banana" type="checkbox" value="香蕉">
		<label for="banana">香蕉</label> <br>

		<input class="fruit" id="watermelon" type="checkbox" value="西瓜">
		<label for="watermelon">西瓜</label> <br>
	</div>



	<script>

		function selectAll(obj){
			$(".fruit").prop("checked",obj.checked);
		}

	</script>	
```



---





3、下拉列表复制：

```js
<div id="box">

		<select name="" id="left" multiple="true">
			<option value="xg">西瓜</option>
			<option value="pg">苹果</option>
			<option value="xj">香蕉</option>
		</select>
		<br>		
		<input id="toRight" type="button" value="=>">
		<br>
		<input id="toLeft" type="button" value="<=">		
		<br>
		<select name="" id="right" multiple="true">
			

		</select>
</div>
<script>

         $(function(){
                // 复制到右边
                $("#toRight").click(function(){
                    $("#right").append($("#left>option:selected").clone())

                })


                // 复制到左边
                $("#toLeft").click(function(){
                    $("#right>option:selected").appendTo($("#left"))

                })

        })
</script>	
```





## 2、Jquery高级





### 2.1 动画



显示和隐藏：

>   参数：
>
>   -   速度：（slow | normal | fast | 毫秒数）
>
>   -   擦除效果的取值：（swing 慢快慢 |  linear 匀速）
>
>   
>
>   1、默认的显示+隐藏：
>
>   -   show(速度，擦除效果，回调函数)
>   -   hide(速度，擦除，回调函数)
>   -   toggle(速度，擦除，回调函数)
>
>    
>
>   2、滑动的显示+隐藏：
>
>   -   slideDown(速度，擦除，回调函数)
>   -   slideUp(速度，擦除，回调函数)
>   -   slideToggle(速度，擦除，回调函数)
>
>    
>
>   3、淡入淡出显示+隐藏：
>
>   -   fadeIn(速度，擦除，回调函数)
>   -   fadeOut(速度，擦除，回调函数)
>   -   fadeToggle(速度，擦除，回调函数)



```js
<div id="box">

		<input id="btn" type="button" name="" value="切换-动画" onclick="change()"></button>
		<div id="demo">123</div>
</div>
<script>

		function change(){
			$("#demo").toggle("normal","swing")			
		}
</script>	
```





### 2.2 循环

-   `for(let i=0;i<10;i++){}`
-   `Jquery对象.each(function(index,element){})`
-   `$.each(obj,回调函数)`
-   `for(item of arr) `





### 2.3 事件绑定与解绑

-   事件绑定：`$("css选择器").on("事件"，回调函数)`
-   事件解绑：`$("css选择器").off("事件")`

```js
<div id="box">
		<input id="btn" type="button" name="" value="切换-动画" onclick="change()"></button>
		<div id="demo">123</div>
</div>
<script>
			$(function(){
				$("#btn").on("mouseover",function(){
					alert("123")
				})
            
            	//解绑
            	$("#btn").on("click",function(){
					$("#btn").off("click")
				})
			})
</script>	
```









### 2.4 Ajax



-   `$.ajax({键值对})`

>    常见参数：
>
>   1.  url：目标地址
>   2.  type：get | post
>   3.  data：自定义的参数对象
>   4.  success：请求成功的回调函数 
>   5.  error：请求失败的回调函数 
>   6.  dataType：接收的文件类型【`xml` |  `json`  |  `text` |  `html` | `script` | `jsonp`】



---





```js


$.ajax({
    url:"/demoServlet",
    type:"post",
    data:{
        "username":"zhangsan",
        "psw":"123"
    },
    success:function(){
        // 请求成功时调用的函数
    },
    dataType:"text"
    
    
})
```



---





-   `$.get()`

>   1.  url：目标地址
>   2.  data：参数对象
>   3.  回调函数
>   4.  type：接收的数据类型

```js
$.get(
   "/login.Servlet",
    data:{
      "username":"zhangsan"  
    },
    function(){
        
    },
    "text"    
)
```



---



-   `$.post()`







### 2.5 JSON

json是一种以键值对形式存储数据的格式，每个键值对之间使用逗号分隔。（键需要有引号包裹）



```js
let persons = {
    "p1":{"name":"张三","age":12},
    "p2":{"name":"李四","age":23}
}

// 取值：对象[键]
let p1 = psersons["p1"]
```



#### 1、JSON解析库

>   -   jsonlib
>   -   fastjson
>   -   jackson
>   -   gson



---



#### 2、Java对象转Json

以Jackson解析库为例：

步骤：

>   1、导入jackson的相关jar包
>
>   2、创建Jackson的核心对象：ObjectMapper对象
>
>   3、调用ObjectMapper对象的转化方法`writeValueAsString(obj)`



常用注解：(放在需要**被转化的类**的某个属性上)

-   `@JsonIgnore`
-   `@JsonFormat(pattern=“相应的格式”)`



---





#### 3、Json转Java对象

步骤：

>   1、导入jackson的相关jar包
>
>   2、创建Jackson的核心对象：ObjectMapper对象
>
>   3、调用ObjectMapper对象的转化方法`readValue(json字符串,java类.class)`
>
>   4、将Java的Response对象的数据返回格式：`resp.setContentType("Application/json;charset=utf-8")`

