---
title: SpringCloud-笔记
tag: Java
categories:
  - [后端,Java,SpringCloud]

---

<h1>SpringCloud-笔记</h1>

[toc]



# 零、资料



> - [SpringCloud-黑马](https://www.bilibili.com/video/BV1LQ4y127n4?p=5)





# 一、微服务概述





## 1、微服务技术栈





![image-20220407085215509](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220407085215509.png)

<center style="color:red">微服务技术栈</center>





微服务是为了<font style="color:red;">拆分服务</font>，形成服务集群。

![image-20220407085629748](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220407085629748.png)

<center style="color:red">服务集群</center>



由于服务过多，调用关系过于复杂，因此，需要有一个<font style="color:red;">注册中心</font>，来记录服务集群的调用关系，每次调用时，只需要到注册中心去找需要调用的服务的ip就行了。

![image-20220407085830021](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220407085830021.png)

<center style="color:red">注册中心</center>



因为服务过多，造成配置过多，难以维护，所以需要<font style="color:red;">配置中心</font>来记录每个服务的配置。

![image-20220407090105359](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220407090105359.png)

<center style="color:red">配置中心</center>



为了让用户访问微服务，所以需要<font style="color:red;">服务网关</font>。

![image-20220407090523824](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220407090523824.png)

<center style="color:red">服务网关</center>



为了加快访问，所以需要有<font style="color:red;">分布式缓存</font>。

![image-20220407090602824](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220407090602824.png)

<center style="color:red">分布式缓存和数据库集群</center>



为方便查询，需要<font style="color:red;">分布式搜索</font>

![image-20220407090705095](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220407090705095.png)

<center style="color:red">分布式搜索</center>



为加快速度，需要有异步通信的<font style="color:red;">消息队列</font>

![image-20220407090958828](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220407090958828.png)

<center style="color:red">消息队列</center>





## 2、微服务架构的演变过程





### 2.1、单体架构



单体架构，将所有的业务功能放在一个项目中开发，打成一个包部署。

优点：架构简单，部署成本低。

缺点：高耦合



![image-20220407091511932](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220407091511932.png)

<center style="color:red">单体架构</center>



### 2.2、分布式架构



根据业务功能，对系统进行拆分，每个业务模块作为一个独立项目，每个业务模块成为一个<font style="color:red;">服务</font>。



优点：低耦合，易扩展。

缺点：带来了服务治理的问题。



分布式架构需要考虑的问题（服务治理）：

> - 服务拆分的粒度如何？
> - 服务集群的地址如何维护？
> - 服务与服务之间如何实现远程调用？
> - 服务健康状态如何感知？





![image-20220407091836479](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220407091836479.png)

<center style="color:red">分布式架构</center>



### 2.3、微服务架构



在总结了**分布式架构**的经验后，提出了<font style="color:red;">微服务架构</font>，它是一种经过**良好设计的分布式架构**。



微服务架构的特征：

> - 单一职责：微服务的拆分粒度更小，一个业务就是一个服务，做到单一职责，避免重复开发。
> - 面向服务：对外暴露服务的业务接口。
> - 自治：团队独立、技术独立、数据独立、部署独立。
> - 隔离性强：服务做好隔离、容错、降级，避免出现级联问题。



![image-20220407093125245](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220407093125245.png)

<center style="color:red">微服务架构</center>





### 2.4、小结

![image-20220407093229045](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220407093229045.png)



## 3、微服务的技术对比



国内较为出名的：SpringCloud 和 Dobbo



SpringCloudAlibaba 兼容 前两种。

![image-20220407094036929](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220407094036929.png)





![image-20220407094101325](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220407094101325.png)