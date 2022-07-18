---
title: Shiro-笔记
tag: Java
categories:
  - [后端,Java,安全框架]

---





<h1>Shiro-笔记</h1>

[toc]



# 零、资料



> - [官网](https://shiro.apache.org/get-started.html)
> - [Apache Shiro Reference Documentation](https://shiro.apache.org/reference.html)
> - [2020-Shiro-视频](https://www.bilibili.com/video/BV1uz4y197Zm)
>
>  [Shiro-教程（打开前先切换为移动端的UA）](https://www.iocoder.cn/Spring-Boot/Shiro/?github)
>
> [Shiro学习笔记_01（权限管理+shiro基本概念+shiro核心架构）：](https://blog.csdn.net/A233666/article/details/113436397)
> [Shiro学习笔记_02（shiro的认证+shiro的授权）：](https://blog.csdn.net/A233666/article/details/113436604)
> [Shiro学习笔记_03（整合SpringBoot项目实战）：](https://blog.csdn.net/A233666/article/details/113436813)
> [Shiro学习笔记_04（Shiro整合springboot之 thymeleaf权限控制）：](https://blog.csdn.net/A233666/article/details/113436981)





# 一、简介



## 1、什么是Apache Shiro？



&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Apache Shiro 是开源安全框架，处理：**身份验证，授权，企业会话管理、加密**。



![image-20220410103927817](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220410103927817.png)

<center style="color:red">Shiro 的特征</center>





## 2、Shiro 的体系结构 3 个主要对象



[官网介绍](https://shiro.apache.org/architecture.html)



- Subject：相当于用户

- SecurityManager：相当于框架本身（至少需要一个）

- Realms：相当于数据库（类似DAO接口的作用）



![image-20220410104247640](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220410104247640.png)

<center style="color:red">Subject、SecurityManager、Realm</center>



## 3、架构图



![image-20220410105515814](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220410105515814.png)



<center style="color:red">详细架构图</center>



## 4、常用术语



- **身份验证**（Authentication）：证明是谁，例如：登录功能

- **授权**（Authorization）：访问控制，检查和解释使用者的角色和权限允许或拒绝访问请求的资源或功能来完成。

- **密码算法**（Cipher）：用于执行加密或解密的算法（通常依赖于**密钥**）。

- **凭据**（Credential）：用户输入的密码，将凭据与主体一起提交，验证用户是否确实是关联的用户

- **密码学**（Cryptography）：加密（Shiro专注于密码学的两个核心元素：密码、消息摘要）

- **哈希**（hash）：哈希函数

- **权限**（Permission）：描述应用程序中的原始功能的语句，如：打开文件、查看“/用户/列表”网页

- **主体**（Principal）：理想的主主体是用户名或作为RDBMS用户表主键的用户ID之类的东西。应用程序中只有一个用户（使用者）的主Subject。

- **领域**（Realm）：特定于安全性的DAO（数据访问对象），可以访问特定于应用程序的安全数据，例如用户、角色和权限。

- **角色**（Role）：Shiro 更喜欢将角色解释为权限的命名集合。就是这样 - 聚合一个或多个权限声明的应用程序唯一名称。

- **会话**（Session）

- **主题**（Subject）：用户特定于安全性的“视图”。



## 5、权限管理（身份认证 + 授权）



权限管理 = 身份认证 + 授权，简称：<font style="color:red;">认证授权</font>



身份认证：输入的用户名和密码对不对（Principal对象、Credential对象）

授权：访问控制（前提：身份认证），控制用户只能访问自己被授权的资源。







# 二、Shiro 的配置-案例

[Apache Shiro Reference Documentation](https://shiro.apache.org/reference.html)

[Apache Shiro Configuration ](https://shiro.apache.org/configuration.html)

[快速配置](https://shiro.apache.org/spring-boot.html#web_applications)



Shiro 提供了一个用于快速学习的配置文件（`.ini`）



## 1、引入 Maven 依赖



```xml
<!-- 简单的案例的依赖 -->
	<dependency>
      <groupId>org.apache.shiro</groupId>
      <artifactId>shiro-core</artifactId>
      <version>1.9.0</version>
    </dependency> 



<!-- 完整的依赖 -->
<!--
	<dependency>
      <groupId>org.apache.shiro</groupId>
      <artifactId>shiro-spring-boot-web-starter</artifactId>
      <version>1.9.0</version>
    </dependency>
 -->
```



## 2、创建配置文件



`resource/shiro.ini`

```ini
[users]
root=123
admin=456
```





## 3、认证



先数据库，再用户。

常见异常：

> - 用户不存在：`UnknownAccountException`
> - 密码错误：`IncorrectCredentialsException`



```java
package com.cyw;

import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.mgt.DefaultSecurityManager;
import org.apache.shiro.realm.text.IniRealm;
import org.apache.shiro.subject.Subject;

public class AuthenticatorTest {
    public static void main(String[] args) {

        // 1. 创建Realm（dao接口）
        IniRealm realm = new IniRealm("classpath:shiro.ini");

        // 2. 创建安全管理器
        DefaultSecurityManager securityManager = new DefaultSecurityManager();

        // 3. 设置 Realm
        securityManager.setRealm(realm);

        // 4. 设置安全管理器
        SecurityUtils.setSecurityManager(securityManager);

        // 5. 获取主体
        Subject subject = SecurityUtils.getSubject();

        // 6. 创建令牌（根据用户输入）
        UsernamePasswordToken token = new UsernamePasswordToken("root","123");
         token.setRememberMe(true);

        // 7. 主体执行登录认证
        try {
            System.out.println("认证之前。。。");
            subject.login(token);           
            System.out.println("认证之后。。。");
        } catch (Exception e) {
            e.printStackTrace();
            // 用户不存在，打印（未知账户异常）：UnknownAccountException
            // 密码错误，打印（错误凭证异常）：IncorrectCredentialsException
            System.out.println("认证失败。。。"+e.getMessage());
        }
        
        // 8. 打印Subject的认证状态
        System.out.println(subject.isAuthenticated());
        
        // 判断角色
        // subject.hasRole("administrator");
        
        // 判断权限
        // subject.isPermitted( "users:add" );
        
        // 退出
        subject.logout();

    }
}

```



认证过程中，执行`subject.login(token)`方法时，执行以下操作：

- 执行**用户名比较**的是：`SimpleAccountRealm`类的`doGetAuthenticationInfo()`方法。
- 执行**密码比较**的是：`AuthenticatingRealm`类的`assertCredentialsMatch()`方法。



总结：

> - `AuthenticatingRealm` ：认证 realm，执行`doGetAuthenticationInfo()`
> - `AuthorizingRealm`：授权 realm，执行`doGetAuthorizationInfo()`





## 4、授权



授权有三个核心元素：权限、角色、用户。



<h4>权限粒度的级别：</h4>

上述权限指定对资源（门、文件、客户记录等）的操作（打开、读取、删除等）。在 Shiro 中，您可以定义任意深度的权限。以下是按粒度顺序排列的几个常见权限级别。

- 资源级别 - 用户可以编辑客户记录。指定了资源，但不是该资源的特定实例。
- 实例级别 - 权限指定资源的实例。用户可以编辑 IBM 的客户记录。
- 属性级别 - 权限现在指定实例或资源的属性。用户可以编辑 IBM 客户记录上的地址。



<h4>角色：</h4>

-  隐式角色
- 显式角色





# 三、自定义 Realm



自定义Realm只需要 继承`AuthorizingRealm`类，覆盖重写`doGetAuthenticationInfo()`方法和`doGetAuthorizationInfo()`方法。



步骤：

（1）创建一个类`MyRealm`，继承`AuthorizingRealm`类，覆写2个方法：

```java
package com.cyw;

import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.AuthenticationInfo;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.authc.SimpleAuthenticationInfo;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;

public class MyRealm extends AuthorizingRealm {

    // 2. 授权
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        return null;
    }

    // 1. 认证
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {

        // 1.1 获取用户名
        String principal = (String)token.getPrincipal();


        // 1.2 根据用户名获取【数据库】中的用户信息
        String username = "root";
        String pwd = "123";


        // 1.3 比较用户输入的信息和数据库中的信息
        //         如果数据库中根据用户名查到的用户为null,表示没有该用户
        if(username.equalsIgnoreCase(principal)){
            // 1.4 根据数据库中的用户名、密码、当前Realm的名字创建认证信息
            SimpleAuthenticationInfo authenticationInfo = new SimpleAuthenticationInfo(username,pwd,this.getName());
            System.out.println("用户名认证成功！");
            return authenticationInfo;
        }

        return null;
    }
}

```



（2）测试自定义Realm的使用：

```java
package com.cyw;

import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.IncorrectCredentialsException;
import org.apache.shiro.authc.UnknownAccountException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.mgt.DefaultSecurityManager;
import org.apache.shiro.subject.Subject;

public class MyRealmTest {
    public static void main(String[] args) {
        
        MyRealm myRealm = new MyRealm();
        DefaultSecurityManager securityManager = new DefaultSecurityManager();
        securityManager.setRealm(myRealm);
        SecurityUtils.setSecurityManager(securityManager);
        Subject subject = SecurityUtils.getSubject();
        UsernamePasswordToken token = new UsernamePasswordToken("root", "1230");

        try {
            subject.login(token);
        } catch (UnknownAccountException e1) {
            e1.printStackTrace();
            System.out.println("用户名错误！");
        } catch (IncorrectCredentialsException e2) {
            e2.printStackTrace();
            System.out.println("密码错误！");
        }catch (AuthenticationException e3) {
            e3.printStackTrace();
            System.out.println("认证失败！");
        }finally {
            subject.logout();
        }

    }
}

```





# 四、MD5 与 加盐



![image-20220410173436502](https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220410173436502.png)



<center style="color:red">MD5与加盐流程图</center>

流程：

> - 注册，传入用户名、密码
> - 在 Service 层，随机生成一个盐，拼接到密码中，使用 MD5 加密，将盐与加密后的密码存入数据库
> - 登录，传入用户名、密码
> - Service 层，根据用户名获取数据库中的密码与盐，将当前用户输入的密码与数据库中查到的盐进行MD5计算，若与数据库中的密码一致，则登录成功。





Md5工具类：

```java
package com.cyw.utils;

import org.apache.shiro.crypto.hash.Md5Hash;

public class Md5Utils {
    // 盐
    private static final String salt = "abc";
    // 哈希值
    private static final int hash = 100;
    // 数据库中的密码
    private static final String dataPwd = "1632cab5305ea5363422b3afb8089f43";


    public static String getMd5(String pwd) {
        Md5Hash md5Hash = new Md5Hash(pwd, salt, hash);
        System.out.println("原密码："+pwd+" ，盐："+salt+" ,哈希值："+hash);
        System.out.println("加密后："+md5Hash.toHex());
        return md5Hash.toHex();
    }

    public static boolean verify(String pwd) {
        String md5 = getMd5(pwd);

        if (md5.equalsIgnoreCase(dataPwd)){
            return true;
        }
        return false;
    }
}

```













## 1、没加盐的MD5



（1）第1种MD5加密方式（没加盐）：

```java
package com.cyw;

import org.apache.shiro.crypto.hash.Md5Hash;

public class MD5Test {
    public static void main(String[] args) {

        String pwd = "123";

        Md5Hash md5Hash = new Md5Hash();
        md5Hash.setBytes(pwd.getBytes());
        String s = md5Hash.toHex();
        
        // 加密前：123
		// 加密后：313233
        System.out.println("加密前："+pwd);
        System.out.println("加密后："+s);
        System.out.println("\n===============");

    }
}

```



（2）第2种MD5加密方式（没加盐）：：

```java
package com.cyw;

import org.apache.shiro.crypto.hash.Md5Hash;

public class MD5Test {
    public static void main(String[] args) {

        String pwd = "123";
     
        // 第2种
        Md5Hash md5Hash2 = new Md5Hash(pwd);
        String s2 = md5Hash2.toHex();
        System.out.println("加密前："+pwd);
        System.out.println("加密后："+s2);
        System.out.println("\n===============");

    }
}

```







## 2、加盐的MD5



（1）第1种MD5加密方式（没加哈希值）：

```java
package com.cyw;

import org.apache.shiro.crypto.hash.Md5Hash;

public class MD5Test {
    public static void main(String[] args) {

        String pwd = "123";
        String salt = "abc";
        
        Md5Hash md5Hash3 = new Md5Hash(pwd,salt);
        String s3 = md5Hash3.toHex();
        
        System.out.println("加密前："+pwd+" ==> "+salt);
        System.out.println("加密后："+s3);
        System.out.println("\n===============");

    }
}

```



（2）第3种MD5加密方式（加哈希值）：

```java
package com.cyw;

import org.apache.shiro.crypto.hash.Md5Hash;

public class MD5Test {
    public static void main(String[] args) {

        String pwd = "123";
        String salt = "abc";
        int hash = 100;
        
        Md5Hash md5Hash4 = new Md5Hash(pwd,salt,hash);
        String s4 = md5Hash4.toHex();
        
        System.out.println("加密前："+pwd+" ==> "+salt+" ==> "+hash);
        System.out.println("加密后："+s4);
        System.out.println("\n===============");

    }
}

```





## 3、不加盐MD5-案例



`Md5Realm`类：

```java
package com.cyw.realm;

import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.AuthenticationInfo;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.authc.SimpleAuthenticationInfo;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;

public class MyMD5Realm extends AuthorizingRealm {
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        return null;
    }

    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {

        // 获取用户输入的信息
        String principal = (String)authenticationToken.getPrincipal();

        // 数据库中的数据
        String username = "root";
        String pwd = "123";
        String pwdHex = new Md5Hash(pwd).toHex();

        // 如果数据库中根据用户名查到的用户为null,表示没有该用户
        if(username.equalsIgnoreCase(principal)){
            return new SimpleAuthenticationInfo(username,pwdHex,this.getName());
        }

        return null;
    }
}

```



`Md5Realm`测试类：

```java
package com.cyw;

import com.cyw.realm.MyMD5Realm;
import com.cyw.realm.MyRealm;
import com.cyw.utils.Md5Utils;
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.authc.credential.HashedCredentialsMatcher;
import org.apache.shiro.mgt.DefaultSecurityManager;
import org.apache.shiro.subject.Subject;

public class MD5Test {
    public static void main(String[] args) {
        String username = "root";
        String pwd = "123";

        DefaultSecurityManager securityManager = new DefaultSecurityManager();
        MyMD5Realm realm = new MyMD5Realm();
        realm.setCredentialsMatcher(new HashedCredentialsMatcher("md5"));
        securityManager.setRealm(realm);
        SecurityUtils.setSecurityManager(securityManager);
        Subject subject = SecurityUtils.getSubject();

        UsernamePasswordToken token = new UsernamePasswordToken(username,pwd);

        try {
            subject.login(token);
            System.out.println("认证成功！！");
        } catch (AuthenticationException e) {
            e.printStackTrace();
        }

    }
}
```



## 4、加盐MD5-案例



`Md5Realm`类：

```java
package com.cyw.realm;

import com.cyw.utils.Md5Utils;
import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.AuthenticationInfo;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.authc.SimpleAuthenticationInfo;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.crypto.hash.Md5Hash;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;
import org.apache.shiro.util.ByteSource;

public class MyMD5Realm extends AuthorizingRealm {
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        return null;
    }

    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {

        // 获取用户输入的信息
        String principal = (String)authenticationToken.getPrincipal();

        // 数据库中的数据
        String username = "root";
        String pwd = "123";
        String salt = "abc";
        String pwdHex = (new Md5Hash(pwd,salt)).toHex();

        // 如果数据库中根据用户名查到的用户为null,表示没有该用户
        if(username.equalsIgnoreCase(principal)){
            return new SimpleAuthenticationInfo(username, pwdHex, ByteSource.Util.bytes(salt),this.getName());
        }

        return null;
    }
}

```







`Md5Realm`测试类：

```java
package com.cyw;

import com.cyw.realm.MyMD5Realm;
import com.cyw.realm.MyRealm;
import com.cyw.utils.Md5Utils;
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.authc.credential.HashedCredentialsMatcher;
import org.apache.shiro.mgt.DefaultSecurityManager;
import org.apache.shiro.subject.Subject;

public class MD5Test {
    public static void main(String[] args) {
        
        String username = "root";
        String pwd = "123";
        
        UsernamePasswordToken token = new UsernamePasswordToken(username,pwd);

        DefaultSecurityManager securityManager = new DefaultSecurityManager();

        MyMD5Realm realm = new MyMD5Realm();
        // 设置加密算法为：MD5算法
        HashedCredentialsMatcher matcher = new HashedCredentialsMatcher("MD5");
        // 设置散列次数
        matcher.setHashIterations(1);
        realm.setCredentialsMatcher(matcher);


        securityManager.setRealm(realm);
        SecurityUtils.setSecurityManager(securityManager);
        Subject subject = SecurityUtils.getSubject();


        try {
            subject.login(token);
            System.out.println("认证成功！！");
        } catch (AuthenticationException e) {
            e.printStackTrace();
        }

    }
}

```







# ————————————-



# Shiro 整合 SpringBoot ——登录案例



## 1、导入依赖



创建SpringBoot 的Web项目后，在`POM.xml`中导入如下依赖：

```xml
    <dependencies>
        <!--        Shiro 依赖-->
        <dependency>
            <groupId>org.apache.shiro</groupId>
            <artifactId>shiro-web</artifactId>
            <version>1.9.0</version>
        </dependency>
        <dependency>
            <groupId>org.apache.shiro</groupId>
            <artifactId>shiro-spring</artifactId>
            <version>1.9.0</version>
        </dependency>
        
        <!--        SpringBoot 依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.2.2</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        
        <!--        MySQL 8.0 依赖-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
        <!--        Lombok 依赖-->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>

    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <excludes>
                        <exclude>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
            </plugin>
        </plugins>
    </build>

```



## 2、利用 SpringBoot 配置 SSM 环境



### 2.1、创建数据表



```sql
create table permission(
	id int primary key auto_increment,
	permission varchar(10)
);
INSERT INTO `permission` (`id`, `permission`) VALUES (1, 'user:add');
INSERT INTO `permission` (`id`, `permission`) VALUES (2, 'user:del');
INSERT INTO `permission` (`id`, `permission`) VALUES (3, 'user:update');
INSERT INTO `permission` (`id`, `permission`) VALUES (4, 'user:find');


create table role(
	id int primary key auto_increment,
	role varchar(10)
);
INSERT INTO `role` (`id`, `role`) VALUES (1, 'admin');
INSERT INTO `role` (`id`, `role`) VALUES (2, 'normal');


create table role_permission(
	pid int,
	rid int,
	foreign key(pid) REFERENCES permission(id),
	foreign key(rid) REFERENCES role(id),
	primary key(pid,rid)
);
INSERT INTO `role_permission` (`pid`, `rid`) VALUES (1, 1);
INSERT INTO `role_permission` (`pid`, `rid`) VALUES (2, 1);
INSERT INTO `role_permission` (`pid`, `rid`) VALUES (3, 1);
INSERT INTO `role_permission` (`pid`, `rid`) VALUES (4, 1);
INSERT INTO `role_permission` (`pid`, `rid`) VALUES (1, 2);
INSERT INTO `role_permission` (`pid`, `rid`) VALUES (4, 2);


create table users(
	id int primary key auto_increment,
	username varchar(10)
);
INSERT INTO `users` (`id`, `username`, `pwd`) VALUES (1, 'zs', '123');
INSERT INTO `users` (`id`, `username`, `pwd`) VALUES (2, 'ls', '123');


create table role_users(
	uid int,
	rid int,
	foreign key(uid) REFERENCES users(id),
	foreign key(rid) REFERENCES role(id),
	primary key(uid,rid)
);
INSERT INTO `role_users` (`uid`, `rid`) VALUES (1, 1);
INSERT INTO `role_users` (`uid`, `rid`) VALUES (2, 2);


-- ********************************

-- users-role
SELECT
	users.*, role.role
FROM
	users,role,role_users
where users.id = role_users.uid
and role_users.rid = role.id

	
-- permission-role	
SELECT
	role.*,permission.permission
FROM
	role,permission,role_permission
where role.id = role_permission.rid
and role_permission.pid = permission.id



-- 5张表连接：用户、角色、权限、用户_角色、权限_角色
SELECT
	users.*, role.role,permission.permission
FROM
	users,role,role_users,permission,role_permission
where users.id = role_users.uid
and role_users.rid = role.id
and role.id = role_permission.rid
and role_permission.pid = permission.id
and username = 'zs'

```









### 2.2、POJO（实体类）



实体类需要：`用户`、`角色`（本案例中没用到）、`权限`。





<font style="color:red;font-size:1.2em;">注意：</font>数据库中的权限设计：`资源名：操作名`，

例如：`users:add`（完整版：`资源名：操作名：实例id`）









#### 2.2.1、统一结果集 Result 类



`com.cyw.test02.pojo.Result`：

```java
package com.cyw.test02.pojo;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@AllArgsConstructor
@NoArgsConstructor
@Data
public class Result {
    private  boolean flag;
    private  String msg;
    private  Object data;

    public static Result ok(String msg,Object data){
        return new Result(true,msg,data);
    }

    public static Result fail(String msg){
        return new Result(false,msg,null);
    }
}
```







#### 2.2.2、用户类 User

`com.cyw.test02.pojo.User`：

```java
package com.cyw.test02.pojo;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import java.util.List;


@AllArgsConstructor
@NoArgsConstructor
@Data
public class User {
    private int id;
    private String username;
    private String pwd;
    private List<Permission> permissionList;
}
```





#### 2.2.3、权限类 Permission

`com.cyw.test02.pojo.Permission`：

```java
package com.cyw.test02.pojo;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;


@AllArgsConstructor
@NoArgsConstructor
@Data
public class Permission {
    private int id;
    private String permission;
}
```









### 2.3、配置 Mybatis



#### 2.3.1、Mybatis 的 核心配置文件

`src/main/resources/mybatis-config.xml`

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>

</configuration>
```







#### 2.3.3、Mybatis 的 Mapper 接口



`src/main/java/com/cyw/test02/mapper/UserMapper.java`

```java
package com.cyw.test02.mapper;

import com.cyw.test02.pojo.User;
import org.apache.ibatis.annotations.Mapper;

@Mapper
public interface UserMapper {
    User getUserByUserName(String username);
}

```







#### 2.3.3、Mybatis 的 Mapper 配置文件



`src/main/resources/com/cyw/test02/mapper/UserMapper.xml`

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.cyw.test02.mapper.UserMapper">

    <resultMap id="UserRstMap" type="User">
        <id property="id" column="id"/>
        <result property="username" column="username"/>
        <result property="pwd" column="pwd"/>
        <collection property="permissionList" column="permission" ofType="Permission" javaType="list">
            <id property="id" column="permissionId"/>
            <result property="permission" column="permission"/>
        </collection>
    </resultMap>

    <select id="getUserByUserName" parameterType="string"
            resultMap="UserRstMap">
        SELECT
            users.*, role.role,permission.permission,role.id as roleId,permission.id as permissionId
        FROM
            users,role,role_users,permission,role_permission
        where users.id = role_users.uid
          and role_users.rid = role.id
          and role.id = role_permission.rid
          and role_permission.pid = permission.id
          and username = #{username}
    </select>

</mapper>
```



#### 2.3.3、Mybatis 的 Mapper 包扫描



在SpringBoot的启动类上添加注解`@MapperScan("mapper接口的包路径")`：

```java
package com.cyw.test02;

import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
@MapperScan("com.cyw.test02.mapper")
public class Test02Application {

    public static void main(String[] args) {
        SpringApplication.run(Test02Application.class, args);
    }
}
```







### 2.4、修改 SpringBoot 的配置文件



`src/main/resources/application.yml`

```yml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/shiro_study?ServerTimezone=UTC
    username: root
    password: root
    driver-class-name: com.mysql.cj.jdbc.Driver

mybatis:
  config-location: classpath:mybatis-config.xml
  type-aliases-package: com.cyw.test02.pojo

server:
  servlet:
    context-path: /
```



### 2.5、Service 层



`UserService`接口：

```java
package com.cyw.test02.service;

import com.cyw.test02.pojo.User;

public interface UserService {
    User getUserByUserName(String username);
}
```





`UserServiceImpl`接口实现类：

```java
package com.cyw.test02.service.impl;

import com.cyw.test02.mapper.UserMapper;
import com.cyw.test02.pojo.User;
import com.cyw.test02.service.UserService;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;
import javax.annotation.Resource;


@Transactional
@Service("userService")
public class UserServiceImpl implements UserService {
    @Resource
    private UserMapper userMapper;

    @Override
    public User getUserByUserName(String username) {
        return userMapper.getUserByUserName(username);
    }
}

```





### 2.6、Controller 层



位置：`src/main/java/com/cyw/test02/controller`



页面跳转的Controller：

```java
package com.cyw.test02.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class IndexController {
    
    @RequestMapping("/")
    public String index(){
        return "index.html";
    }

    @RequestMapping("/main")
    public String main(){
        return "main.html";
    }

    @RequestMapping("/403")
    public String unAuthen(){
        return "403.html";
    }

}
```



返回数据的Controller【登录功能】：

```java
package com.cyw.test02.controller;

import com.cyw.test02.pojo.User;
import com.cyw.test02.service.UserService;
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.IncorrectCredentialsException;
import org.apache.shiro.authc.UnknownAccountException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.subject.Subject;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import javax.annotation.Resource;
import com.cyw.test02.pojo.Result;

@RestController
@RequestMapping("/user")
public class UserController {
    @Resource
    private UserService userService;

// 使用Shiro
    @PostMapping("/login")
    public Result login(User user){
		// 前端没有传用户名和密码
        if(user == null){
            return Result.fail("请先输入用户名和密码");
        }
        
		// Subject 是Shiro 的对象，代表当前用户，
        // 执行该对象的login()方法就可进入Realm进行登录验证
        Subject subject = SecurityUtils.getSubject();

        UsernamePasswordToken token = new UsernamePasswordToken(user.getUsername(), user.getPwd());

        try {
            、// 执行认证，进入Realm进行具体的校验
            subject.login(token);
            System.out.println("====》  认证通过........");
        } catch (UnknownAccountException ex1){
            return  Result.fail("用户名错误！");
        } catch (IncorrectCredentialsException ex2){
            return  Result.fail("用户名错误！");
        } catch (AuthenticationException e) {
           return  Result.fail("用户名或密码错误！");
        }

        User userByUserName = userService.getUserByUserName(user.getUsername());
        return Result.ok("请求成功",userByUserName);
    }

// 没有使用 Shiro :
//    @PostMapping("/login")
//    public Map login(User user){
//        HashMap<String, Object> map = new HashMap<>();
//        if(user == null){
//            map.put("code",400);
//            map.put("msg","请先输入用户名和密码");
//            map.put("data",null);
//            return map;
//        }
//        User userByUserName = userService.getUserByUserName(user.getUsername());
//
//        map.put("code",200);
//        map.put("msg","请求成功");
//        map.put("data",userByUserName);
//        return map;
//    }
    
}
```



### 2.7、HTML 页面



位置：`src/main/resources/static/`



登录页：

```html
<!DOCTYPE html>
<html lang="zh-cn">
<head>
    <meta charset="UTF-8">
    <title>登录</title>
    <script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.6.0/jquery.js"></script>
</head>
<body>

<div style="margin-top:200px;">
    <center>
       <form id="form">
           用户名：<input id="username" name="username" type="text" placeholder="用户名"> <br>
           密码：<input id="pwd" type="text" name="pwd" placeholder="密码"> <br>
           <input id="btn" type="button" value="提交"><br>
       </form>
    </center>
</div>

<script>
    $("#btn").click(function (){
        $.post("/user/login",$("#form").serialize(),function (resp) {
            console.log(resp);
            if (resp){
                location.href = "/main"
            }
        });
    });
</script>

</body>
</html>
```



主页：

```hrml
<!DOCTYPE html>
<html lang="zh-cn">
<head>
    <meta charset="UTF-8">
    <title>主页</title>
    <script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.6.0/jquery.js"></script>

</head>
<body>
    <h1>主页</h1>
    <div>

    </div>
    <script>
      
    </script>
</body>
</html>
```



未授权页：

```html
<!DOCTYPE html>
<html lang="zh-cn">
<head>
    <meta charset="UTF-8">
    <title>未授权</title>
</head>
<body>

    <h1>未授权</h1>

</body>
</html>
```





## 3、配置 Shiro





### 3.1、自定义Realm类 MyRealm



继承`AuthorizingRealm`，重写认证和授权的2个方法。



认证流程：

> （1）用户输入`用户名`和`密码`，传给 `Controller`，
>
> （2）将`用户名`和`密码`封装为`UsernamePasswordToken`对象
>
> （3）通过`SecurityUtils`工具类获取代表当前用户的`Subject`对象
>
> （4）执行`Subject`对象的`login(token)`方法（并捕获异常）
>
> （5）进入自定义的Realm`MyRealm`的认证方法`doGetAuthenticationInfo(token)`
>
> （6）在`MyRealm`的认证方法内，根据`用户传入的用户名`调用`Service`的方法查询数据库中的`用户对象`
>
> （7）在`MyRealm`的认证方法内，创建`SimpleAuthenticationInfo`对象，传入`数据库中的用户名、密码、realm名`并返回该用户认证信息对象。

```java
package com.cyw.test02.pojo;

import com.cyw.test02.service.UserService;
import org.apache.shiro.authc.*;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;
import org.springframework.beans.factory.annotation.Autowired;

public class MyRealm extends AuthorizingRealm {
    
    @Autowired
    private UserService userService;

    // 2. 授权
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        return null;
    }

    // 1. 认证
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
        String principal = (String)authenticationToken.getPrincipal();

        User userByUserName = userService.getUserByUserName(principal);

        if (userByUserName == null){
            throw  new UnknownAccountException();
        }

        String usernameFromDB = userByUserName.getUsername();
        String pwdFromDB = userByUserName.getPwd();

        SimpleAuthenticationInfo info = new SimpleAuthenticationInfo(usernameFromDB, pwdFromDB, this.getName());
        return info;
    }
}

```









### 3.2、注册、配置 Shiro



`com.cyw.test02.config.ShiroConfig`

```java
package com.cyw.test02.config;

import com.cyw.test02.pojo.MyRealm;
import org.apache.shiro.spring.web.ShiroFilterFactoryBean;
import org.apache.shiro.web.mgt.DefaultWebSecurityManager;
import org.apache.shiro.web.servlet.ShiroFilter;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.util.LinkedHashMap;

@Configuration
public class ShiroConfig {

    // 注册 自定义的Realm
    @Bean
    public MyRealm myRealm() {
        return new MyRealm();
    }

    // 注册 SecurityManager
    @Bean
    public DefaultWebSecurityManager defaultWebSecurityManager(MyRealm myRealm) {
        DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager();
        securityManager.setRealm(myRealm);
        return securityManager;
    }

    // 注册 shiroFilterFactory
    @Bean
    public ShiroFilterFactoryBean shiroFilterFactoryBean(DefaultWebSecurityManager defaultWebSecurityManager) {
        ShiroFilterFactoryBean filterFactoryBean = new ShiroFilterFactoryBean();
        filterFactoryBean.setSecurityManager(defaultWebSecurityManager);
        
        // 设置登录页、未授权页、登录成功页
        filterFactoryBean.setLoginUrl("/");
        filterFactoryBean.setSuccessUrl("/main");
        filterFactoryBean.setUnauthorizedUrl("/403");

        // url 权限的设置（有序），这里的anon、authc是Shiro提供的权限别名
        LinkedHashMap<String, String> map = new LinkedHashMap<>();
        map.put("/", "anon");
        map.put("/user/login", "anon");
        map.put("/403", "anon");
        map.put("/**", "authc");

        filterFactoryBean.setFilterChainDefinitionMap(map);

        return filterFactoryBean;
    }

}

```



