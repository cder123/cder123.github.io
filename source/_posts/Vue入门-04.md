---
title: Vue 入门-04
tag: Vue
categories:
  - 前端
  - Vue
---

# Vue入门-04

[toc]

## 1.1 Vuex

使用 Vuex 可以在整个Vue项目中共享数据【类似 数据库的作用】
=》官方叫`状态管理模式` 
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210122104027301.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
项目中要使用 vuex 必须要 `use` 来应用【vue 的 router 也一样】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210122104249153.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
 vuex 的路径：`/src/store/index.js`
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210122104820568.png)
首先，在`/store/index.js`文件中引入vuex，并导出1个store对象，
在`main.js`文件中导入 `/store/index.js`文件，并在Vue对象中注册
![在这里插入图片描述](https://img-blog.csdnimg.cn/202101221056369.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210122105030964.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210122110141606.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
### 实例1【获取数据】：
- store中的state里面写数据
- store中的getters方法
- 在要使用数据的组件中导入vuex的 mapGetters
- 在计算属性computed中 使用`...mapGetters(["getter方法名1"，“getter方法名2”])`
- 利用 {{ 计算属性名}} 来使用数据
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210122113209775.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021012211431555.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
### 实例2【获取数据】：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210122114921444.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210122115028541.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
### 实例3 【设置数据】

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210122122416420.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210122122915950.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)

