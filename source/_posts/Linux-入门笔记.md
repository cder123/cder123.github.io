---
title: CentOS_7 配置笔记
tag: Linux
categories:
  - [后端,操作系统,Linux]
---







# CentOS_7 配置笔记

[toc]





## 1. JDK 安装

1. 下载：`https://mirrors.tuna.tsinghua.edu.cn/AdoptOpenJDK/8/jdk/x64/linux/`
2. 上传到CentOS:
	- XShell在 `/usr.`下新建一个目录用于作为JAVA_HOME:`/usr/java/jdk`
	- XFTP将 jdk 上传到`/use/java/jdk`目录【上述目录】
3. 解压【进入上述目录】： `tar -xzvf     jdk压缩包名     -C    目标目录`，
	即：`tar -xzvf OpenJDK8U-jdk_x64_linux_openj9_8u282b08_openj9-0.24.0.tar.gz -C /usr/java/jdk`
4. 给解压后的文件夹改一个短的名字：`mv ./jdk8u282-b08 ./jdk8`
5. 设置jdk的环境变量：
	 - `echo  "export JAVA_HOME=/usr/java/jdk/jdk8" >> /etc/profile`	
	- `echo  "export CLASSPATH=.:$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib" >> /etc/profile`

	- `echo  "export PATH=$JAVA_HOME/bin:$PATH" >> /etc/profile`
6. 使环境变量生效：`source /etc/profile`
7. 验证jdk使用可用：`java -version`













## 2. MySQL 安装

1. 下载源：`wget http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm`
2. 安装源：`yum localinstall mysql57-community-release-el7-8.noarch.rpm`
3. 检测源是否安装完成：`yum repolist enabled | grep "mysql.*-community.*"`
4. 安装mysql：`yum install mysql-community-server`
5. 启动mysql 服务：`systemctl enable mysqld`
6. 查看mysql 版本：`rpm -aq | grep -i mysql`
7. 重启mysql 服务：`systemctl restart mysqld`
8. 查看初始密码：`grep 'A temporary password' /var/log/mysqld.log`
9. 更改初始密码：`mysqladmin -u root -p'旧密码' password '新密码'`【若报错，可设置复杂的密码】
10. 设置权限：
	- 进入mysql：` mysql -uroot -p密码`
	- 授予权限：`grant all privileges on *.* to '用户名'@'ip地址' identified by '密码' with grant option;`【ip地址可改为`%%`以表示所有ip】
11. 设置轻应用服务器 防火墙的3306端口
12. 重启服务器

 **测试 mysql 连接**
 下面使用 MySQL WorkBench远程连接到mysql:
 ![mysql workbench](https://z3.ax1x.com/2021/06/02/2QBlwj.png)











## 3. Nginx 安装

 1. 创建Nginx的根目录并进入：如：`/usr/nginx`
 2. 下载： `wget https://nginx.org/download/nginx-1.15.9.tar.gz`
 3. 解压：`tar -xvf nginx-1.15.9.tar.gz`
 4. 进入：`cd ./nginx-1.15.9`
 5. 解决make报错：
		-  `yum -y install make zlib-devel gcc-c++ libtool openssl openssl-devel`
		- `./configure `
		- `make && make install`
 6. 创建nginx用户：`useradd nginx`
 7. 配置nginx： `./configure --user=nginx --group=nginx --prefix=nginx安装目录 --with-http_stub_status_module --with-http_ssl_module  `
 8. 编译nginx： `make && make install`
 9. 检查是否安装成功，命令：`cd /usr/local/nginx/sbin`、`./nginx -t`
 10. 创建软链接：`ln -s /usr/nginx安装目录/sbin/nginx /usr/sbin/`
 11. 安装完成后查看Nginx的相关环境配置信息是否正确 ，命令：`/usr/local/nginx/sbin/nginx –V`

Nginx命令：
- 目录：`/usr/nginx/installFolder/`
- 启动Nginx：`cd /usr/nginx/installFolder/sbin` ，`./nginx`
- 停止Nginx：`pkill -9 nginx`
- 查看nginx进程号及运行情况：`ps -ef | grep nginx`
- 测试nginx是否运行正常：`浏览器：ip地址`







## 4. httpd 的安装

- 下载安装：`yum -y install httpd`
- 设为启动项：`systemctl  enable  httpd`
- 启动httpd：`system restart  httpd`
- 查看是否运行成功：`systemctl status httpd` 
- 将编写好的html文件改为`index.html`，并传到`/var/www/html`目录中
- 将80端口改为8080端口：[更改端口](https://jingyan.baidu.com/article/86112f136573352736978744.html)
- 然后在服务器控制台开放8080端口，即可使用浏览器访问页面





## 5、最小化安装CentOS 7 后，安装桌面



```shell
vi /etc/resolv.conf


nameserver 114.114.114.114


yum groupinstall "X Window System"


yum groupinstall "GNOME Desktop"


ln -sf /lib/systemd/system/runlevel5.target /etc/systemd/system/default.target 


reboot


systemctl set-default graphical.target  //由命令行模式更改为图形界面模式
systemctl set-default multi-user.target  //由图形界面模式更改为命令行模式

```

