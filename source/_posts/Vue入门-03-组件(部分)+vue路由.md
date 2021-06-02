---
title: Vue 入门-03
tag: Vue
categories:
  - 前端
  - Vue
---

# Vue入门-03

[toc]

## 1.1 Vue cli 安装

1. 官网下载+安装 node.js：[node官网下载](http://nodejs.cn/download/)
2. 配置 node.js 的 镜像源：`npm install -g cnpm --registry=https://registry.npm.taobao.org`
3. 安装 vue:`npm install @vue/cli -g`
4. 使用cmd进入项目目录输入 vue ui 管理项目【不要关闭cmd】：`vue ui`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121093747655.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121093904458.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121094019976.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121094221635.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121094411293.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121094544103.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121094611966.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121095832299.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121095919324.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
最终的目录下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121100242313.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
展开 src目录：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121100537873.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121100556258.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121101042308.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121101228593.png)
## 1.2 Vue 组件
组件的其他笔记可参见  [vue笔记01](https://blog.csdn.net/m0_46578592/article/details/112737312)

 组件的两种方式：
1. 方式1【内部】：在script标签内
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021012111022074.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
2. 方式2【单独在外部写成1个文件】：`App.Vue`文件
App.vue最终会挂载到 HTML中
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121110724470.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121111817442.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
## 组件的使用步骤
- 定义组件
- App.vue 文件【相当于Vue对象】 中 手动引入 (`import`)自定义的组件文件【相当于 组件】
- 在App.vue模板中使用组件，编写事件
- 自动挂载到 Html
- 
### 1.2.1 创建组件并引入【外部组件】：
1.创建外部的组件
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121122353996.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
2. App.vue中导入刚才创建的组件：

【@符号表示：根目录，此例中为App.vue所在的src目录】
此例中，`demo1` 表示 `demo.vue整个文件`，`Demo`才是`真正的名字`
将 `demo1` 放到对象 `components` 对象内的过程就是`注册组件`【此时组件才可以使用】
【注册时，如果组件的 **调用名字** 和 **导出名字**  一模**一样**，则可直接省略`名字:名字`中的冒号及之后的名字来简写】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121124527572.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
#### 1.2.1.1 实例1 组件使用时 传值给自定义的属性：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121130542732.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021012113062640.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)

组件定义时的类型检查：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121133534888.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
自定义的属性 在定义时常用：
- type: 参见上图
- require: true/false
- default: 默认值
#### 1.2.1.2 实例2 【输入标题和内容，输出组件】：
组件定义：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121132616948.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)

#### 1.2.1.3 实例3 【给自创的组件 定义事件】：
在 组件定义 的时候 在 methods键 后的对象内定义1个函数，函数内绑定 自定义的`事件名`和`要返回的数据`，在`App.vue`文件中定义事件【该**事件作为参数** 传给 组件的事件（即：回调函数的语法）】，

### 1.2.2 组件的内容 : slot标签
在定义时，用 `<slot>  </slot>`作为 组件内容的占位符，当组件使用时，就会用 组件 包裹的部分来替换掉 slot 标签

## 1.3 Vue 路由
Vue路由用到了组件，下图中 组件 router-view 相当于是 1个小页面，当点击router-link时，router-view 中的内容就会根据组件的内容改变【相当于 frame】

[vue路由-官网](https://router.vuejs.org/zh/guide/#html)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210119131025839.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
组件调用：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121132924901.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)

### 1.3.1 Vue 路由-前置知识实例：
目标页面：【变化部分为 .container】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121155547745.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
#### 1.3.1.1 组件的 css 样式
3种：
- 默认【样式重名时，样式会污染出去】：`<style>       </style>`
- scoped【样式只在定义的文件中有效】: `<style scoped>      </style>`

#### 1.3.1.2 left组件
router-view 跟着 router-link 变化
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121164057933.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121164906518.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
上图中，
path就是地址栏的变化，
`/about` 最后导入的组件显示在 router-view 中


实例1：【 **left中 的点击 router-link**  使    **container 中的 router-view 变化**】

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121170344391.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121170532683.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121170714448.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
### 基本路由的大致步骤小结
【在默认的情况下，点击链接所在的组件，让router-view变化】：
- 定义组件
- 在App.vue中定义router-view
- 在某个组件中定义router-link，且router-link标签的 to 属性绑定 地址
- 在 views 目录下 定义组件，由于点击链接后显示
- 在 router/index 中 绑定 地址栏的变化 与 导入组件的关系【注册路由】

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121172438231.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
### 1.3.2 动态路由【地址栏传参数】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121175810568.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121180036456.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
HTML中如果需要获取地址栏的参数：` {%raw%}{{ $route.params.参数名}} {%endraw%}`
JS 中使用地址栏的参数：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121191630300.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)


实例1：
- 目标：点击 left中的“博客” =》 右侧出现 博客的title,点击title,跳转到显示pid的界面【pid为地址栏中的博客id】
- 主要 需修改 的文件：
	-  left.vue组件
	-  路由【包括：左侧导航栏的路由，博文列表中每1项的路由】
	-  Articels.vue组件: 存放博文列表
	- article.vue：博文内容

left.vue组件：点击后显示博客的标题列表
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121190127215.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121190229322.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121190401380.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121190544132.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121191840694.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
1. 在 函数中使用 地址栏中的参数-实例：
`this.$route.params.地址栏参数名`
【假设 data 是 ajax 获取的数据集，根据地址栏的参数id 来匹配 data中的每一项，将不符合条件的过滤掉，最后取 过滤后的数组的第1项 来显示】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121192834676.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
### 子组件向父组件传值：
- 子组件 先 绑定**方法**
- 子组件的方法中 再 绑定**事件**：`$emit("小写的事件名"，this.$data)`
- 父组件 的方法的参数名为data, 
- 父组件使用子组件时，将父组件中的**方法** => **传入** 子组件的**事件**中

### 组件嵌套的路由
要使用路由一般要有以下条件：
- 路由注册【path，要导入的组件】:最重要
- route-view
- route-link
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121200043973.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
### 1.4 命名路由
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121202032986.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
在 js 的方法中中可使用【name需要params；path需要拼接】：
-  `this.$router.push(地址)`  
-  `this.$router.push({path:"地址",query:{参数名：参数值，参数名：参数值}})`
-  `this.$router.push({name:"路由名"})`
- `this.$router.push({name:"路由名",params:{参数名：参数值，参数名：参数值}})`

###  1.5 去掉使用路由时，地址栏上的#号
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021012120374382.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210121204319530.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)

