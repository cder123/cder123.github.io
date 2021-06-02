---
title: CCNA-笔记-01【非理论】
tag: CCNA
categories:
  - 网络工程
  - CCNA 
---
# CCNA-笔记【非理论】

[toc]

## 第一章 计算机网络
### 1.1 Internet的组成
- 实际上由 **ISP + 企业网 +网民**  组成。
- 用户接入ISP,不同的ISP之间也有 线路相连【但跨运营商网速慢】
- 服务器一般托管到运营商机房，为适应不同运营商之间的网速，一般服务器的机房有多个ISP 的接口。

### 1.2 局域网与广域网
#### 1.2.1 局域网与广域网
- 局域网：自己花钱组网，带宽固定
- 广域网：借助运营商【ISP】的线路组网 ，花钱租带宽。


#### 1.2.2 局域网的三层：
- 接入层交换机：接口多，但单个口的带宽小
- 汇聚层交换机：
- 核心层交换机【路由器】：服务器接入到核心层，接口少，但单个口的带宽大


#### 1.2.3 C/S:【客户机/服务器】：
- 一个设备是 C 还是 S 是根据角色划分的


#### 1.2.4 数据传输-分层：
- 为了便于传输，先拆分成小块并编号，到达 目的地 后重组
- 每一层的变化都是独立的，底层为高层服务

#### 1.2.5 OSI-7层：
- 应用层：应用程序；
- 表示层：表示、处理数据【数据是 二进制 还是 ASCII码】；压缩、解压、加密、解密
- 会话层：维持不同应用程序的数据分割。
- 传输层：可靠/不可靠；可靠时 =》检错、纠错、重传、流控
- 网络层：根据逻辑地址选择最佳路径
- 数据链路层：利用MAC地址提供对介质的访问，只检错、不纠错
- 物理层：比特流，电压大小、线路速率、接口标准

#### 1.2.6 查看会话【步骤】：
- <kbd>Win + R</kbd>
- 输入：`netstat -n`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201209094109324.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)

#### 1.2.7 查看程序与会话的对应关系
-  管理员身份运行 cmd，入`netstat -nb`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201209094214378.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
#### 1.2.8 找到进程所在位置【两种方法】：
- `Ctrl + Alt + Delete` => `任务管理器`  => `选中进程` => `右键-打开文件位置`
- 在`任务管理器`表头`右键`=> 勾选命令行  

#### 1.2.9 查看运行的服务【有多种】
- `win + r` , 输入 `msconfig`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201209095626352.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)

#### 1.2.10 服务与启动项的区别：
- 服务：只要开启服务，不管 登不登陆 都运行【针对计算机】
- 启动项：用户登录后才运行【针对用户】

#### 1.2.11 UDP
不建立会话，采用广播的形式，不可靠，节省资源

#### 1.2.12 网络排错 和 OSI
由底层向上层排错
- 物理层问题：【替换法】
	 1. 网线是否连接正确；网线是否损坏
	 2. 查看网卡 接受/发送数据包是否有异常【如：1方为0】
	 3. 禁用、启用网卡，查看是否正常
	 4. 在 **设备管理器** 中  重新安装网卡的**驱动**【右键卸载(不勾选删除)、右键扫描】
- 数据链路层问题：
	 1.  MAC地址冲突
	 2.  ADSL拨号
- 网络层问题：
	1. 网关错误
	2. 路由表错误
- 应用程序问题：
	1. IE 代理错误
#### 1.2.13  网络安全和 OSI
- 物理层：不要混用接口
- 数据链路层：MAC地址认证，ADSL账号密码，划分Vlan
- 物理层：路由器ACL
- 传输层：端口安全
- 应用层安全：网站安全、操作系统安全

#### 1.2.14 网络设备
网络设备：
- 网线：双绞线=》 8根 =》 百米
	1. 10M
	2. 100M【4根线】
	3. 1000M【8根线】
	- 同种设备：交叉线
	- 不同设备：直通线
-	网卡：MAC地址=》物理地址、硬件地址=》 不可更改

- 查看 IP地址和 MAC地址：`ipconfig /all`

- 更改MAC地址：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201209103436535.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201209103508130.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201209103832511.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201209103858212.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
注意：下图3中的 mac 地址不要有 “--”
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201209104044757.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)

- 集线器：不看源地址和目的地址，只广播发送，计算机收到数据后发现不是自己的才丢弃，共享带宽，不安全。
- 网桥：根据数据包的源地址学习到设备的位置，只在第一次不知道目的地址时广播，在第二次发送数据时已经知道目的MAC地址就可以不用广播而直接发。独享带宽。


-单工：电视台发信号
- 半双工：对讲机，同一时间只能由一方通信
- 全双工：同时收发

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201209105451457.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201209105536874.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
交换机：
- 基于MAC地址转发数据，安全，带宽独享、全双工

路由器：
- 负责跨网段通信，一般有广域网口
- 隔绝广播【二进制全1 的目标ip，全F的 mac 】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201209110340850.png)
计算机上设置多个IP地址：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201209110836160.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201209110915186.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020120911094499.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)