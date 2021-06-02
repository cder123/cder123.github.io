---
title: ARP协议
tag: CCNA
categories:
  - 网络工程
  - CCNA 
---



## 1. ARP请求与ARP代理

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201214192045886.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
- R1 上配 用`接口`的静态路由到R3，【最后一定可以知道目的设备的MAC,因为只知道从接口发，而不知道发给谁，只有得到目标设备的MAC地址，才能完成封装】
- R3 上配 用`IP地址`的静态路由到R1 ,【最后不会知道目标的MAC，因为静态路由配的是地址，R3明确知道要发给谁，可以直接封装】

**R1  = ping => R3** 过程
1. R1到R3需要跨网段，因此，需要涉及代理ARP
2. R1根据R3的IP地址发送ARP广播【广播包中有源IP，源MAC，目的IP】。网关R2收到后，解封装【其他设备收到后发现不是发给自己的，就丢弃】，再以自己的名义封装ARP请求并发给R3。R3收到请求后，返回1个ARP响应给R2,R2再发给R1


代理ARP：以自己的名义将别人的包转发出去。





## 2. 双出口

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201214195453914.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
- 上图是网络双出口，R1到外网从R2走
	- 原因：
> R1 ping 外网时，发送ARP广播，R2和R3都收到ARP广播，并发送了响应
> 但是，由于R3的响应报文先发，R2的报文后发，后发的报文会覆盖先发的报文，因此，R1到外网从R2出去

**华为、华三设备尤其要注意接口代理ARP是否打开。**