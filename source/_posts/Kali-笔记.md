---
title: Kali-安装笔记
tag: Linux
categories:
  - [后端,操作系统,Linux使用]
  - [信息安全,系统安全]

---

# KaLi—— 笔记
[toc]
下载地址：【国内镜像】

```bash
搜狐：http://mirrors.sohu.com/
网易：http://mirrors.163.com/
阿里云：http://mirrors.aliyun.com/
腾讯：http://android-mirror.bugly.qq.com:8080/（仅针对APP开发的软件，限流，不推荐）
淘宝：http://npm.taobao.org/

上海交通大学：http://ftp.sjtu.edu.cn/html/resources.xml（部分移动运营商出口状况不佳，无法访问）
华中科技大学：http://mirror.hust.edu.cn/（当前已用容量估计：4.83T）
清华大学：http://mirrors.tuna.tsinghua.edu.cn/（当前已用容量估计：9.8T）
北京理工大学：http://mirror.bit.edu.cn/web/
兰州大学：http://mirror.lzu.edu.cn/
中国科技大学：http://mirrors.ustc.edu.cn/（当前已用容量估计：21.32T）
大连东软信息学院：http://mirrors.neusoft.edu.cn/（当前已用容量估计：2.5T）
东北大学：http://mirror.neu.edu.cn/
大连理工大学：http://mirror.dlut.edu.cn/
哈尔滨工业大学：http://run.hit.edu.cn/html/（部分联通运营商出口状况不佳，无法访问）
北京交通大学：http://mirror.bjtu.edu.cn/cn/
天津大学：http://mirror.tju.edu.cn（无法访问，ping超时）
中国地质大学：http://mirrors.cug.edu.cn/（当前已用容量估计：2.3T）
浙江大学：http://mirrors.zju.edu.cn/
厦门大学：http://mirrors.xmu.edu.cn/
中山大学：http://mirror.sysu.edu.cn/
重庆大学：http://mirrors.cqu.edu.cn/（当前已用容量估计：3.93T）
北京化工大学：http://ubuntu.buct.edu.cn/（Android SDK镜像仅供校内使用，当前已用容量估计：1.72T）
南阳理工学院：http://mirror.nyist.edu.cn/
中国科学院：http://www.opencas.org/mirrors/
电子科技大学：http://ubuntu.uestc.edu.cn/（无法访问，ping超时）
电子科技大学星辰工作室：http://mirrors.stuhome.net/（当前已用容量估计：1.08T）
西北农林科技大学：http://mirrors.nwsuaf.edu.cn/（只做CentOS镜像，当前已用容量估计：140GB） 
浙江大学：http://mirrors.zju.edu.cn/
台湾淡江大学: http://ftp.tku.edu.tw/Linux/

首都在线科技股份有限公司（英文名Capital Online Data Service）：http://mirrors.yun-idc.com/
中国电信天翼云：http://mirrors.ctyun.cn/
noc.im：http://mirrors.noc.im/（当前已用容量估计：3.74T）
常州贝特康姆软件技术有限公司：http://centos.bitcomm.cn/（只做CentOS镜像，当前已用容量估计：140GB）
公云PubYun（母公司为贝特康姆）：http://mirrors.pubyun.com/
Linux运维派：http://mirrors.skyshe.cn/（使用阿里云服务器，界面使用浙江大学的模板，首页维护，内容可访问）
中国互联网络信息中心：http://mirrors.cnnic.cn/（只做Apache镜像，当前已用容量估计：120GB）
Fayea工作室：http://apache.fayea.com/（只做Apache镜像，当前已用容量估计：120GB）
开源中国社区 http://mirrors.oss.org.cn/
```

## 1. 安装
参考【部分不一致】：
`https://zhuanlan.zhihu.com/p/107667275?from_voters_page=true`
文件= 》 新建虚拟机 =》 经典 = 》稍后安装 =》 Linux =》 
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201128190857420.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201128191053589.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201128191208991.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201128191337918.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201128191517333.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201128191551465.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201128191640963.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
选择第一项：图形化安装【由于我们刚才分配给虚拟机的内存太小（只有 512 MB），所以，只出现这种难看的界面】
然后：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201128191829246.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201128191915611.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201128191949932.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/202011281927033.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201128192721872.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020112819281250.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201128192916509.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201128192950314.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201128193153727.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201128193207605.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201128193225924.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020112819325350.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201128193316391.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201128193529659.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
在这里选择【图形界面的种类】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201128193622921.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201128193744420.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201128194928831.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201128194957601.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201128195238913.png)




















## 2. 初始设置
### 2.1 设置分辨率【以下为：2020.4版】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201128153043764.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)























### 2.2 设置网络
使用VM中的 NAT 模式
要注意：
- 物理机是否启用 vm 的 nat 服务
- 是否将 网卡设为可共享- 
- **锐捷校园网的客户端每隔一段时间就会关掉vm的nat服务**
- 进入`/etc/network/interfaces`文件，仿照 lo0添加网卡，保存；重启服务 `/etc/init.d/networking restart`


使用VM中的 NAT 模式的**步骤**： 

- `win + R`，输入`services.msc` , 找到`VM NAT Server`服务，启动
- 设置vmnet8 网卡的 ip 与 网关【两个要在同一网段】
- 在vm 中打开虚拟网络编辑器，选择 vmnet8
- 在下方填写 **子网网段 和 掩码** ，
- 点击 NAT 设置，输入**网关IP**【该IP不能和物理机中vmnet8的IP相同，但要与物理机中vmnet8 的网关相同】


### 2.3 更改鼠标大小
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130141604663.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
### 2.4 调整 终端 字体
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130142910861.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)
### 2.4 取消自动休眠
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201130145923390.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2NTc4NTky,size_16,color_FFFFFF,t_70)



### 2.4 更换软件安装源
- 打开终端
- 切换到 root 用户
- 注释掉 `/etc/apt/sources.list` 文件中的源，并换上国内源，保存
- 切换回普通用户


```bash
# 切换用户
sudo su
# 输入密码
kali-test
# 更改软件安装源
vim vim /etc/apt/sources.list
```
注释掉原来的内容，并复制 下列内容到 sources.list 中

```bash
#中科大
deb http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
deb-src http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
 
#阿里云
deb http://mirrors.aliyun.com/kali kali-rolling main non-free contrib
deb-src http://mirrors.aliyun.com/kali kali-rolling main non-free contrib
 
#清华大学
deb http://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling main contrib non-free
deb-src https://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling main contrib non-free
 
#浙大
deb http://mirrors.zju.edu.cn/kali kali-rolling main contrib non-free
deb-src http://mirrors.zju.edu.cn/kali kali-rolling main contrib non-free
 
#东软大学
deb http://mirrors.neusoft.edu.cn/kali kali-rolling/main non-free contrib
deb-src http://mirrors.neusoft.edu.cn/kali kali-rolling/main non-free contrib
 
#官方源
deb http://http.kali.org/kali kali-rolling main non-free contrib
deb-src http://http.kali.org/kali kali-rolling main non-free contrib
 
deb http://mirrors.163.com/debian/ jessie main non-free contrib
deb http://mirrors.163.com/debian/ jessie-updates main non-free contrib
deb http://mirrors.163.com/debian/ jessie-backports main non-free contrib
deb-src http://mirrors.163.com/debian/ jessie main non-free contrib
deb-src http://mirrors.163.com/debian/ jessie-updates main non-free contrib
deb-src http://mirrors.163.com/debian/ jessie-backports main non-free contrib
deb http://mirrors.163.com/debian-security/ jessie/updates main non-free contrib
deb-src http://mirrors.163.com/debian-security/ jessie/updates main non-free contrib
```

`:wq`保存后，执行以下命令

```bash
apt-get update  # 取回更新的软件包列表信息
apt-get upgrade # 进行一次升级
apt-get clean # 删除已经下载的安装包
reboot  #重启
```

