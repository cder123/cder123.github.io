---
title: Vue 入门-02
tag: Vue
categories:
  - 前端
  - Vue
---

# Vue入门-02

[toc]

## 1.1 计算属性【用于 数据处理】

**作用**：为了不让{%raw%} {{ }} {%endraw%}内的东西太长，使其不好维护，可先使用`计算属性`处理，处理完后的结果再在 {%raw%}{{ }} {%endraw%}中使用，【保证在 {%raw%}{{ }} {%endraw%}只写简单的逻辑】  

  

**计算属性** 会很 **快**，因为使用了 **缓存**，【只计算1次，之后刷新页面时，直接从缓存调用 `计算属性`。除非属性值被修改了，才会再一次进行 computed】   


**格式：**【定义在 conputed 中；定义时 格式类似函数，有小括号，有返回值；调用时 无需小括号】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210118161230581.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
## 1.2 CSS 样式的绑定
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210118171547218.png)

实例1：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210118165226196.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210118165612375.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)

实例2【重要】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210118171331166.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
实例3【指定默认样式，最终样式为：default + odd/even】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210118171831444.png)
实例4【绑定style属性】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210118172421806.png)

## 1.3 数组的变更方法
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210118173323306.png)
以上这些方法可以在**Vue对象内部**  `变更数组`并刷新界面【对原数组进行改变】，
在Vue对象内部，可使用 `Vue.set(要被设置的对象，”属性名“，属性值)`， 
在**Vue对象外面**，还可以使用 `vm.数组名.length=0`来变更数组，
除以上提及的方式外，其他方式均无法在Vue内部更改数组。【改了也不更新页面】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210118180023113.png)
以上方法用 新数组  替换 旧数组

实例：【模糊查询】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210118183909419.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
实例：计划表
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210118191645297.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210118191317308.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210118191444702.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210118191621270.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)

