---
title: JavaScript-笔记
tag: JavaScript
categories:
  - [前端,基础]
  - [前端,JavaScript]
---

# JavaScript-笔记

[toc]

## 1. 数据类型 + 类型转化

-   string
-   boolean
-   number
-   object
-   null
-   undefined



### 1.1 String  => Number

【直接查看字符串的首字符 是否 为数字=》不是 =》NaN】

-   parseInt( )

```js
parseInt("123abc")   // 123

parseInt("22.5") 	 // 22

parseInt("abc")		 // NaN
```



-   parseFloat( )

```js
parseFloat("123abc")	// 123.0

parseFloat("123.23.4")	// 123.23

parseFloat("abc")		// NaN
```

### 1.2  Number => String  

-   toString( )：转化为字符串
-   toFix( 保留的小数位数 )：四舍五入后，转化为字符串

## 2. 数组

### 2.1 定义：

```js
var arr = [1,2,3,4]

var arr1 = new Array(1,2,3,4)

var arr2 = new Array(size)


console.log(arr)	//打印整个数组
console.log(arr[0]) //打印数组第一个元素
```



### 2.2 数组长度：

arr.length ，长度可被修改

```js
var arr = [1,2,3,4]

console.log(arr.length)	//4

arr.length = 10
```



### 2.3 数组元素 + 数组属性 的修改

【下标：若不是数字，则当作属性】

```js
var arr = [1,2,3,4]

arr[4] = 5

console.log(arr)	//打印整个数组


arr['uname']='张三'	// 向数组中添加一个属性uname ,uname的属性值为张三
```



### 2.4 数组的 遍历

```js
var arr = [1,2,3,4]

// for()：不遍历属性

	for(let i=0;i<arr.length;i++){
        console.log(arr[i])        
    }







// for... in：不遍历 索引和属性的undefined

	for(let index in arr){
        console.log(index)  
        console.log(arr[index])  
    }






// forEach()：不遍历 索引的undefined
	
	ar.forEach(function(element,index){
         console.log(index)  
        console.log(element)
    })

	
```

### 2.5 数组的 常用方法

```js
/*
 * arr.push(元素)：末尾插入
 * arr.pop()：末尾删除
 * arr.unshift(元素)：首部插入
 * arr.shift(元素)：首部删除
 * arr.indexOf(字符)：查找字符的索引
 * arr.reverse()：反转，改变原数组
 * arr.concat(数组2)：数组合并,不改变原数组
 * arr.join(字符串连接符)：将 数组元素 按照连接符  合并为 字符串
 * arr.slice(startIndex,endIndex):切片，不改变原数组=> [start,end)
 * arr/splice(startIndex,count,插入元素1，插入元素2)：替换/删除，改变原数组
 * 		
*/
```

## 3. 函数

定义：

```js
// 1. 
		function add(a,b){
            return a+b;
        }

		add(1,2);


// 2. 
		let add = function(a,b){
            return a+b;
        }
        
        add(1,2);


// 3. 
		let add = new Function('a','b','return a+b')
        
        add(1,2);


```

## 4.  内置对象 +对象

### 4.1 内置对象

-   string
-   date
-   math
-   array
-   arguments





### 4.2 对象

#### 4.2.1. 定义：

```js
let obj = {
    name:'张三'，
    age: 20    
}

obj.age = 15;

console.log(obj.name)
```



#### 4.2.2. 序列化 + 反序列化

-   序列化 ：对象  =转化为= 》 字符串

-   反序列化： 字符串  =转化为= 》对象

```js
JSON.stringify(obj)

JSON.parse(str)
```

#### 4.2.3. this

默认指向window对象。谁调用，this就指向谁

```js
let print = function(){
    this.name = 15
    console.log(this.name)
}
```

## 5. 事件

**注意**：

1.  js最好写html后面，防止加载js时还没有html标签，造成程序报错
2.  若一定想要写在html前面，则需要在 js 外面套函数：window.onload = function( ){… }



### 5.1 事件定义：

**事件** = 事件源 + 监听器+ 事件名 + 处理

【3种事件】

```js
// 1. html事件
//			 事件源:input
//			 监听器:onclick
//			 事件名 + 处理: alert('hello world')
	<input type="button" value="点击" id='btn1' onclick="alert('hello world')">
        
    
        
        
// 2. dom0级事件 【多次给某个事件添加函数时，只有最后一次添加的函数生效】
        <input type="button" value="点击" id='btn1'>
        
        let Obtn1 = document.getElementById('btn1');

		Obtn1.onclick = function(){
            alert('hello world')
        }



// 3. dom2级事件【其中事件:冒泡/捕获的bool值可以 省略,默认：事件冒泡 】
//				Obtn1.addEventListener(事件名，处理函数，是否使用事件捕获=false)
//				Obtn1.removeEventListener(事件名，处理函数，是否使用事件捕获=false)

		<input type="button" value="点击" id='btn1'>
            
        let Obtn1 = document.getElementById('btn1');

		Obtn1.addEventListener('click'，function(){alert('hello')}，false)
        Obtn1.removeEventListener('click'，function(){alert('hello')}，false)

		
        
```

### 5.2 常用事件：

>-   window.onload
>
>
>
>鼠标：
>
>-   on click
>-   on doublec lick
>-   on mouse over
>-   on mouse out
>
>
>
>键盘：
>
>-   on key down
>-   on key up
>
>
>
>文本框
>
>-   on focus
>-   on blur
>
>
>
>select+option标签
>
>-   on change





事件流：【事件的触发顺序】

-   事件冒泡【默认的事件流】：内层标签  =传递到=》外层标签
-   事件捕获：外层标签  =传递到=》内层标签



## 6. BOM对象

### 6.1 弹窗：

-   alert( ‘警告信息’)
-   prompt( ‘ 提示’)：输入弹窗，获取字符串
-   confirm( ‘ 提示’)：确认框，返回 bool值





### 6.2 页面【打开、关闭】：

-   window.open(网址str)
-   window.close( )：只适用于 关闭window.open( )打开的页面

```js
window.open()	// 打开空白页面

window.open('./abc.html')	// 打开本地页面

window.open('www.baidu.com')	// 打开网络页面

window.open('www.baidu.com'，‘_blank’)	// 以新页面方式，打开网络页面

window.close( )	//关闭当前打开的页面
```







### 6.3 时间日期：

```js
let d = new Date()
d.toLocaleDateString()  	// "2021/5/6"
```







### 6.4 history对象：

-   window.history.length ： 历史记录数

-   window.history.back( )：后退按钮
-   window.history.forward( )：前进按钮
-   window.history.go( num | url )：跳转

```js
history.go(-1)		// 上一个页面，-1为 当前页面 与 目标url 的相对位置
```







### 6.5 location对象：

-   window.location.href：网址
-   window.location.reload( )：刷新页面
-   window.location.replace( )：替换url

```js
window.location.href = 'http://www/baidu.com'

window.location.reload( )

location.replace('http://qq.com')
```





## 7. DOM对象

### 7.1 直接 获取 dom节点：

-   document. getEmenentBy Id ( 标签 id )：`ID`
-   document. getEmenentsBy ClassName ( 标签class ) [下标]:`类名`
-   document. getElementsBy TagName ( 标签名 ) [下标]：`标签`
-   document. getElementsBy Name ( 表单元素的name属性值 ) [下标]：`表单名`





### 7.2 间接 获取 dom节点：

-   parentNode



-   previousSibling
-   nextSibling



-   childNodes
-   firstChild
-   lastChild

```JS

let ul = document. getEmenentById('uls')

console.log(ul.ChildNodes)
```







### 7.3 创建节点

-   document.createElement(标签str )
-   document.createTextNode(文本str )
-   标签1.innerHTML = html字符串



### 7.4 添加节点：

-   标签1.appendChild( 标签2)
-   标签1.innerHTML = html字符串
-   标签1.insertBefore(标签2)：`先找父节点，再调用insertBefore(标签类型，在哪个节点前)`

```js
//方式1:  appendChild
	let div1 = document.getElementById('div1');

	let p =  document.createElement('p');

	let txt = document.createTextNode('你好');

	p.appendChild(txt);
	
	div1.appendChild(p);
	



//方式2:  innerHTML

	let div1 =  document.createElement('div');
    
	div1.innerHTML = `<p>你好</p>`;
    
    
    
    
    
//方式3:  insertBefore
    // div1前 插入节点 div0
   let parent =document.getElementById('div1').parent;   
    
    let div0 =  document.createElement('div');    
    div0.innerHTML = "123";

    parent.insertBefore('div',document.getElementById('div1'))
```

### 7.5 删除结点

-   removeChild( )

```js
let div1 = document.getElementById('div1')

div1.parentNode.removeChild( div1 )		// 删除div1
```







### 7.6 修改结点的属性

-   直接访问属性后修改
-   利用 标签1.setAttribute(属性名，属性值)
-   利用  innerHTML 的字符串拼接

```js
//方式1

		let div1 = document.getElementById('div1')
		let img1 = document.createElement('img')
        
        img1.width = 200
		img1.height = 200
		img1.src = './img/pic.jpg'

		div1.appendChild(img1)

	


//方式2

		let div1 = document.getElementById('div1')
        
		let img1 = document.createElement('img')
        
        img1.setAttribute('src','./img/pic.jpg')

		div1.appendChild(img1)


//方式3
		
	  let div1 = document.getElementById('div1')
        
      div1.innerHTML +=`<img width="200px" height="200px" src="./img/pic.jpg" />`
		
```

