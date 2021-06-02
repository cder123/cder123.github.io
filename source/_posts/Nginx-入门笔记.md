---
title: Nginx-入门
tag: Nginx
categories:
  - 后端
  - 服务器
  - Nginx
---



# Nginx-基础

[toc]







## 0. 教程

-   [b站-Nginx通俗视频教程](https://www.bilibili.com/video/BV1zJ411w7SV?p=2)













## 1. Nginx的相关概念
- **正向代理：** 隐藏客户机。利用正向代理服务器的 ip 访问服务器的数据【fan墙】
- **反向代理：** 隐藏服务器。利用反向代理服务器的ip，将服务端的数据传给客户机【服务器的负载分担】
- **动静分离：** 准备若干台服务器，一部分服务器专门存放**静态资源**(html+css+js+img等)，另一部分服务器存放**动态的资源**。



















## 2. Nginx的安装+启动
1.  ssh 连接到 Linux（以下以centos为例）
2. 下载依赖、nginx： openssl + pcre + zlib +nginx
3. 安装完后，nginx的根目录为：`/usr/local/nginx/`



-   安装 openssl +zlib：

```bash
yum -y install make zlib zlib-devel gcc-c++ libtool  openssl openssl-devel
```



-   安装pcre:	

```bash
#下载最新版本的，注意不要用pcre2
wget https://jaist.dl.sourceforge.net/project/pcre/pcre/8.42/pcre-8.42.tar.gz

#解压
tar -xvf pcre-8.42.tar.gz


cd pcre-8.42 


./configure

# 安装编译
make && make install

#查看pcre版本
pcre-config --version
```


-   下载 + 安装 + 启动 Nginx:

```bash
wget https://nginx.org/download/nginx-1.15.9.tar.gz

tar -xvf nginx-1.15.9.tar.gz

cd nginx-1.15.9

./configure

# 编译安装
make && make install


#安装后的nginx在：
/usr/local/nginx

# nginx的启动脚本：
cd /usr/local/nginx/sbin/

# 启动 nginx
./nginx

# 查看nginx是否启动成功：
ps -ef | grep nginx


```

















## 3. Nginx常用命令

 **Nginx命令：** 所有nginx命令都必须在 `/usr/local/nginx/conf` 目录下进行
```bash
# nginx配置文件的位置,所有nginx命令都必须在此目录下进行
cd /usr/local/nginx/conf

# 1. 查看nginx版本号
./nginx -v

# 2. 启动nginx
./nginx

# 3. 关闭nginx
./nginx -s stop

# 4. 重启nginx
./nginx -s reload
```



















## 4. 配置Nginx

1. 开放防火墙(可选,在虚拟机中 |  ecs中开放安全组的相关端口)：
```bash
# 查看防火墙的端口
firewall-cmd --list-all

# 添加端口
sudo firewall-cmd --add-port=8001-tcp --permanent

# 重启防火墙
firewall-cmd --reload

```

2. **Nginx的配置文件** 的位置: `/usr/local/nginx/conf/nginx.conf`

```bash
cd /usr/local/nginx/conf

vim ./nginx.conf
```
Nginx的配置文件的3部分组成【每个层次都有1个全局块】：
- **全局块：** 最外面1层的内容，影响**全局运行配置**
> 例如：
> worker_processes  1;	// nginx处理的并发数，越大越多

- **events块：** 影响网络连接
> worker_connections  1024;	//最大连接数
- **http块：** 最频繁操作的部分，**代理+缓存+日志**等内容
	- http全局块：文件引入+MIME类型+超时+日志+单次请求数上限
	- server块：虚拟主机相关
		- 全局server：
		- location server：





















## 5. 案例1：反向代理的配置1
**效果：** 
> 浏览器访问Nginx的域名时，跳转到 Tomcat 的页面

### 5.1 步骤一：安装Tomcat

[tomcat的安装](https://blog.csdn.net/zhaoyanjun6/article/details/79131856)
1. 安装tomcat,使用其默认的8080端口：`yum -y install tomcat`,
2. 安装目录在 ：`/usr/share/tomcat`，
3. 查看tomcat的状态：`systemctl status tomcat`，
4. 启动Tomcat：`systemctl start tomcat`，
5. 安装tomcat的web界面：`cd webapps/` + `yum install -y tomcat-webapps tomcat-admin-webapps`
6. tomcat 的其他命令：
> 关闭：systemctl stop tomcat
> 重启：systemctl restart tomcat
> 开机启动：systemctl enable tomcat
>
> **注意：** 要开放防火墙的8080端口【ecs则要配置安全组的8080端口】

```bash
# 开放端口
firewall-cmd   --add-port=8080/tcp   --permanent

# 重启防火墙
firewall-cmd   --reload

# 查看端口开放情况
firewall-cmd   --list-all

```



### 5.2 步骤二：配置Nginx反向代理
1. 域名解析：
> 为了实现**访问 nginx的域名 =》tomcat**。因为目前**手上没域名**，使用我们需要先修改本地Windows的**hosts文件**，来实现域名的ip的映射。
> `C:\Windows\System32\drivers\etc\hosts`
2. 配置Nginx的请求转发

```bash
cd /usr/local/nginx/conf

vim ./nginx.conf
```
> 找到 `server`下的`server_name`,将后面的localhost改为 nginx的 ip地址
> 将 `location` 的大括号内,添加`proxy_pass http://127.0.0.1:8080;`
> 保存：`:wq`
3. 重启nginx,使得配置生效：

```bash
cd /usr/local/nginx/sbin

./nginx -s reload
```



















## 6. 案例2：反向代理的配置2

**效果：** 
> 浏览器访问 **Nginx** 的域名时【监听9001】，根据域名后的**路径不同**，跳转**不同的页面**【遇到edu=>跳到8080的服务器；遇到vod=>跳到8081的服务器】
### 6.1 准备
准备两个tomcat服务器,1个开放8080，一个开放8081

### 6.2 创建测试页面
在两台服务器的 tomcat 的安装目录下的 webapps内分别创建文件夹：
- edu：a.html
- vod：b.html

### 6.3 配置Nginx反向代理
1. 开放 9001，8080，8081端口
2. 打开配置文件
```bash
cd /usr/local/nginx/conf

vim ./nginx.conf
```
3. 修改 nginx.conf

	- 将原来 被注释的server及其内部 取消注释，将监听的端口改为9001
	- server_name 后面的对应项改为 ip地址
	- 将location复制1份，粘贴
```bash
server {
	listen       9001;	#监听9001端口
	
	server_name  相应的ip地址;
	
	location ~ /edu/ {	# 遇见/edu路径，转发到8080端口
		proxy_pass http:127.0.0.1:8080;
	}

	location ~ /vod/ {	# 遇见/vod路径，转发到8081端口
		proxy_pass http:127.0.0.1:8081;
	}
```
> **注意：**
> location后，大括号前的为正则表达式，用于匹配路径。
> 主要有以下几项：
> + ~   表示 区分大小写的正则表达式
> + ~* 表示 不区分大小写的正则表达式
> + =   表示严格匹配
4. 重启nginx【`/usr/local/nginx/sbin`】：`./nginx -s reload`



















## 7. 案例3：负载均衡

 



### 7.1 负载均衡的概念：

>   通过 增加1台 负载均衡服务器，由 `负载均衡服务器` 将 `客户端的请求` 分发到`多台服务器`中









### 7.2 效果：

>   在浏览器的地址栏，输入：`http://ip地址/edu/a.html` ,负载均衡后，平均请求到8080、8081两个端口。
>
>   即：将`http://ip地址:80/edu/a.html`的请求，
>
>   ​	=> 转发到`http://ip地址:8080/edu/a.html`
>
>   ​	=> 转发到`http://ip地址:8081/edu/a.html`



### 7.3 准备工作：

>   -   准备`2台Tomcat`服务器，1台监听`8080`，另一台监听`8081`
>   -   在2台Tomcat的 `WebApps目录`中，创建edu文件夹，并放入测试的 a.html【利用XFTP工具远程上传】
>   -   启动2台Tomcat





### 7.4 步骤：

-   在Nginx的`配置文件`中，`http块`内，配置以下的相关内容

```bash
// 新增项，(配置：分配策略)表示要转发到哪两个服务器【要点】
   upstream myserver{
     ip_hash;	// ip_hash策略
     server 123.60.25.23:8080 weight=1;	 // weight =1是默认值，值越大，分配到的请求越多
     server 123.60.25.23:8081 weight=1;
     fair;	// 第三方的分配策略
   }
   
    server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
            
// 新增项，【要点】
            proxy_pass http://myserver;
            proxy_connect_timeout 10;
        }
     }
```







### 7.5 Nginx负载均衡的分配策略：



-   `轮询` -（**默认的策略**）:【按照时间逐一分配，若服务器down掉，自动剔除。】
-   `weight权重`：默认1，数字越高表示分配的请求越多。
-   `ip_hash`：根据请求的IP地址的 hash值来决定分配的服务器，该策略可以保证某个客户端每次请求都分配同一个服务器，解决了Session的问题。
-   fair：【第三方的策略】按照响应时间来分配，响应快的优先。



注意：当轮询几率相同时，weight权重 与 访问率 成正比。适用于后端服务器性能不一的情况。







