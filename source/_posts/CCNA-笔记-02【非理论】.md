---
title: CCNA-笔记-02【非理论】
tag: CCNA
categories:
  - 网络工程
  - CCNA 
---

# CCNA-笔记-02【非理论】

[toc]

## 第二章 TCP / IP协议
### 1. 协议分层
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201211110629118.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
#### 1.1 传输层
- TCP：可靠，建立会话【消耗资源】，差错校验，分段传输，适用于：1个数据包传不完，需要分段传输，需要重传的场合，如：浏览网页、发邮件
`netstat -n`查看会话
- UDP：不可靠，适用于：1个数据包就能传完。如：屏幕广播，QQ聊天，域名解析
#### 1.2 应用层
- HTTP    = tcp + 80
- HTTPS = tcp +443
- FTP      = tcp + 20 / 21  【20传数据(主动模式时20，被动模式由软件协商)，21做控制】
- SMTP = tcp + 25
- POP3 = tcp +110
- RDP = tcp + 3389
- DNS = TCP【同步数据】/ UDP【查dns，常用】 + 53
- CIFS【使用IP地址访问】 = tcp + 445
- CIFS【使用计算机名访问】 = tcp + 139 
- SQL Server【远程访问时】 = tcp + 1433
- Telnet  = tcp + 23
- SSH = tcp + 22
> `服务与端口的关系`：
> 用 端口 来区分使用的 服务
> - 【服务 侦听 端口】
> - 【客户端请求服务】
>  【客户端请求的服务使用目标端口区分】
>   【服务停止，该侦听的端口就关闭】
>   
> 1个服务 占用 1个端口【不能重复】
> `netstat -a` 查看服务和端口情况【外部地址以主机名的形式显示】
> `netstat -an` 使用数字形式显示端口占用情况【外部地址以数字的形式显示】
> `netstat -anb ` 查看进程与端口的对应关系
> `netstat -an | find "80"`:查看80端口的使用情况

不允许他人更改IP地址：
- 禁用 Network Connections 服务。

不允许共享资源：
- 禁用 Workstation 服务。

##### 1.2.1 实例：远程桌面：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201211114532201.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201211114608755.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201211114641857.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201211114714737.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
一路“确定”
【使用RDP，注意`防火墙`是否允许】
##### 1.2.2 实例： 安装服务
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201211120937700.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201211120950740.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201211121049884.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
#### 1.3 端口安全
测试端口 开没开【现在weindow默认关闭telnet】：`telnet   ip地址   端口`

端口安全：
- TCP/IP筛选：[百度经验-TCP/IP筛选](https://jingyan.baidu.com/article/90bc8fc84f568ff653640cca.html)
服务器【win 2003 server】：右键网卡-》找到Tcp/IP筛选、防火墙

##### 1.3.1 实例：打开防火墙
**场景：**自己的计算机 只开启80端口，其他人只能访问我这台计算机的80端口，而我可以访问别人的其他端口
【即：入站和出站是不同的规则，我出去用的是临时打开的随机端口，会话结束就关闭这个临时端口】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201211124446917.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020121112452153.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
Ping 不通：可能是`防火墙`的问题
排除木马【可疑程序】`msconfig`查看计算机中的已安装的未知程序

#### 1.4 IPSec:防木马，严格控制出入流量
防火墙和TCP/IP筛选：不控制出去的流量

server: 使用 IPSec + TCP/IP筛选
Client:  使用防火墙【windows 防火墙依赖 `服务`】

场景：web服务器：只允许80端口进出
- 打开本地安全策略
- 新建 策略
- 拒绝 所有 - 应用 - 确定
- 添加 - 从“80端口”
- 应用-确定
- 启用策略
策略：采用`最佳匹配`原则
#### 1.5 网络层协议
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201211133044459.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201211133104828.png)
组播：[组播原理-链接](https://blog.csdn.net/cong_xue/article/details/78639611?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522160766496919725211917529%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=160766496919725211917529&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_v2~rank_v29-10-78639611.pc_search_result_cache&utm_term=%E7%BB%84%E6%92%AD&spm=1018.2118.3001.4449)
组播就是计算机都配成同一个组播地址，就加入了同一个组。
IGMP，就是在路由器上控制有没有必要将多播数据包传到绑定多播地址的计算机的一种协议。

#### 1.6 ARP协议
PC1  => PC2
- PC1 将PC2的IP地址封装，广播询问  该IP  相对应的  MAC地址
- PC2收到后，返回响应
- 其他PC收到后，发现不是发给自己的，就丢弃
查看IP 与 MAC 的对应关系：`arp -a `
临时绑定MAC和IP地址：`arp -s  IP地址  MAC地址(带有-)`
删除MAC与IP的对应关系：`arp -d `


#### 1.7 跨网段通信：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201211140109229.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)

PC1 发送广播ARP广播，得到网关R1的MAC地址，不断变换MAC源地址、目标MAC，直到找到目标PC2.
- MAC地址 决定了  **下一跳**给哪个设备。
- IP地址 决定了 **最终**给 哪个 计算机
- 该跨网段的过程运用了`代理ARP`的原理：
	-  跨网段时，PC1将ARP请求发给网关，由网关【以网关自己的名义】去请求目的设备的ARP地址。
	- 如果PC1的ARP不是配成网关，而是直接将目的MAC地址配成跨网段的设备的MAC地址，则在ARP请求时，网关收到ARP请求，发现不是发给自己的包，就丢弃，所以这时不能跨网段通信。

