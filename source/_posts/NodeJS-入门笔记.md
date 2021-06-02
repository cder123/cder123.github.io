



# Node.js-入门笔记
[toc]



## 一、安装

安装教程：`https://www.runoob.com/nodejs/nodejs-install-setup.html`
【以windows为例：】
1. 【直接下载 .exe文件,然后配置环境变量】
[下载地址：](https://nodejs.org/dist/)
`https://nodejs.org/dist/`

2. 【下载 .msi安装包，配置环境变量】
[下载地址：](https://nodejs.org/en/download/)
`https://nodejs.org/en/download/`

安装完毕后，打开 cmd ，检查 node.js 版本





## 二、Node.js 基础

### 2.1 Node.js 的两种模式

- `脚本模式`：先写代码，保存为`xxx.js`后，使用 node 运行
- `交换模式`：实时运行 REPL【类似 python的 `IDLE`】，cmd中输入 `node`



### 2.2 Node应用程序的组成部分

1. 引入 required 模块：我们可以使用 require 指令来载入 Node.js 模块。
2. 创建服务器：服务器可以监听客户端的请求，类似于 Apache 、Nginx 等 HTTP 服务器。
3. 接收请求与响应请求 服务器很容易创建，客户端可以使用浏览器或终端发送 HTTP 请求，服务器接收请求后返回响应数据。



### 实例1：

1. 保存下列代码为 `1.js`
2. cmd中进入该文件所在目录，输入`node 1.js` 【1.js为文件名】

```javascript
# 请求node自带的http服务，并赋给变量http_s
	var http_s = require("http");

#  创建服务，并使用回调函数
	http_s.createServer(function(request,response){
	
		#  在浏览器中显示
		response.writeHead(200,{"Content-Type":"text/plain"});
		response.end("hello world");

	}).listen(8888);

#  在node后端显示
	console.log('Server running at http://127.0.0.1:8888/');
```
<img src="NodeJS-入门笔记/2Q6yQ0.png" alt="2Q6yQ0.png" border="0" />
![2](https://z3.ax1x.com/2021/06/02/2Q64Y9.png)

小结：

>   实例1-要点：
>
>   - 引入 `http`模块
>   - 使用`http`模块的 `createServser( ).listen(端口号)`





### 2.3  NPM 包管理器

注意：【一般用` 淘宝的cnpm`代替 npm 】



下载 NPM【使用淘宝镜像】:
`npm install -g cnpm --registry=https://registry.npm.taobao.org`





#### 2.3.1 NPM 安装服务器上的程序：

语法：`npm install 模块名`
例如：`npm install express`   
- 安装node中的web框架-express，
- 下载到了工程目录的node_modules 目录下，
- 因此，无需指定路径，
- 可以直接引用`var express = require('express');`



#### 2.3.2 全局安装  和  本地安装

- 全局安装 ：
	- `npm install express          # 本地安装`
	- 安装包放在 ./node_modules 下（运行 npm 命令时所在的目录）
	- 通过 require() 来引入
- 本地安装 ：
	- `npm install express -g   # 全局安装`
	- 安装包放在 /usr/local 下或者你 node 的安装目录
	-  可直接在命令行里使用





##### 1. 列出 全局安装 的所有模块

cmd中直接输入`npm list -g`【不要在REPL中输入】



##### 2. 列出 全局安装 的某个模块

cmd中直接输入`npm list 模块名`



#####  3. package.json

package.json 位于模块的目录下，用于定义包的属性



##### 4. 卸载、更新、搜索  - 模块

- `npm uninstall 模块名`
- `npm update 模块名`
- `npm search 模块名`












## 三、案例


### 3.1 基础

Node.js是 1个 让 js运行在服务器端的开发平台。
在以前，js代码需要借助 HTML 才能运行。
- 使用运行 js的指令【在命令行下】：`node ./abc.js`，然后在浏览器`127.0.0.1:3000`
- 每次对js文件进行**修改**，就要`ctrl + c`,然后重新`node ./abc.js`
- 通用模板：

```javascript
// 请求模块
const http = require("http");

//创建服务
const server = http.createServer(function(req,res){
    //设置字符集
    res.setHeader("Content-Type","text/html;charset=UTF8");

    //输出
    res.end("你好，世界~~!!");
})

//监听端口
server.listen(3000);
```


![3](https://z3.ax1x.com/2021/06/02/2QcSSI.png)





###  3.2 res.write( )  与 res.end( ) 的区别：

【都可输出html标签】
	- res.write( )： 普通的输出，在其后必须要有 res.end( )；不能输出非字符串
	- res.end( )：最后一次输出，只要遇到 res.end( )，即使后面有 res.write( )也不执行；不能输出非字符串









### 3.3 前导-实例

![4](https://z3.ax1x.com/2021/06/02/2Qcl0U.png)



![5](https://z3.ax1x.com/2021/06/02/2QcstH.png)









### 3.4 fs模块【异步读取本地文件】



![6](https://z3.ax1x.com/2021/06/02/2Qc4HS.png)









### 实例1：读取文件

![7](https://z3.ax1x.com/2021/06/02/2QcLj0.png)

```javascript
//导入模块
const http = require("http");
const fs = require("fs");

//创建 http服务
const server = http.createServer(function(req,res){

    //设置字符集
    res.setHeader("Content-Type","text/html;charset=UTF8")

    //读取本地文件
    fs.readFile("./public/1.html",function(err,data){
        //console.log(req.url);
        //如果地址栏的域名的后面的地址为 “/abc”,则输出读取的内容，否则输出提示
        if(req.url == "/abc"){
            res.end(data);

        }else{
            res.end("无此页面！！");

        }        
    })

})

server.listen(3000)
```








### 实例2：路由1

传统：路由即文件夹
node.js: 颠覆传统，通过路由读取文件，实现顶层路由设计
![8](https://z3.ax1x.com/2021/06/02/2QgCC9.png)









### 实例2：路由2-页面跳转

```javascript
const http = require("http")
const fs = require("fs")

const server = http.createServer(function(req,res){

    res.setHeader("Content-Type","text/html;charset=UTF8");

    //获取地址栏的地址
    let url = req.url

    //匹配地址：/user/字符串1/字符串2
    let arr = url.match(/\/user\/(.+)\/(.+)$/)
    console.log(arr);

   
    if(!arr){
        res.end("<h1>无页面显示！！</h1>");
        return;
    }else{

        //数据模拟
        let user = {
            'zy':"张毅",
            'wj':"吴京",
            'xs':'许嵩',
        }

        let list = {
            "post":"文章",
            "ask":'提问',
            "pic":"照片"
        }

        // arr[0]域名后的完整路径，arr[1]匹配的第一项，arr[1]匹配的第二项
        let item1 = arr[1];  
        let item2 = arr[2]; 
           
		//http://127.0.0.1:3000/user/wj/post
        res.write("<h1>欢迎,"+user[item1]+"</h1>");
        res.write("<h1>"+list[item2]+" 板块</h1>");    

    }

})

server.listen(3000)



```








### 实例3：路由-图片

![9](https://z3.ax1x.com/2021/06/02/2QgmUe.png)



```javascript
const http = require("http")
const fs = require("fs")

const server = http.createServer(function (req, res) {

    res.setHeader("Content-Type", "text/html;charset=UTF8");
    
    let url = req.url;
    
    if (url == "/user/jj") {
        fs.readFile("./public/1/jj.html", function (err, data) {
            res.end(data);
        })

    }else if(url == "/user/jj/jj.jpg"){
        res.setHeader("Content-Type", "image/jpg;");
        fs.readFile("./public/img/jj.jpg", function (err, data) {
            res.end(data)
        });
        
    }else{
        res.end("<h1>无此页面</h1>");
    }

})

server.listen(3000);
```








### 实例4：路由-页面+图片

![10](https://z3.ax1x.com/2021/06/02/2Qg8Df.png)



![11](https://z3.ax1x.com/2021/06/02/2QgtUg.png)





当需要请求的资源很多时，可使用 node 的 express 模块来静态化1个文件夹，使得文件夹内的文件无需单独设置路由就可直接加载。





![12](https://z3.ax1x.com/2021/06/02/2Qg6VU.png)











### 3.5 模块

1. 在 1个 HTML中引入多个 js 文件，则这些js文件共用1个宿主环境，公用作用域，所有的变量都属于 window对象
2. require( "模块名")
![13](https://z3.ax1x.com/2021/06/02/2Qgg54.png)
3. 在Node中，所有的js文件的作用域都是隔离的【因为node没有window对象】，
为了使js文件可以向其他文件暴露出部分内容，可以使用 exports

```javascript
//模块文件
var a = 123;
exports.a = a;

// app.js
let a = require("./1.1.5")
console.log(a);	// a就是导入的模块的对象，好处是有自己的命名空间

```









### 3.6 exports 和 module.exports 的区别：

- exports 可以暴露模块的多个参数
- module.exports 只暴露1个参数【通常为构造函数】

```javascript
//构造方法
function People(name,age,sex){
    this.name = name;
    this.age = age;
    this.sex = sex

}
//成员方法
People.prototype.sayHello = function(){
    console.log("我是"+this.name+",今年"+this.age+"岁了，性别为"+this.sex);
}

//暴露构造方法
module.exports  = People
```

```javascript
let People = require("./1.1.5");

let xiaoming  = new People("小明",20,"男")

xiaoming.sayHello();
```









### 3.7 导入模块



![14](https://z3.ax1x.com/2021/06/02/2QgXRA.png)




![15](https://z3.ax1x.com/2021/06/02/2Q2es0.png)





### 3.8 NPM

[npm社区](https://www.npmjs.com/)
`https://www.npmjs.com/`

在社区中搜索关键字，查找模块，根据文档安装和使用模块

```bash
npm install 模块名
```

