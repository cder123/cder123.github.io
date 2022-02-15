

<h1>Vue2 + Vue3-笔记</h1>

[toc]



# 零、资料



> 视频：
>
> - [尚硅谷 Vue2.0 + Vue 3.0-bilibili](https://www.bilibili.com/video/BV1Zy4y1K7SH?p=1)
>
> 
>
> 文档：
>
> - [Vue 3-中文官网](https://v3.cn.vuejs.org/)
>
> -  [Vue 2-中文文档](https://vuejs.bootcss.com/guide/)
> - [Vue 3-中文文档](https://v3.cn.vuejs.org/guide/introduction.html)
>
> 
>
> 博客：
>
> - [Vue 2-博客](https://blog.csdn.net/weixin_44972008/category_10622253.html)
> - [Vue 3-博客](https://juejin.cn/post/7005140118960865317/)



---





# 一、Vue 简介

<font style="color:red;font-size:1.2em;">（1）Vue 是谁开发的？：</font>尤雨溪。



<font style="color:red;font-size:1.2em;">（2）Vue 的特点：</font>

> 1. 遵循 MVVM 模式
> 2. 编码简洁，体积小，运行效率高，适合 移动/PC 端开发
> 3. 它本身只关注 UI，可以轻松引入 vue 插件或其它第三方库开发项目
> 4. 采用**组件化**模式，提高代码复用率、且让代码更好维护
> 5. **声明式**编码，让编码人员无需直接操作DOM，提高开发效率
> 6. 使用**虚拟DOM**和**Diff算法**，尽量复用DOM节点



<font style="color:red;font-size:1.2em;">（3）Vue 与 其他前端框架的关联：</font>

> - 借鉴 `angular `的 **模板** 和 **数据绑定** 技术
> - 借鉴 `react `的 **组件化** 和 **虚拟DOM** 技术



<font style="color:red;font-size:1.2em;">（4）Vue 扩展插件：</font>

> - `vue-cli`：vue 脚手架
> - `vue-resource(axios)`：ajax 请求
> - `vue-router`：路由
> - `vuex`：状态管理（它是 vue 的插件但是没有用 vue-xxx 的命名规则）
> - `vue-lazyload`：图片懒加载
> - `vue-scroller`：页面滑动相关
> - `mint-ui`：基于 vue 的 UI 组件库（移动端）
> - `element-ui`：基于 vue 的 UI 组件库（PC 端）





---





## 1、安装 Vue 的两种方式



### 1.1、 `<sctipt>` 标签



<font style="color:red;font-size:1.2em;">步骤：</font>

> - 在`<script src="cdn地址"></script>`中，引入`Vue`的 CDN地址。
> - 在`HTML`中，定义一个`div`，用于作为 Vue 数据显示的`根结点`。
> - 在`JS`中，使用`Vue.config.productionTip = false;`来关闭控制台的产品提示。
> - 创建一个Vue对象，绑定`根节点`，编写数据。



```html
<!-- 引入Vue -->	
	<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>

<!-- Vue的显示区域 -->	
	<div id="container">
        <!-- 
			{{ }} 表示Vue单向传值，可以写js表达式（自动计算表达式的值），
			也可以写Vue对象data属性中的数据			
		-->	
        {{ txt }}           <br/>
        2 + 3 = {{ 2+3 }}   <br/>
    </div>

<!-- Vue的代码 -->	
	<script>
        // 取消开发环境下，浏览器控制台的产品提示信息
        Vue.config.productionTip = false;
        
        // 创建一个Vue的代理对象
        var vm = new Vue({
            el:'#container',	// el属性，绑定Vue的显示区域的根标签，只能有一个
            data:{				// data属性，绑定需要显示在页面上的数据
                txt:'hello world'
            }
        });       

    </script>
```





---





### 1.2、 `vue-cli` 脚手架

























---





## 2、创建 Vue 对象的注意事项



<font style="color:red;font-size:1.2em;">创建 Vue 对象的注意事项：</font>

> 1. 想让 Vue 工作，就必须创建一个 `Vue 实例`，且要传入一个`配置对象`；
> 2. `root 容器`里的代码依然符合 `HTML `规范，只不过混入了一些特殊的 Vue 语法；
> 3. `root 容器`里的代码被称为【Vue模板】；
> 4. `Vue 实例`和容器是一一对应的；
> 5. 真实开发中**只有一个 Vue 实例**，并且会配合着组件一起使用；
> 6. `{{ xxx }} `中的 `xxx `要写` js 表达式`，且 `xxx `可以自动读取到 `data `中的所有属性；
> 7. 一旦 `data `中的数据发生改变，那么页面中用到该数据的地方也会自动更新；



<font style="color:red;font-size:1.2em;">注意区分：js表达式 和 js代码(语句)</font>

1. 表达式：一个表达式会产生一个值，可以放在任何一个需要值的地方：
	(1). `a`
	(2). `a+b`
	(3). `demo(1)`
	(4). `x === y ? 'a' : 'b'`
2. js代码(语句)
	(1). `if(){}`
	(2). `for(){}`



<font style="color:red;font-size:1.2em;">挂载根结点（2种方式）：</font>

1. `el` 属性：

```js
// js
const vm = new Vue({
	el:'#container', //第一种写法
	data:{
		txt:'Hello world'
	}
})
```

2. `$mount(挂载点的css选择器)` 方法：

```js
// js
const vm = new Vue({
	data:{
		txt:'Hello world'
	}
})
vm.$mount('#container') //第二种写法 */
```



<font style="color:red;font-size:1.2em;">data（2种方式）：</font>

(1)对象式：

```js
// js
const vm = new Vue({
	data:{
		txt:'Hello world'
	}
})
```



(2)函数式：【推荐】

```js
// js
const vm = new Vue({
    
	data(){        
		return {
            txt:'Hello world'
        }
	}
})
```



<font style="color:red;font-size:1.3em;">！！由Vue管理的函数，不能写箭头函数，一旦写了箭头函数，this就不再是Vue实例。</font>





---





## 3、模板语法



Vue 的模板语法有两大类：

> 1、插值语法
>
> - 功能：解析标签体中动态绑定的内容。
> - 写法：`{{ xxx }}`，其中的`xxx`表示`js表达式`，且`xxx`可以直接读取Vue对象的`data`标签的内容。
>
> 
>
> 2、指令语法：
>
> - 功能：用于解析标签（包括：标签属性、标签内容、绑定事件等）
> - 举例：`:href="xxx" `，其中的`xxx`表示`js表达式`，且`xxx`可以直接读取Vue对象的`data`标签的内容。
> - 写法：`v-xxxx`，其中的`xxxx`表示指令的名字，具体见文档。







### 3.1、插值语法



在如下代码中，`#container`中的内容就是模板，该案例中，模板使用的**双大括号**叫`插值语法`。

```html
<!-- 引入Vue -->	
	<script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>

<!-- Vue的显示区域 -->	
	<div id="container">      
        {{ txt }} 			// 插值语法-纯文本      
        2 + 3 = {{ 2+3 }}	// 插值语法-表达式
        {{ rawHtml }}		// 插值语法-原始的HTML
    </div>

<!-- Vue的代码 -->	
	<script>        
        var vm = new Vue({
            el:'#container',	
            data:{				
                txt:'hello world',
                rawHtml:'<span style="color:red;">这是原始 html </span>'
            }
        }); 
        Vue.config.productionTip = false; 
    </script>
```





### 3.2、指令语法



Vue 中，凡是以`v-`开头的都是指令，指令一般需要绑定一个参数（标签的属性）。

**例如**：

> 指令-数据绑定：
>
> - `v-bind`：**单向**绑定属性，
> -  `v-model`：**双向**绑定表单控件
> -  `v-on`：**单向**绑定事件
>
>  
>
> 指令-数据显示（当值为`true`时）：
>
> - `v-if` + `v-else-if` + `v-else`：不渲染，开销大。
> - `v-show`：渲染，但使用css的`display:none`来隐藏，开销小。
>
>  
>
> 指令-循环：
>
> - `v-for`：循环复制并渲染所修饰的标签。



`v-on`指令的修饰符：

- `.stop`
- `.prevent`
- `.capture`
- `.self`
- `.once`
- `.passive`

```html
<!-- 阻止单击事件继续冒泡 -->
<a @click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form @submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a @click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form @submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即内部元素触发的事件先在此处理，然后才交由内部元素进行处理 -->
<div @click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div @click.self="doThat">...</div>

<!-- 点击事件将只会触发一次 -->
<a @click.once="doThis"></a>
```



在 [`v-for`](https://v3.cn.vuejs.org/guide/forms.html#修饰符) 中，你会看到其它修饰符的例子。

> 使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，用 `@click.prevent.self` 会阻止**元素本身及其子元素的点击的默认行为**，而 `@click.self.prevent` 只会阻止对元素自身的点击的默认行为。



#### 3.2.1、<font style="color:red;font-size:1.2em;">v-bind 指令：</font>

`v-bind`指令可以直接简写为`:`，该指令用于绑定HTML标签的属性，使得 HTML标签 能够被 Vue更改 。

在HTML标签的属性被`v-bind`指令绑定后，该属性的内容就变为了 js 表达式。

`v-bind`指令-演示：

```html
	<div id="container">      
        <a v-bind:href="url" target="_blank">百度-使用v-bind指令</a>   <br/>
        <a :href="url" target="_blank">百度-使用v-bind指令的简化版‘:’</a>   <br/>
    </div>

    <script>
        var vm = new Vue({
            el:'#container',
            data(){
                return {                    
                    url:"https://www.baidu.com"
                }
            }
        });
        Vue.config.productionTip = false;
    </script>
```





#### 3.2.2、`v-model`指令：

`v-model`指令，没有简写，只能绑定在**表单标签**上，被该指令修饰的HTML标签的属性的属性值应该改为 Vue 对象`data`属性中的键。

```html
    <div id="container">
        <!-- v-model  -->
        <form action="#">
            <input type="text" name="username" v-model:value="username" 
                   placeholder="请输入用户名">
            <span style="color:red">{{ username }}</span>
        </form>
    </div>


    <script>
        var vm = new Vue({
            el:'#container',
            data(){
                return{                  
                    username:''
                }                
            }
        });
        Vue.config.productionTip = false;
    </script>
```









#### 3.2.3、`v-on`指令：

`v-on`指令，简写形式为：`@`，该指令用于绑定Vue对象中`methods`属性中的JS事件。



```html
	<div id="container">
        <button @click="sayHello"> sayHello </button>
    </div>

    <script>
        var vm = new Vue({
            el:'#container',
            data(){
                return{                   
                }                
            },
            methods: {
                sayHello:function(){
                    alert("你好！")
                },
            },
        });
        Vue.config.productionTip = false;

    </script>

```





#### 3.2.4、`v-show`指令：

`v-show`指令，使用CSS的`display:none`来隐藏标签，相比`v-if`开销小。

```html
	<div id="container">     
       
        <div v-show="true">
            显示
        </div>
        <div v-show="false">
            隐藏
        </div>

    </div>

    <script>
        var vm = new Vue({
            el:'#container',
            data(){
                return{ 
                }                
            },
            methods: {
                sayHello:function(){
                    alert("你好！")
                },
            },
        });
        Vue.config.productionTip = false;

    </script>
```





#### 3.2.5、`v-if`指令：

`v-if`指令，作用类似`v-show`，但不同的是，`v-if`指令的值为false时，不渲染数据，频繁切换时开销大。



```html
	<div id="container">
        
        <div v-if="isShow">
            显示、隐藏-1
        </div>
        <div v-else="!isShow">
            显示、隐藏-2
        </div>

    </div>

    <script>
        var vm = new Vue({
            el:'#container',
            data(){
                return{                   
                    isShow:true
                }                
            },          
        });
        Vue.config.productionTip = false;
       
    </script>
```



#### 3.2.5、`v-for`指令：

`v-for`指令，用于循环显示数据。

```ht
	<div id="container">
    
        <ul>
            <li v-for="(item,index) in books" :key="index">
                {{ index }} => {{ item }}
            </li>
        </ul>
                    <!-- 结果：
                        0 => 西游记
                        1 => 红楼梦
                        2 => 三国演义
                    -->

    </div>

    <script>
        var vm = new Vue({
            el:'#container',
            data(){
                return{                   
                   books:["西游记","红楼梦","三国演义"]
                }                
            },         
        });
        Vue.config.productionTip = false;
       
    </script>
```





## 4、理解 MVVM 模型

