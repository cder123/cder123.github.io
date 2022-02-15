---
title: Vue 入门-01
tag: Vue
categories:
  - 前端
  - Vue

---



# Vue入门-01

[toc]
## 1. Vue 的介绍



`https://cn.vuejs.org/v2/guide/`
[Vue官网教程](https://cn.vuejs.org/v2/guide/)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210117122819956.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)

### 1.1 安装
#### 1.1.1 使用CDN【不用安装，但需要联网】
开发环境：

```markup
<!-- 开发环境版本，包含了有帮助的命令行警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```
生产环境：

```markup
<!-- 生产环境版本，优化了尺寸和速度 -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```
#### 1.1.2 使用npm包管理器安装【不推荐新手】

。。。




### 1.2 Vue必备步骤
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210117124822974.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
- Vue 以数据驱动的【即：根据数据来改变】
- Vue 的指令：
	- 与 `标签属性相关`：使用 `:`   =》 `v-bind:src="key名"`
	- 与 `标签属性无关`：使用 `=`  =》  `v-if="false"`


### 1.3 Vue 绑定数据
- 单向绑定【适用于 普通的标签】:  `Vue =渲染到=》HTML`
- 双向绑定【适用于 表单标签】： `Html的内容变，则Vue中也变；Vue内变，则Html中也变`

单项绑定-实例：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210117125749164.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
双项绑定-实例：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210117130609985.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
### 1.4 Vue 绑定属性
`v-bind:属性名=“Vue中的变量”`
例如：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210117131910842.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)


### 1.5 Vue 显示+隐藏
不显示内容：【将条件置为false】
- 使用 `v-if`： 完全不渲染【连标签都没有】
- 使用 `v-show`：渲染，但利用 CSS	中的 `display:none;` 来隐藏

`v-if="布尔值"`实例：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021011713293566.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)


`v-show=“布尔值”`实例：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210117132536937.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
### 1.6 Vue 循环
`v-for=“变量名 in 可遍历的对象”`

`v-for`指令实例：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210117133639478.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
### 1.7 Vue 绑定事件
` v-on:click`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210117134420764.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
### 1.8 实例-1 ：输入文本，查看预览效果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210117140218529.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
### 1.9 实例-2 ：勾选复选框-显示图片
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210117141728671.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)

### 1.10 实例-3 ：博客发布-简易版
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210117144208281.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210117144242331.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
## 2.1 组件(自定义组件)
定义组件名时：使用 驼峰法 或 - 法
使用组件时，用 - 法
组件的属性在定义时要有双引号<kbd> "  "</kbd>
组件的data的**冒号后**只能跟函数【函数的返回值可以是对象】，这点要与Vue对象区分开，且组件不能有el 键【这也与Vue对象不同】
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021011715054395.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)

### 2.2.1 自定义组件-实例：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210117151523122.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
## 3.1 常用指令



> v-show： 显示、隐藏=》display:none;
> v-if：显示、隐藏=》不渲染
> v-else
> v-else-if

> v-text：插入vue的data中的值，相当于{%raw%}{{ }}{%endraw%}
> v-html ：可以插入html代码

> v-bind：绑定属性
> v-model：双向绑定数据
> v-on：{% raw %}绑定事件，结合vue中的 `methods：{ 事件名：function(){ } } `来使用{% endraw %}

> v-for：循环



### 3.1.1 v-text 和 {%raw %} {{ }} {% endraw %}



- `v-text`  : 等价于{%raw %} {{ }}  {% endraw %}，当输出的内容为html代码时，{% raw %}{{}}{% endraw %}原样输出，
而`v-html`会根据输出内容渲染
例如：
【以下两条指令等价，但 {%raw %} {{}}  {%endraw %} 在js加载缓慢时可能为显示出来】

```markup
  <p>{% raw %} {{ msg }} {% endraw %}</p>
  
  <p v-text="msg"></p>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210117153705654.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)

 {%raw %} 为避免{{ }} 插值时 {%endraw %} ，因为加载速度慢而渲染出 大括号，可使用`v-cloak`来避免出现大括号
【使用 `v-cloak`的前提是 在`<style> [v-cloak]{display:none;}</style>`,然后在有{%raw %} {{ }}  {%endraw%}的标签中`v-cloak`】  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210117172433827.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)



### 3.1.2 v-for
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210117161917689.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)

### 3.1.3 v-on

`v-on`  等价于  `@`

例如【以下二者等价】：

```markup
<button v-on:click="alertMsg">点击1</button>
           
<button @click="alertMsg">点击2</button>
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021011716304391.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
`冒泡事件`：内层元素触发事件后，外层元素也会触发事件
例如：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210117165840328.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)

- 冒泡事件：`@click="helloMsg"` 
- 停止冒泡事件`事件.stop`：`v-on:click.stop="helloMsg"` 即：`@click.stop="helloMsg"` 
- 鼠标事件【只有mousedown 事件才触发】：
	- `@mousedown.left="helloMsg"`  ：鼠标左键
	-  `@mousedown.right="helloMsg"`：鼠标右键
	- `@mousedown.middle="helloMsg"`：鼠标滚轮 
- 键盘事件【键盘按下某个键时才触发，键盘码 / ctrl / shift 等】：
- `@keydown.enter="hellMsg"`：键盘按下回车

### 3.1.4 v-bind

`v-bind:` 绑定属性：  等价于 直接 `：`
例如【以下二者等价】：

```markup
    <img v-bind:src="imgSrc" alt="">
    <img :src="imgSrc" alt="">
```

### 3.1.6 实例：tab切换【v-show 或 v-if】

快捷生成标签：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210117155449125.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210117155505200.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210117160113765.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
或
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210117160332375.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)



## 4. 注意事项
### 4.1 取用Vue对象中的值

 取用 Vue对象中的内容【设 vm为 Vue对象的名字】： `vm.$变量名`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210117173258973.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)



### 4.2 `.$watch`:监听 指定属性 的属性值的变化



第一种方式【Vue对象外】：
`vm.$watch("属性名",function(新值，旧值){ }  )`

第二种方式【Vue对象内】：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210118163855925.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)




### 4.3 Vue的生命周期
#### 4.3.1 beforeCreate事件

beforeCreate事件在new 完 Vue对象后马上触发，是第一个触发的事件【此时还没有事件的监听，即：`初始化之后，数据监听前（还没有数据）`】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210117182607184.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)



#### 4.3.2 created事件【重要：绑定数据】
在 beforeCreate事件之后，dom元素之前【此时还没有事件的监听，即：`初始化之后，数据监听后（有数据了）`】，ajax赋值要在这个阶段之后

#### 4.3.3 beforeMount 事件



 beforeMount 事件：在dom元素产生之后，渲染之前触发，【即：{% raw %}有{{ }}且还没有处理{{ }}的阶段{% endraw %}】



#### 4.3.4 mounted 事件【重要：绑定dom元素】
mounted 事件：在dom元素渲染之后触发，【此时，已经可以识别元素】

#### 4.3.5 beforeUpdate 事件

beforeUpdate 事件：在监听阶段（即：vm.$watch(属性名，function(){})），如果属性发生变化，则在渲染之前触发该事件

#### 4.3.6 updated 事件【重要：修改属性之后】
updated 事件：在监听阶段，如果属性发生变化，则在渲染之后触发该事件
#### 4.3.7 beforeDestroy 事件
beforeDestroy 事件:销毁之前触发

#### 4.3.8 Destroyed 事件
beforeDestroy 事件:销毁之后触发

