

<h1>SSM整合Vue</h1>

[toc]





# 参考资料



- [Vue-整合SSM](https://www.bilibili.com/video/BV1A44y1p7x5?p=26)







# 一、前端



## 1、环境搭建



> - node
> - webpack
> - vue-cli
> - axios
> - element-ui
> - Vue的插件（router、vuex）



### 1.1、NodeJS 的安装与配置



第一步 安装：[NodeJS-安装教程](https://www.runoob.com/nodejs/nodejs-install-setup.html)

第二步 配置淘宝镜像： `npm install -g cnpm --registry=https://registry.npmmirror.com`





### 1.2、Webpack的安装



安装：`cnpm install -g webpack`





### 1.3、VueCli 的安装

`cnpm install -g @vue/cli`





### 1.4、Vue-router 的安装

【在项目文件夹内】`cnpm install --save vue-router`

路由前后变化的部分显示在`router-view`标签中。



```js
// router/router.js
    import { createRouter,createWebHistory } from "vue-router";
    // import HelloWorld from '../components/HelloWorld.vue';
    const router = createRouter({
        history:createWebHistory(),
        routes:[
            // {
            //     path:'/',
            //     component:',
            // }
        ]
    })
    export default router;



// main.js
	import { createApp } from 'vue'
    import App from './App.vue'
    import ElementPlus from 'element-plus';
    import 'element-plus/dist/index.css';
    import router from './router'
    const app = createApp(App)
    app.use(ElementPlus)
    app.use(router)
    app.mount('#app')

```







### 1.5、Vue-vuex 的安装

【在项目文件夹内】`cnpm install --save vuex@next `

```js
// 在 main.js
    import Vuex from 'vuex'

    new Vue({	
      Vuex, // 注册
      render: h => h(App),
    }).$mount('#app')


//=============================
// 单个文件
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store  = new Vuex.Store({
    state:{

    }
})
export default store;



//=
// vue3中创建store实例对象的方法createStore()按需引入
import { createStore } from 'vuex'

export default createStore({
  state: {
  },
  mutations: {
  },
  actions: {
  },
  getters: {
  },
  modules: {
  }
})
```





### 1.6、axios 的安装



【在项目文件夹内】`cnpm install --save axios `

```js
// 在 main.js
import axios from 'axios'


const Vue = createApp(App)
    .use(store)
    .use(router)
    .use(ElementPlus)

Vue.config.globalProperties.axios = axios

Vue.mount('#app')
```





### 1.7、Element-Plus 的安装

【在项目文件夹内】安装：`cnpm install --save element-plus`

配置：

```js
// 在 main.js 下

import { createApp } from 'vue'
import App from './App.vue'
import ElementPlus from 'element-plus';
import 'element-plus/dist/index.css';
const app = createApp(App)
app.use(ElementPlus)
app.mount('#app')
```



# 二、后端





## 1、IDEA-Maven-打包



<img src="https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220305141445747.png"/>

