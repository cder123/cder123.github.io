

<h1>RBAC-权限控制</h1>

[toc]



# 零、参考资料

- [阮一峰-JWT](https://ruanyifeng.com/blog/2018/07/json_web_token-tutorial.html)
- [JWT介绍以及java-jwt的使用CSDN博客](https://blog.csdn.net/oscar999/article/details/102728303)

- RBAC数据表设计图：[RBAC用户、角色、权限、组设计方案 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/63769951)

- [springboot整合springsecurity最完整，只看这一篇就够了 - QianTM - 博客园 (cnblogs.com)](https://www.cnblogs.com/qiantao/p/14605154.html)

- [Spring Security | 中文文档 (gitcode.net)](https://docs.gitcode.net/spring/guide/spring-security/)
- [【JavaEE】SpringSecurity—— 三更草堂_小何学长的博客-CSDN博客](https://blog.csdn.net/HXBest/article/details/106433454)

- [【全网最细致】SpringBoot整合Spring Security + JWT实现用户认证_bsegebr的博客-CSDN博客_springboot整合security+jwt](https://blog.csdn.net/bsegebr/article/details/125348334)
- [SpringBoot2.7 WebSecurityConfigurerAdapter类过期如何配置 - 开发技术 - 亿速云 (yisu.com)](https://www.yisu.com/zixun/720908.html)

- [别再用过时的方式了！全新版本Spring Security，这样用才够优雅！ - SegmentFault 思否](https://segmentfault.com/a/1190000041947192)

- [Spring Security 5.7 最新配置细节（直接就能用），WebSecurityConfigurerAdapter 已废弃_江帅帅的博客-CSDN博客](https://blog.csdn.net/qq_41340258/article/details/125138902)

- [Spring Security 新版本配置_（结合三更草堂的代码）](https://blog.csdn.net/weixin_46073538/article/details/125547484)

- [别再用过时的方式了！全新版本Spring Security，这样用才够优雅！_倾听铃的声的博客-CSDN博客](https://blog.csdn.net/m0_67698950/article/details/125376716)



视频教程（三更草堂）：

- https://www.bilibili.com/video/BV1mm4y1X7Hc?p=3&share_source=copy_web&vd_source=d79d78365612e7ba4c6de5381643b6cf

<iframe height="550px" src="//player.bilibili.com/player.html?aid=677478312&bvid=BV1mm4y1X7Hc&cid=464430136&page=3" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>





尚硅谷：

https://www.bilibili.com/video/BV15a411A7kP?p=2

<iframe height="550px" src="//player.bilibili.com/player.html?aid=202502517&bvid=BV15a411A7kP&cid=254922998&page=21" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>



# 一、RBAC 权限控制模型



## 1.1、RBAC 权限控制的概念

权限控制的**核心**：`用户`通过`角色`来跟`权限`关联。

由此核心可以得到一个模型：`RBAC`（基于角色的访问控制）。

一个用户 ——-》多个角色 ———-》多个权限



<img src="https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220715184241163.png"/>

<center>RBAC 各个模型的关系</center>



## 1.2、RBAC0



`RBAC0`是最基本的权限控制模型，后面的几种都是建立在`RBAC0`的基础上。



数据表的创建模型：

<img src="https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220715185503107.png"/>





## 1.3、RBAC1



在`RBAC0`的基础上，增加了`角色继承`，最终的权限：（自己的权限+继承过来的权限）

例如：VIP会员继承了普通会员的权限，并新增了自己的新权限。





<img src="https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220715190457204.png"/>





<img src="https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220715190650587.png" />





## 1.4、RBAC2



在`RBAC0`的基础上，增加了`角色责任分离`，



<h4>角色责任分离：</h4>

> **静态**角色责任分离 + **动态**角色责任分离。
>
> 
>
> **静态**角色责任分离：**分配**完角色，马上生效。
>
> - 约束1：有**互斥**的角色时，只能拥有互斥角色中的一个角色。
> - 约束2：一个角色所具有的权限和用户都是**有限的**，一个用户具有的角色也是有限的。
> - 约束3：**先决**角色条件，即拥有某个角色前，必须先有另一个角色。
> - 
>
> 
>
> **动态**角色责任分离：用户登录后，才生效
>
> - 约束1：具有多个角色时，根据场景激活角色。



下图中：基本（用户、角色、权限、用户组）+资源（菜单、操作、页面元素、文件）+ 中间表

<img src="https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220715190835669.png" />







## 1.5、RBAC3



  RBAC3，它是RBAC1与RBAC2合集，所以RBAC3是既有角色分层又有约束的一种模型









# 二、SpringSecurity 框架（单体场景）





<iframe height="550px" src="//player.bilibili.com/player.html?aid=95017741&bvid=BV1bE411T7oZ&cid=162426562&page=196" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>





## 1.1、SpringSecurity 中的概念



### 1.1.1 Principal 主体



Principal 主体：登录系统来使用的人。



### 1.1.2 Authentication 认证



Authentication 认证：证明自己是谁。（相当于登录功能）

```java
// 在非Security组件的地方访问用户数据AuthenticationToken就是用户数据（UserDetails的对象）
AuthenticationToken SecurityContextHolder.getContext().getAuthentication();
```





### 1.1.2 Authorization 授权



Authorization 授权：分配权限





## 1.2、SpringBoot 整合 SpringSecurity



整合案例的目标：

> - 浏览器地址栏无论输入什么地址，都会跳转到登录页（/login.html  |   /login）
> - 登录页输入用户名和密码，正确后跳转到首页（/index.html）
> - 有一个测试接口`/user/info`，只有`admin 或 HD`角色的用户才能访问，其他用户访问后只能跳转到`403错误页`。



SQL：

```sql
SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for tb_auth
-- ----------------------------
DROP TABLE IF EXISTS `tb_auth`;
CREATE TABLE `tb_auth`  (
  `id` int NOT NULL,
  `auth_name` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NULL DEFAULT NULL,
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of tb_auth
-- ----------------------------
INSERT INTO `tb_auth` VALUES (1, 'user:add');
INSERT INTO `tb_auth` VALUES (2, 'user:del');
INSERT INTO `tb_auth` VALUES (3, 'user:update');
INSERT INTO `tb_auth` VALUES (4, 'user:get');

SET FOREIGN_KEY_CHECKS = 1;



SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for tb_menu
-- ----------------------------
DROP TABLE IF EXISTS `tb_menu`;
CREATE TABLE `tb_menu`  (
  `id` int NOT NULL,
  `pid` int NULL DEFAULT NULL,
  `level` tinyint NULL DEFAULT NULL,
  `menu_name` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NULL DEFAULT NULL,
  `url` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NULL DEFAULT NULL,
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of tb_menu
-- ----------------------------
INSERT INTO `tb_menu` VALUES (1, 0, 1, '用户管理', NULL);
INSERT INTO `tb_menu` VALUES (2, 1, 2, '个人资料', NULL);
INSERT INTO `tb_menu` VALUES (3, 1, 2, '全局设置', NULL);
INSERT INTO `tb_menu` VALUES (4, 0, 1, '权限管理', NULL);
INSERT INTO `tb_menu` VALUES (5, 0, 1, '职位管理', NULL);

SET FOREIGN_KEY_CHECKS = 1;




SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for tb_role
-- ----------------------------
DROP TABLE IF EXISTS `tb_role`;
CREATE TABLE `tb_role`  (
  `id` int NOT NULL,
  `role_name` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NULL DEFAULT NULL,
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of tb_role
-- ----------------------------
INSERT INTO `tb_role` VALUES (1, 'admin');
INSERT INTO `tb_role` VALUES (2, 'HRD');
INSERT INTO `tb_role` VALUES (3, 'HR');
INSERT INTO `tb_role` VALUES (4, 'normal');

SET FOREIGN_KEY_CHECKS = 1;




SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for tb_role_menu
-- ----------------------------
DROP TABLE IF EXISTS `tb_role_menu`;
CREATE TABLE `tb_role_menu`  (
  `rid` int NOT NULL,
  `mid` int NOT NULL,
  PRIMARY KEY (`rid`, `mid`) USING BTREE,
  INDEX `fk_mid`(`mid`) USING BTREE,
  CONSTRAINT `fk_mid` FOREIGN KEY (`mid`) REFERENCES `tb_menu` (`id`) ON DELETE RESTRICT ON UPDATE RESTRICT,
  CONSTRAINT `fk_rid` FOREIGN KEY (`rid`) REFERENCES `tb_role` (`id`) ON DELETE RESTRICT ON UPDATE RESTRICT
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of tb_role_menu
-- ----------------------------
INSERT INTO `tb_role_menu` VALUES (1, 1);
INSERT INTO `tb_role_menu` VALUES (2, 1);
INSERT INTO `tb_role_menu` VALUES (3, 1);
INSERT INTO `tb_role_menu` VALUES (1, 2);
INSERT INTO `tb_role_menu` VALUES (2, 2);
INSERT INTO `tb_role_menu` VALUES (3, 2);
INSERT INTO `tb_role_menu` VALUES (1, 3);
INSERT INTO `tb_role_menu` VALUES (2, 3);
INSERT INTO `tb_role_menu` VALUES (3, 3);
INSERT INTO `tb_role_menu` VALUES (1, 4);
INSERT INTO `tb_role_menu` VALUES (2, 4);
INSERT INTO `tb_role_menu` VALUES (1, 5);

SET FOREIGN_KEY_CHECKS = 1;




SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for tb_user
-- ----------------------------
DROP TABLE IF EXISTS `tb_user`;
CREATE TABLE `tb_user`  (
  `id` int NOT NULL,
  `user_name` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NULL DEFAULT NULL,
  `pwd` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NULL DEFAULT NULL,
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of tb_user
-- ----------------------------
INSERT INTO `tb_user` VALUES (1, '张三', '$2a$10$jmBt9.QgxNECuqr7XcKVt.AOJ19HIL03nvG2byVRLBtEVhvDFx23u');
INSERT INTO `tb_user` VALUES (2, '李四', '$2a$10$Ye3W1FTa9GAQVulFntQj.esYcXhQEjZtLGivj5XuEqcosJUQz1jM.');
INSERT INTO `tb_user` VALUES (3, '王五', '$2a$10$k3WQcjeLfjqUYt1dXlQsvuML1B6pYH62oyqXPYGVwtq138JbqdSHG');

SET FOREIGN_KEY_CHECKS = 1;




SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for tb_user_role
-- ----------------------------
DROP TABLE IF EXISTS `tb_user_role`;
CREATE TABLE `tb_user_role`  (
  `uid` int NOT NULL,
  `rid` int NOT NULL,
  PRIMARY KEY (`uid`, `rid`) USING BTREE,
  INDEX `dk_rid`(`rid`) USING BTREE,
  CONSTRAINT `dk_rid` FOREIGN KEY (`rid`) REFERENCES `tb_role` (`id`) ON DELETE RESTRICT ON UPDATE RESTRICT,
  CONSTRAINT `fk_uid` FOREIGN KEY (`uid`) REFERENCES `tb_user` (`id`) ON DELETE RESTRICT ON UPDATE RESTRICT
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of tb_user_role
-- ----------------------------
INSERT INTO `tb_user_role` VALUES (1, 1);
INSERT INTO `tb_user_role` VALUES (2, 2);
INSERT INTO `tb_user_role` VALUES (3, 3);
INSERT INTO `tb_user_role` VALUES (3, 4);

SET FOREIGN_KEY_CHECKS = 1;

```









### 1.2.1、整合

（1）POM.xml中导入依赖：

```xml
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-security</artifactId>
		</dependency>
```



（2）在`resources/static`目录下创建`login.html` 和 `index.html`：

（可以在WebSecurityConfig的securityFilterChain方法中，通过loginPage来配置）

`resources/static/login.html`:

```html
<!DOCTYPE html>
<html lang="zh-cn">
<head>
    <meta charset="UTF-8">
    <title>登录</title>
</head>
<body>
    <center><h2>登录</h2></center>
    <center>
        <form method="post" action="/login">
            <input type="text" name="username" placeholder="用户名"> <br/>
            <input type="text" name="pwd" placeholder="密码"> <br/>
            <input type="submit" value="登录"> <br/>
        </form>
    </center>

</body>
</html>
```





`resources/static/index.html`:

```html
<!DOCTYPE html>
<html lang="zh-cn">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
</head>
<body>

    <h1>首页</h1>
    <h3>欢迎回来！</h3>

<script>
</script>
    
</body>
</html>
```



（3）编写 数据库查询的POJO、Mapper：

`MyRst`-实体类：

```java
package com.cyw.rbac01.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class MyRst {
    private Integer code;
    private String msg;
    private Object data;


    public static MyRst ok(String msg){
        return new MyRst(2000,msg,null);
    }

    public static MyRst ok(String msg,Object data){
        return new MyRst(2000,msg,data);
    }

    public static MyRst error(String msg){
        return new MyRst(4000,msg,null);
    }
}
```



`MyUser`-实体类：

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class MyUser {
    private Integer id;
    private String username;
    private String pwd;
    private List<Role> roleList;
}
```



`Role`-实体类：

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Role {
    private Integer id;
    private String roleName;
    private List<Menu> menuList;
}
```



`UserMapper`：

```java
@Mapper
public interface UserMapper {
    User getUserByUserName(String username);
}
```



`UserMapper.xml`：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.cyw.rbac01.mapper.UserMapper">

    <resultMap id="rstMap" type="com.cyw.rbac01.pojo.User">
        <id property="id" column="uid"/>
        <result property="username" column="user_name"/>
        <result property="pwd" column="pwd"/>
        <collection property="roleList" ofType="com.cyw.rbac01.pojo.Role">
            <id property="id" column="rid"/>
            <result property="roleName" column="role_name"/>
        </collection>
    </resultMap>

    <select id="getUserByUserName" resultMap="rstMap">
        select
            *
        from tb_user,tb_role,tb_user_role
        where tb_user.id = tb_user_role.uid
          and tb_role.id = tb_user_role.rid
        and tb_user.user_name = #{usrename}
    </select>


</mapper>
```



（4）编写SpringSecurity的`UserDetailsService`接口的实现类：

该实现类由SpringSecurity **自动管理** ，无需用户调用。

```java
package com.cyw.rbac01.service.impl;

import com.cyw.rbac01.mapper.MenuMapper;
import com.cyw.rbac01.mapper.UserMapper;
import com.cyw.rbac01.pojo.Menu;
import com.cyw.rbac01.pojo.Role;
import com.cyw.rbac01.pojo.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;

import java.util.ArrayList;
import java.util.List;

@Service("UserDetailsService")
public class UserDetailsServiceImpl implements UserDetailsService {
    
    @Autowired
    private UserMapper userMapper;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        // 1、从数据库中根据用户名查询用户数据（尤其是权限数据）
        User userFromDB = userMapper.getUserByUserName(username);
        if (userFromDB==null){
            throw new UsernameNotFoundException("无此用户！");
        }
		
        // 封装权限列表（此处封装了角色列表）
        List<Role> roleList = userFromDB.getRoleList();
        List<GrantedAuthority> permList = new ArrayList<GrantedAuthority>();

        for (Role role : roleList) {
            permList.add(new SimpleGrantedAuthority(role.getRoleName()));
        }
        
        // 将数据库中的用户数据封装为Security框架的User类中，让框架知道用户正确的密码和权限
        org.springframework.security.core.userdetails.User userDetails = new org.springframework.security.core.userdetails.User(username,userFromDB.getPwd(),permList);
        return userDetails;
    }
}
```



Security的配置类

```java
package com.cyw.rbac01.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.config.annotation.authentication.configuration.AuthenticationConfiguration;
import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
@EnableWebSecurity
@EnableGlobalMethodSecurity(prePostEnabled = true, securedEnabled = true)
public class WebSecurityConfig {
    @Autowired
    private UserDetailsService userDetailService;

    @Bean
    public BCryptPasswordEncoder bCryptPasswordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Bean
    public AuthenticationManager authenticationManager(AuthenticationConfiguration authenticationConfiguration) throws Exception {
        return authenticationConfiguration.getAuthenticationManager();
    }

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        return http.userDetailsService(userDetailService)
                .formLogin()						// 开始登录功能的设置
                .loginPage("/login.html")
                .usernameParameter("username")
                .passwordParameter("pwd")
                .loginProcessingUrl("/login")		// 注意：要与登录表单的action地址一致
                .defaultSuccessUrl("/index.html")	// 登录成功后要跳转到的页面
                .permitAll()
                .and()
                .logout()							// 开始退出登录的设置
                .logoutUrl("/logout")
                .logoutSuccessUrl("/login.html")
                .permitAll()
                .and()
                .authorizeRequests()				// 限制所有请求
                .antMatchers("/**")
                .authenticated()
                .and()
                .exceptionHandling()				// 未授权页面的设置
                .accessDeniedPage("/4xx.html")
                .and()
                .csrf()								// 关闭跨域
                .disable()
                .build();

    }
}
```









### 1.2.1、配置



（1）创建 SpringSecurity 的配置类（`com.cyw.demo.config.WebSecurityConfig`）：

新版：

```java
package com.cyw.demo.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.*;
import org.springframework.security.config.annotation.authentication.configuration.AuthenticationConfiguration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.core.userdetails.UserDetailsService;

@EnableWebSecurity
@Configuration
//@EnableGlobalMethodSecurity(prePostEnabled = true, securedEnabled = true)
public class SecurityConfig {

    // 创建自己覆盖重写SpringSecurity中UserDetailsService接口的实现类
    @Autowired
    private UserDetailsService userDetailServiceImpl;

    // 创建SpringSecurity推荐使用的加密器
    @Bean
    public BCryptPasswordEncoder bCryptPasswordEncoder() {
        return new BCryptPasswordEncoder();
    }

    // 创建AuthenticationManager
    @Bean
    public AuthenticationManager authenticationManager(AuthenticationConfiguration authenticationConfiguration) throws Exception {
        return authenticationConfiguration.getAuthenticationManager();
    }

    // 创建过滤器链（核心配置）
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        return
                http.userDetailsService(userDetailServiceImpl)
                        .formLogin()
                        .loginPage("/index.html")
                        .usernameParameter("userName")
                        .loginProcessingUrl("/user/login")
                        .defaultSuccessUrl("/main.html")
                        .permitAll()
                        .and()
                        .authorizeHttpRequests()
                        .antMatchers("/","/user/login")
                        .permitAll()
                        .anyRequest()
                        .authenticated()
                        .and()
                        .csrf()
                        .disable()
                        .build();
    }
}

```







旧版：

```java
// (1) 作为配置类
@Configuration
// (2) 开启 Spring Security 的功能
@EnableWebSecurity
// (3) 是否可用@PreAuthorize,@PostAuthorize等注解,设置为true,会拦截加了这些注解的接口
@EnableGlobalMethodSecurity(prePostEnabled=true)
public class WebSecurityConfig {
    
    @Bean
    public PasswordEncoder passwordEncoder(){
        // 使用BCrypt加密密码
        return new BCryptPasswordEncoder();
    }
    
    @Override
    public void configure(WebSecurity web) throws Exception {
        // 放行静态资源
        web.ignoring().antMatchers("/static/**");
    }
  
//  模拟数据库中的数据（手动控制认证）   
//    @Override
//    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
//        auth.inMemoryAuthentication()
//                .withUser("zs")//用户名
//                .password(passwordEncoder().encode("123"))//密码
//                .roles("admin");//角色
//        auth.inMemoryAuthentication()
//                .withUser("ls")//用户名
//                .password(passwordEncoder().encode("456"))//密码
//                .roles("admin");//角色
//
//    }
    
        @Resource
    private AuthenticationSuccessHandler loginSuccessHandler; //认证成功结果处理器
    @Resource
    private AuthenticationFailureHandler loginFailureHandler; //认证失败结果处理器

    //http请求拦截配置
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        //开启运行iframe嵌套页面
        http.headers().frameOptions().disable();

        http//1、配置权限认证
                .authorizeRequests()
                //配置不拦截路由
                .antMatchers("/500").permitAll()
                .antMatchers("/403").permitAll()
                .antMatchers("/404").permitAll()
                .antMatchers("/login").permitAll()
                .anyRequest() //任何其它请求
                .authenticated() //都需要身份认证
                .and()
                //2、登录配置表单认证方式
                .formLogin()
                .loginPage("/login")//自定义登录页面的url
                .usernameParameter("username")//设置登录账号参数，与表单参数一致
                .passwordParameter("password")//设置登录密码参数，与表单参数一致
                // 告诉Spring Security在发送指定路径时处理提交的凭证，默认情况下，将用户重定向回用户来自的页面。登录表单form中action的地址，也就是处理认证请求的路径，
                // 只要保持表单中action和HttpSecurity里配置的loginProcessingUrl一致就可以了，也不用自己去处理，它不会将请求传递给Spring MVC和您的控制器，所以我们就不需要自己再去写一个/user/login的控制器接口了
                .loginProcessingUrl("/user/login")//配置默认登录入口
                .defaultSuccessUrl("/index")//登录成功后默认的跳转页面路径
                .failureUrl("/login?error=true")
                .successHandler(loginSuccessHandler)//使用自定义的成功结果处理器
                .failureHandler(loginFailureHandler)//使用自定义失败的结果处理器
                .and()
                //3、注销
                .logout()
                .logoutUrl("/logout")
                .logoutSuccessHandler(new CustomLogoutSuccessHandler()) // 自己写处理器
                .permitAll()
                .and()
                //4、session管理
                .sessionManagement()
                .invalidSessionUrl("/login") //失效后跳转到登陆页面
                //单用户登录，如果有一个登录了，同一个用户在其他地方登录将前一个剔除下线
                //.maximumSessions(1).expiredSessionStrategy(expiredSessionStrategy())
                //单用户登录，如果有一个登录了，同一个用户在其他地方不能登录
                //.maximumSessions(1).maxSessionsPreventsLogin(true) ;
                .and()
                //5、禁用跨站csrf攻击防御
                .csrf()
                .disable();
    }
    
}
```









（2）创建获取用户信息的接口及其实现（service、mapper）：

pojo：

```java
package com.cyw.demo.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class UserInfo {
    private Integer id;
    private String username;
    private String pwd;
    private String role;
}

```

mapper:

```java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.cyw.demo.mapper.UserMapper">
    <resultMap id="rstMap" type="com.cyw.demo.pojo.UserInfo">
        <id property="id" column="id"/>
        <result property="username" column="username"/>
        <result property="pwd" column="pwd"/>
        <result property="role" column="role_name"/>
    </resultMap>
    <select id="getUserByUserName" resultMap="rstMap">
        SELECT tb_user.id,username,pwd,role_name  
        FROM tb_user,tb_user_role,tb_role      
        WHERE
          tb_user.id = tb_user_role.uid
         and tb_role.id = tb_user_role.role_id
         and  username = #{username}
    </select>
</mapper>
```

service：

```java
package com.cyw.demo.service.impl;

import com.cyw.demo.mapper.UserMapper;
import com.cyw.demo.pojo.UserInfo;
import com.cyw.demo.service.UserInfoService;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;
import javax.annotation.Resource;

@Service("userInfoService")
@Transactional
public class UserInfoServiceImpl implements UserInfoService {

    @Resource
    private UserMapper userMapper;

    @Override
    public UserInfo getUserByUserName(String userName) {
        return userMapper.getUserByUserName(userName);
    }
}

```



（2）配置认证：

```java
package com.cyw.demo.config;

import com.cyw.demo.pojo.UserInfo;
import com.cyw.demo.service.UserInfoService;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.stereotype.Component;
import javax.annotation.Resource;
import java.util.ArrayList;
import java.util.List;

@Component
// UserDetailsService类 是SpringSecurity的接口
public class CustomUserDetailsService implements UserDetailsService {
    // 自己写的用户查询接口
    @Resource
    private UserInfoService userInfoService;

    // PasswordEncoder是在WebSecurityConfig配置类中注册的类
    @Resource
    private PasswordEncoder passwordEncoder;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        // 调用自己的查询接口(查到数据库中的用户数据)
        UserInfo userFromDb = userInfoService.getUserByUserName(username);
        if(userFromDb == null) {
            throw new UsernameNotFoundException("无此用户！！");
        }

        //定义权限列表.
        List<GrantedAuthority> authorities = new ArrayList<>();

        // 获取用户所拥有的权限 注意：必须最终以"ROLE_"开头
        authorities.add(new SimpleGrantedAuthority("ROLE_"+ userFromDb.getRole()));

        // User类是Spring Security 自带的类,userFromDb是自己定义的类
        User user = new User(userFromDb.getUsername(),passwordEncoder.encode(userFromDb.getPwd()),authorities);
        return user;
    }
}

```



（3）放行静态资源：

```java
@Configuration
public class WebMvcConfig extends WebMvcConfigurationSupport {
    /**
     * 配置静态资源
     * @param registry
     */
    @Override
    protected void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/static/**").addResourceLocations("classpath:/static/");
        super.addResourceHandlers(registry);
    }
}
```



（4）错误页的配置：

```java
/**
 * @Author qt
 * @Date 2021/3/25
 * @Description 配置错误页面 403 404 500  适用于 SpringBoot 2.x
 */
@Configuration
public class ErrorPageConfig {

    @Bean
    public WebServerFactoryCustomizer<ConfigurableWebServerFactory> webServerFactoryCustomizer() {
        WebServerFactoryCustomizer<ConfigurableWebServerFactory> webCustomizer = new WebServerFactoryCustomizer<ConfigurableWebServerFactory>() {
            @Override
            public void customize(ConfigurableWebServerFactory factory) {
                ErrorPage[] errorPages = new ErrorPage[] {
                        new ErrorPage(HttpStatus.FORBIDDEN, "/403"),
                        new ErrorPage(HttpStatus.NOT_FOUND, "/404"),
                        new ErrorPage(HttpStatus.INTERNAL_SERVER_ERROR, "/500"),
                };
                factory.addErrorPages(errorPages);
            }
        };
        return webCustomizer;
    }
}
```



（5）自定义权限决策器：

```java
/**
 * @Author qt
 * @Date 2021/3/31
 * @Description 自定义权限决策管理器
 */
@Component
public class CustomAccessDecisionManager implements AccessDecisionManager {

    /**
     * @Author: qt
     * @Description: 取当前用户的权限与这次请求的这个url需要的权限作对比，决定是否放行
     * auth 包含了当前的用户信息，包括拥有的权限,即之前UserDetailsService登录时候存储的用户对象
     * object 就是FilterInvocation对象，可以得到request等web资源。
     * configAttributes 是本次访问需要的权限。即上一步的 MyFilterInvocationSecurityMetadataSource 中查询核对得到的权限列表
     **/
    @Override
    public void decide(Authentication auth, Object o, Collection<ConfigAttribute> collection) throws AccessDeniedException, InsufficientAuthenticationException {
        Iterator<ConfigAttribute> iterator = collection.iterator();
        while (iterator.hasNext()) {
            if (auth == null) {
                throw new AccessDeniedException("当前访问没有权限");
            }
            ConfigAttribute ca = iterator.next();
            //当前请求需要的权限
            String needRole = ca.getAttribute();
            if ("ROLE_LOGIN".equals(needRole)) {
                if (auth instanceof AnonymousAuthenticationToken) {
                    throw new BadCredentialsException("未登录");
                } else
                    return;
            }
            //当前用户所具有的权限
            Collection<? extends GrantedAuthority> authorities = auth.getAuthorities();
            for (GrantedAuthority authority : authorities) {
                if (authority.getAuthority().equals(needRole)) {
                    return;
                }
            }
        }
        throw new AccessDeniedException("权限不足!");
    }

    @Override
    public boolean supports(ConfigAttribute configAttribute) {
        return true;
    }

    @Override
    public boolean supports(Class<?> aClass) {
        return true;
    }
}
```



（6）注销处理器：

```java
/**
 * @Author qt
 * @Date 2021/3/31
 * @Description 注销登录处理
 */
public class CustomLogoutSuccessHandler implements LogoutSuccessHandler {
    private Logger logger = LoggerFactory.getLogger(getClass());

    @Override
    public void onLogoutSuccess(HttpServletRequest httpServletRequest, HttpServletResponse response, Authentication authentication) throws IOException, ServletException {
        System.out.println("注销成功!");
        //这里写你登录成功后的逻辑
        response.setStatus(HttpStatus.OK.value());
        response.setContentType("application/json;charset=UTF-8");
        response.getWriter().write("注销成功!");
    }
}
```



（7）登录成功处理器

```java
/** @Author qt  * @Date 2021/3/24
 * @Description 登录成功处理
 */
@Component("loginSuccessHandler")
public class LoginSuccessHandler extends SavedRequestAwareAuthenticationSuccessHandler {
    private Logger logger = LoggerFactory.getLogger(getClass());

    @Resource
    private ObjectMapper objectMapper;

    private RequestCache requestCache;

    @Override
    public void onAuthenticationSuccess(HttpServletRequest request, HttpServletResponse response, Authentication authentication) throws ServletException, IOException {
        // 获取前端传到后端的全部参数
          Enumeration enu = request.getParameterNames();
          while (enu.hasMoreElements()) {
              String paraName = (String) enu.nextElement(); System.out.println("参数- " + paraName + " : " + request.getParameter(paraName));
          }
        logger.info("登录认证成功");
        //这里写你登录成功后的逻辑，可以验证其他信息，如验证码等。
        response.setContentType("application/json;charset=UTF-8");
        JSONObject resultObj = new JSONObject();
        resultObj.put("code", HttpStatus.OK.value());
        resultObj.put("msg","登录成功");
        resultObj.put("authentication",objectMapper.writeValueAsString(authentication));
        response.getWriter().write(resultObj.toString());
        this.getRedirectStrategy().sendRedirect(request, response, "/index");//重定向
    }
}
```





（8）登录失败处理器

```java
/**
 * @Author qt
 * @Date 2021/3/24
 * @Description 登录失败处理
 */
@Component("loginFailureHandler")
public class LoginFailureHandler extends SimpleUrlAuthenticationFailureHandler {
    private Logger logger = LoggerFactory.getLogger(getClass());
    @Resource
    private ObjectMapper objectMapper;

    @Override
    public void onAuthenticationFailure(HttpServletRequest request, HttpServletResponse response, AuthenticationException exception) throws IOException, ServletException {
        logger.info("登录失败");
        this.saveException(request, exception);
        this.getRedirectStrategy().sendRedirect(request, response, "/login?error=true");
    }
}
```



（9）jwt 工具类:

```java
package com.cyw.demo.util;

import io.jsonwebtoken.Claims;
import io.jsonwebtoken.JwtBuilder;
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;

import javax.crypto.SecretKey;
import javax.crypto.spec.SecretKeySpec;
import java.util.Base64;
import java.util.Date;
import java.util.UUID;

/**
 * JWT工具类
 */
public class JwtUtil {

    //有效期为
    public static final Long JWT_TTL = 60 * 60 *1000L;// 60 * 60 *1000  一个小时
    //设置秘钥明文
    public static final String JWT_KEY = "sangeng";

    public static String getUUID(){
        String token = UUID.randomUUID().toString().replaceAll("-", "");
        return token;
    }

    /**
     * 生成jwt
     * @param subject token中要存放的数据（json格式）
     * @return
     */
    public static String createJWT(String subject) {
        JwtBuilder builder = getJwtBuilder(subject, null, getUUID());// 设置过期时间
        return builder.compact();
    }

    /**
     * 生成jwt
     * @param subject token中要存放的数据（json格式）
     * @param ttlMillis token超时时间
     * @return
     */
    public static String createJWT(String subject, Long ttlMillis) {
        JwtBuilder builder = getJwtBuilder(subject, ttlMillis, getUUID());// 设置过期时间
        return builder.compact();
    }

    private static JwtBuilder getJwtBuilder(String subject, Long ttlMillis, String uuid) {
        SignatureAlgorithm signatureAlgorithm = SignatureAlgorithm.HS256;
        SecretKey secretKey = generalKey();
        long nowMillis = System.currentTimeMillis();
        Date now = new Date(nowMillis);
        if(ttlMillis==null){
            ttlMillis=JwtUtil.JWT_TTL;
        }
        long expMillis = nowMillis + ttlMillis;
        Date expDate = new Date(expMillis);
        return Jwts.builder()
                .setId(uuid)              //唯一的ID
                .setSubject(subject)   // 主题  可以是JSON数据
                .setIssuer("sg")     // 签发者
                .setIssuedAt(now)      // 签发时间
                .signWith(signatureAlgorithm, secretKey) //使用HS256对称加密算法签名, 第二个参数为秘钥
                .setExpiration(expDate);
    }

    /**
     * 创建token
     * @param id
     * @param subject
     * @param ttlMillis
     * @return
     */
    public static String createJWT(String id, String subject, Long ttlMillis) {
        JwtBuilder builder = getJwtBuilder(subject, ttlMillis, id);// 设置过期时间
        return builder.compact();
    }

    public static void main(String[] args) throws Exception {
        String token = "eyJhbGciOiJIUzI1NiJ9.eyJqdGkiOiJjYWM2ZDVhZi1mNjVlLTQ0MDAtYjcxMi0zYWEwOGIyOTIwYjQiLCJzdWIiOiJzZyIsImlzcyI6InNnIiwiaWF0IjoxNjM4MTA2NzEyLCJleHAiOjE2MzgxMTAzMTJ9.JVsSbkP94wuczb4QryQbAke3ysBDIL5ou8fWsbt_ebg";
        Claims claims = parseJWT(token);
        System.out.println(claims);
    }

    /**
     * 生成加密后的秘钥 secretKey
     * @return
     */
    public static SecretKey generalKey() {
        byte[] encodedKey = Base64.getDecoder().decode(JwtUtil.JWT_KEY);
        SecretKey key = new SecretKeySpec(encodedKey, 0, encodedKey.length, "AES");
        return key;
    }

    /**
     * 解析
     *
     * @param jwt
     * @return
     * @throws Exception
     */
    public static Claims parseJWT(String jwt) throws Exception {
        SecretKey secretKey = generalKey();
        return Jwts.parser()
                .setSigningKey(secretKey)
                .parseClaimsJws(jwt)
                .getBody();
    }


}

```







## 1.3、登录校验流程分析





<img width="1000px" src="https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220717150903738.png" />





## 1.4、SpringSecurity 原理初探



SpringSecurity 的原理是利用了`过滤器链`



<img src="https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220717151704386.png" />







我们写代码的执行流程：

<img width="1000px"  src="https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220717153251391.png" />



由上图可以看出，实际上需要做的步骤：

- （1）自定义登录接口（可以考虑登录成功后的 JWT 存入Redis中）
- （2）自定义查询数据库的方法
- （3）自定义 JWT 认证过滤器（获取token、解析token、查询用户信息、存入security context holder对象中）







## 1.5、登录认证-完整案例



（1）POM.xml中导入依赖：

```xml
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-security</artifactId>
		</dependency>
```



（2）在`resources/static`目录下创建`index.html`和`main.html`：

在正常不用 SpringSecurity 的时候，可以直接访问该首页，配置SpringSecurity 后，需要登录（可以在WebSecurityConfig的securityFilterChain方法中，通过loginPage来配置）

`index.html`:

```html
<!DOCTYPE html>
<html lang="zh-cn">
<head>
    <meta charset="UTF-8">
    <title>首页</title>
</head>
<body>
    <center>
    <form method="post" action="/user/login">
        <input type="text" name="userName" placeholder="用户名"> <br/>
        <input type="text" name="password" placeholder="密码"> <br/>
        <input type="submit" value="登录"> <br/>
    </form>
</center>

</body>
</html>
```

`main.html`:

```html
<!DOCTYPE html>
<html lang="zh-cn">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
	<h2>主页：登陆成功！</h2>
</body>
</html>
```





（3）创建 SpringSecurity 的配置类（`com.cyw.demo.config.WebSecurityConfig`）：

新版（5.7）：

```java
package com.cyw.demo.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.*;
import org.springframework.security.config.annotation.authentication.configuration.AuthenticationConfiguration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.core.userdetails.UserDetailsService;

@EnableWebSecurity
@Configuration
//@EnableGlobalMethodSecurity(prePostEnabled = true, securedEnabled = true)
public class SecurityConfig {

    // 创建自己覆盖重写SpringSecurity中UserDetailsService接口的实现类
    @Autowired
    private UserDetailsService userDetailServiceImpl;

    // 创建SpringSecurity推荐使用的加密器
    @Bean
    public BCryptPasswordEncoder bCryptPasswordEncoder() {
        return new BCryptPasswordEncoder();
    }

    // 创建AuthenticationManager
    @Bean
    public AuthenticationManager authenticationManager(AuthenticationConfiguration authenticationConfiguration) throws Exception {
        return authenticationConfiguration.getAuthenticationManager();
    }

    // 创建过滤器链（核心配置）
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        return
                http.userDetailsService(userDetailServiceImpl)
                        .formLogin()            			
                        .loginPage("/index.html")			// 自定义登录页
                        .usernameParameter("userName")		// 自定义登录表单的用户名的name属性
                        .loginProcessingUrl("/user/login")	// 登录逻辑处理的 controller
                        .defaultSuccessUrl("/main.html")	// 登录成功后跳转到的页面
                        .permitAll()						// 允许以上url通过
                        .and()
                        .authorizeHttpRequests()
                        //.antMatchers("/","/user/login")
                        //.permitAll()
                        .anyRequest()
                        .authenticated()
                        .and()
                        .csrf()
                        .disable()
                        .build();
    }
}

```



（4）POJO：

结果集：

```java
package com.cyw.demo.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class MyResult {
    private Integer code;
    private String msg;
    private Object data;

    public static MyResult ok(String msg){
        return new MyResult(200,null,null);
    }
    public static MyResult ok(String msg,Object data){
        return new MyResult(200,msg,data);
    }
    public static MyResult error(String msg){
        return new MyResult(400,msg,null);
    }
}
```



MyUser （最后需要传入Security的UserDetails接口的实现类User的构造函数中）：

```java
package com.cyw.demo.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import java.io.Serializable;
import java.util.Date;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class MyUser implements Serializable {
    private static final long serialVersionUID = -40356785423868312L;
    private Long id;
    //用户名
    private String userName;
    //昵称    
    private String nickName;
    //密码
    private String password;
    //账号状态（0正常 1停用）
    private String status;
    // 邮箱
    private String email;
    //手机号
    private String phonenumber;
    //用户性别（0男，1女，2未知）
    private String sex;
    // 头像
    private String avatar;
    //用户类型（0管理员，1普通用户）
    private String userType;
    //创建人的用户id
    private Long createBy;
    //创建时间
    private Date createTime;
    //更新人
    private Long updateBy;
    //更新时间
    private Date updateTime;
    // 删除标志（0代表未删除，1代表已删除）
    private Integer delFlag;
}
```





（5）UserMapper：

```java
@Mapper
public interface UserMapper {
    public MyUser getUserByName(String username);
}
```

UserMapper.xml：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.cyw.demo.mapper.UserMapper">
    <select id="getUserByName" resultType="com.cyw.demo.pojo.MyUser">
        select *
        from sys_user
        where user_name = #{username}
    </select>
</mapper>
```



（6）Service接口：

```java
package com.cyw.demo.service.impl;

import com.cyw.demo.mapper.UserMapper;
import com.cyw.demo.pojo.MyUser;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.authority.AuthorityUtils;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;

@Service("userDetailServiceImpl")
// 实现SpringSecurity 的接口
public class UserDetailServiceImpl implements UserDetailsService {
    @Autowired
    private UserMapper userMapper;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        // (1)根据用户名从数据库中查询用户
        MyUser user = userMapper.getUserByName(username);
        // 如果没有查询到用户
        if (user == null){
            throw new UsernameNotFoundException("用户名错误或者密码错误");
        }
        // User类是SpringSecurity中UserDetails接口的实现类
        // 最后一个参数是权限集合（Collection），此案例中没有用到，只是占一下位置
        return  new User(user.getUserName(),
                user.getPassword(),
                AuthorityUtils.commaSeparatedStringToAuthorityList("admin,nornal"));
    }
}
```



（6）登录 Controller：

```java
package com.cyw.demo.controller;

import com.cyw.demo.pojo.MyResult;
import com.cyw.demo.pojo.MyUser;
import com.cyw.demo.service.LoginService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/user")
public class LoginController {

    @PostMapping("/login")
    public MyResult login(MyUser myUser){      
        return MyResult.ok("控制层！！");
    }

}
```



（7）覆盖重写SpringSecurity的 UserDetailsService 接口：

```java
package com.cyw.demo.service.impl;

import com.cyw.demo.mapper.UserMapper;
import com.cyw.demo.pojo.MyUser;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.authority.AuthorityUtils;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;

@Service("userDetailServiceImpl")
// 实现SpringSecurity 的接口
public class UserDetailServiceImpl implements UserDetailsService {
    @Autowired
    private UserMapper userMapper;


    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        // (1)根据用户名从数据库中查询用户
        MyUser user = userMapper.getUserByName(username);
        System.out.println(user);
        // 如果没有查询到用户
        if (user == null){
            throw new UsernameNotFoundException("用户名错误或者密码错误");
        }
        // MyUserDetails类是SpringSecurity中UserDetails接口的实现类
        return  new User(user.getUserName(),
                user.getPassword(),
                AuthorityUtils.commaSeparatedStringToAuthorityList(""));

    }
}

```







## 1.6、授权-配置类（授权方法）



在 `config/WebSecurityConfig`配置类中，设置权限的四个方法：

> - `hasAuthority()`：拥有指定的**某个**权限，则返回 `true`
> - `hasAnyAuthority()`：拥有指定的**某些**权限，则返回 `true`
> - `hasRole()`：拥有指定的**某个**角色，则返回 `true`
> - `hasAnyRole()`：拥有指定的**某些**角色，则返回 `true`



SpringSecurity 中，角色与权限的用法类似，但区别是，角色（数据库中的字段）必须要有前缀`ROLE_` 。





### 1.6.1、hasAuthority 方法



目标：用户具有`admins`权限时，可以访问`/user/helloAdmin`



在 `config/WebSecurityConfig`配置类中，设置`hasAuthority()`，具有`admins`权限的用户允许访问`/user/helloAdmin`。

```java
package com.cyw.demo.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.*;
import org.springframework.security.config.annotation.authentication.configuration.AuthenticationConfiguration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.core.userdetails.UserDetailsService;

@EnableWebSecurity
@Configuration
public class SecurityConfig {

    @Autowired
    private UserDetailsService userDetailServiceImpl;

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        return
                http.userDetailsService(userDetailServiceImpl)
                        .formLogin()
                        .loginPage("/index.html")
                        .usernameParameter("userName")
                        .loginProcessingUrl("/user/login")
                        .defaultSuccessUrl("/main.html")
                        .permitAll()
                        .and()
                        .authorizeHttpRequests()
                        .antMatchers("/user/helloAdmin")	// 设置允许访问的权限（开始）
                        .hasAuthority("admins")				// 设置允许访问的权限（结束）
                        .anyRequest()
                        .authenticated()
                        .and()
                        .csrf()
                        .disable()
                        .build();
    }
    
    @Bean
    public BCryptPasswordEncoder bCryptPasswordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Bean
    public AuthenticationManager authenticationManager(AuthenticationConfiguration authenticationConfiguration) throws Exception {
        return authenticationConfiguration.getAuthenticationManager();
    }

}

```



在UserDetailsServiceImpl实现类中设置用户的权限（因为数据表中没有定义权限的字段，所以手动模拟）

```java
package com.cyw.demo.service.impl;

import com.cyw.demo.mapper.UserMapper;
import com.cyw.demo.pojo.MyUser;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.authority.AuthorityUtils;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;

@Service("userDetailServiceImpl")
public class UserDetailServiceImpl implements UserDetailsService {
    @Autowired
    private UserMapper userMapper;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        // (1)根据用户名从数据库中查询用户
        MyUser user = userMapper.getUserByName(username);

        // 如果没有查询到用户
        if (user == null){
            throw new UsernameNotFoundException("用户名错误或者密码错误");
        }
        
        // 模拟数据库中查出的该用户的的权限列表（此处只模拟了该用户只有admins和normal两个权限）
        // 注意：多个权限之间，在字符串中使用逗号分隔（与webSecurityConfig配置类中做区别）
        List permissionList = AuthorityUtils.commaSeparatedStringToAuthorityList("admins,normal")
        
        // User类是SpringSecurity中UserDetails接口的实现类
        User userDetails = new User(
            	user.getUserName(),
                user.getPassword(),
                permissionList
                );
        return  userDetails;
    }
}
```





### 1.6.2、hasAnyAuthority 方法



目标：用户具有`admins，abc`两个权限中的任意一个权限时，可以访问`/user/helloAdmin`



WebSecurityConfig配置类：

```java
package com.cyw.demo.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.*;
import org.springframework.security.config.annotation.authentication.configuration.AuthenticationConfiguration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.core.userdetails.UserDetailsService;

@EnableWebSecurity
@Configuration
//@EnableGlobalMethodSecurity(prePostEnabled = true, securedEnabled = true)
public class SecurityConfig {

    @Autowired
    private UserDetailsService userDetailServiceImpl;

    @Bean
    public BCryptPasswordEncoder bCryptPasswordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Bean
    public AuthenticationManager authenticationManager(AuthenticationConfiguration authenticationConfiguration) throws Exception {
        return authenticationConfiguration.getAuthenticationManager();
    }

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        return
                http.userDetailsService(userDetailServiceImpl)
                        .formLogin()
                        .loginPage("/index.html")
                        .usernameParameter("userName")
                        .loginProcessingUrl("/user/login")
                        .defaultSuccessUrl("/main.html")
                        .permitAll()
                        .and()
                        .authorizeHttpRequests()
                        .antMatchers("/user/helloAdmin")
//                        .hasAuthority("admins")			// 单个权限
                        .hasAnyAuthority("admins","abc")	// 多个权限
                        .anyRequest()
                        .authenticated()
                        .and()
                        .csrf()
                        .disable()
                        .build();
    }
}

```



Service：

```java
package com.cyw.demo.service.impl;

import com.cyw.demo.mapper.UserMapper;
import com.cyw.demo.pojo.MyUser;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.authority.AuthorityUtils;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;

@Service("userDetailServiceImpl")
// 实现SpringSecurity 的接口
public class UserDetailServiceImpl implements UserDetailsService {
    @Autowired
    private UserMapper userMapper;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        // (1)根据用户名从数据库中查询用户
        MyUser user = userMapper.getUserByName(username);
//        System.out.println(user);
        // 如果没有查询到用户
        if (user == null){
            throw new UsernameNotFoundException("用户名错误或者密码错误");
        }
        // User类是SpringSecurity中UserDetails接口的实现类
        User userDetails = new User(user.getUserName(),
                user.getPassword(),
                AuthorityUtils.commaSeparatedStringToAuthorityList("abc"));
        return  userDetails;
    }
}

```





### 1.6.3、hasRole 方法

WebSecurityConfig配置类：

```java
package com.cyw.demo.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.*;
import org.springframework.security.config.annotation.authentication.configuration.AuthenticationConfiguration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.core.userdetails.UserDetailsService;

@EnableWebSecurity
@Configuration
//@EnableGlobalMethodSecurity(prePostEnabled = true, securedEnabled = true)
public class SecurityConfig {

    @Autowired
    private UserDetailsService userDetailServiceImpl;

    @Bean
    public BCryptPasswordEncoder bCryptPasswordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Bean
    public AuthenticationManager authenticationManager(AuthenticationConfiguration authenticationConfiguration) throws Exception {
        return authenticationConfiguration.getAuthenticationManager();
    }

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        return
                http.userDetailsService(userDetailServiceImpl)
                        .formLogin()
                        .loginPage("/index.html")
                        .usernameParameter("userName")
                        .loginProcessingUrl("/user/login")
                        .defaultSuccessUrl("/main.html")
                        .permitAll()
                        .and()
                        .authorizeHttpRequests()
                        .antMatchers("/user/helloAdmin")
//                        .hasAuthority("admins")
//                        .hasAnyAuthority("admins","abc")
                        .hasRole("admin01") // 配置允许访问”/user/helloAdmin“的角色
                        .anyRequest()
                        .authenticated()
                        .and()
                        .csrf()
                        .disable()
                        .build();
    }
}
```



Service：

```java
package com.cyw.demo.service.impl;

import com.cyw.demo.mapper.UserMapper;
import com.cyw.demo.pojo.MyUser;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.authority.AuthorityUtils;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;

@Service("userDetailServiceImpl")
// 实现SpringSecurity 的接口
public class UserDetailServiceImpl implements UserDetailsService {
    @Autowired
    private UserMapper userMapper;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        // (1)根据用户名从数据库中查询用户
        MyUser user = userMapper.getUserByName(username);
//        System.out.println(user);
        // 如果没有查询到用户
        if (user == null){
            throw new UsernameNotFoundException("用户名错误或者密码错误");
        }
        // User类是SpringSecurity中UserDetails接口的实现类
        User userDetails = new User(user.getUserName(),
                user.getPassword(),
                AuthorityUtils.commaSeparatedStringToAuthorityList("abc,ROLE_admin01"));
        return  userDetails;
    }
}

```







### 1.6.4、hasAnyRole 方法



与上面三个方法类似。







## 1.7、自定义 403 页面



（1）创建一个 403 页：

```html
<!DOCTYPE html>
<html lang="zh-cn">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h2>无权限访问！</h2>
</body>
</html>
```

（2）在WebSecurityConfig配置类中，配置403的url：

```java
package com.cyw.demo.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.*;
import org.springframework.security.config.annotation.authentication.configuration.AuthenticationConfiguration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.core.userdetails.UserDetailsService;

@EnableWebSecurity
@Configuration
//@EnableGlobalMethodSecurity(prePostEnabled = true, securedEnabled = true)
public class SecurityConfig {

    @Autowired
    private UserDetailsService userDetailServiceImpl;

    @Bean
    public BCryptPasswordEncoder bCryptPasswordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Bean
    public AuthenticationManager authenticationManager(AuthenticationConfiguration authenticationConfiguration) throws Exception {
        return authenticationConfiguration.getAuthenticationManager();
    }

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        // 403 页面的配置
        http.exceptionHandling().accessDeniedPage("/403.html");
        
        return
                http.userDetailsService(userDetailServiceImpl)
                        .formLogin()
                        .loginPage("/index.html")
                        .usernameParameter("userName")
                        .loginProcessingUrl("/user/login")
                        .defaultSuccessUrl("/main.html")
                        .permitAll()
                        .and()
                        .authorizeHttpRequests()
                        .antMatchers("/user/helloAdmin")
//                        .hasAuthority("admins")
//                        .hasAnyAuthority("admins","abc")
                        .hasRole("admin01")
                        .anyRequest()
                        .authenticated()
                        .and()
                        .csrf()
                        .disable()
                        .build();
    }
}

```















## 1.8、授权-注解



（1）在启动类（或配置类）上，使用注解`@EnableGlobalMethodSecurity`

（2）在具体的Controller中使用注解：

- `@Secured({})`
- `@PreAuthorize()`
- `@PostAuthorize `
- `@PreFilter `
- `@PostFilter `





### 1.8.1、@Secured 角色控制注解



（1）在启动类（或配置类）上，使用注解`@EnableGlobalMethodSecurity(securedEnabled = true)`

```java
package com.cyw.demo;

import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;

@SpringBootApplication
@MapperScan("com.cyw.demo.mapper")
// 启用注解方式的授权
@EnableGlobalMethodSecurity(securedEnabled = true)
public class DemoApplication {
	public static void main(String[] args) {
		SpringApplication.run(DemoApplication.class, args);
	}
}
```



在控制器方法上，基于**角色**的控制：使用注解`@Secured({”角色名1“,”角色名2“})`

```java
package com.cyw.demo.controller;

import com.cyw.demo.pojo.MyResult;
import com.cyw.demo.pojo.MyUser;
import org.springframework.security.access.annotation.Secured;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/user")
public class LoginController {

    // 注解配置权限
    @GetMapping("/helloAdmin")
    @Secured({"ROLE_admin789","ROLE_normal789"})
    public String helloAdmin(){
        return "hello Admin ";
    }

    @PostMapping("/login")
    public MyResult login(MyUser myUser){
        System.out.println(myUser);
        return MyResult.ok("控制层！！");
    }
}
```





### 1.8.2、@PreAuthorize 权限和角色控制注解



先执行注解的权限控制，再执行方法，一般被修饰的方法有返回值。



（1）在启动类（或配置类）上，使用注解`@EnableGlobalMethodSecurity(prePostEnabled= true)`

```java
package com.cyw.demo;

import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;

@SpringBootApplication
@MapperScan("com.cyw.demo.mapper")
// 启用注解方式的授权
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class DemoApplication {
	public static void main(String[] args) {
		SpringApplication.run(DemoApplication.class, args);
	}
}
```



（2）在控制器方法上，基于**权限**的控制：使用注解`@preAuthorize(”hasAuthority('权限名')“)`

```java
package com.cyw.demo.controller;

import com.cyw.demo.pojo.MyResult;
import com.cyw.demo.pojo.MyUser;
import org.springframework.security.access.annotation.Secured;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/user")
public class LoginController {

    // 注解配置权限，这里hasAuthority可以改成之前的其他3个方法
    @GetMapping("/helloAdmin")
    @PreAuthorize("hasAuthority('normal789')")
    public String helloAdmin(){
        return "hello Admin ";
    }

    @PostMapping("/login")
    public MyResult login(MyUser myUser){
        System.out.println(myUser);
        return MyResult.ok("控制层！！");
    }
}
```





### 1.8.2、@PostAuthorize 权限和角色控制注解



先执行方法，再执行注解的权限控制，一般被修饰的方法有返回值。



（1）在启动类（或配置类）上，使用注解`@EnableGlobalMethodSecurity(prePostEnabled= true)`

```java
package com.cyw.demo;

import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;

@SpringBootApplication
@MapperScan("com.cyw.demo.mapper")
// 启用注解方式的授权
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class DemoApplication {
	public static void main(String[] args) {
		SpringApplication.run(DemoApplication.class, args);
	}
}
```



（2）在控制器方法上，基于**权限**的控制：使用注解`@preAuthorize(”hasAuthority('权限名')“)`

```java
package com.cyw.demo.controller;

import com.cyw.demo.pojo.MyResult;
import com.cyw.demo.pojo.MyUser;
import org.springframework.security.access.annotation.Secured;
import org.springframework.security.access.prepost.PostAuthorize;
import org.springframework.security.access.prepost.PreAuthorize;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/user")
public class LoginController {

    @GetMapping("/hello")
    public String hello(){
        return "hello ";
    }


    @GetMapping("/helloAdmin")
//    @Secured({"ROLE_admin789","ROLE_normal789"})
//    @PreAuthorize("hasAuthority('normal789')")
    @PostAuthorize("hasAuthority('normal789')")
    public String helloAdmin(){
        return "hello Admin ";
    }

    @PostMapping("/login")
    public MyResult login(MyUser myUser){
        System.out.println(myUser);
        return MyResult.ok("控制层！！");
    }
}

```





### 1.8.3、@PreFilter



对方法的参数进行过滤。



### 1.8.4、@PostFilter



对方法的返回数据进行过滤。





## 1.9、退出登录



在WebSecurityConfig配置类中：

```java
package com.cyw.demo.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.*;
import org.springframework.security.config.annotation.authentication.configuration.AuthenticationConfiguration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.core.userdetails.UserDetailsService;

@EnableWebSecurity
@Configuration
//@EnableGlobalMethodSecurity(prePostEnabled = true, securedEnabled = true)
public class SecurityConfig {

    @Autowired
    private UserDetailsService userDetailServiceImpl;

    @Bean
    public BCryptPasswordEncoder bCryptPasswordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Bean
    public AuthenticationManager authenticationManager(AuthenticationConfiguration authenticationConfiguration) throws Exception {
        return authenticationConfiguration.getAuthenticationManager();
    }

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http.exceptionHandling().accessDeniedPage("/403.html");
        // 退出页配置
        http.logout()
            .logoutUrl("/logout")
            .logoutSuccessUrl("/");
            //.permitAll();
        
        return
                http.userDetailsService(userDetailServiceImpl)
                        .formLogin()
                        .loginPage("/index.html")
                        .usernameParameter("userName")
                        .loginProcessingUrl("/user/login")
                        .defaultSuccessUrl("/main.html")
                        .permitAll()
                        .and()
                        .authorizeHttpRequests()
//                        .antMatchers("/user/helloAdmin")
//                        .hasAuthority("admins")
//                        .hasAnyAuthority("admins","abc")
//                        .hasRole("admin01")
                        .anyRequest()
                        .authenticated()
                        .and()
                        .csrf()
                        .disable()
                        .build();
    }
}

```





## 1.10、记住我-功能

（1）数据库中创建数据表：

```sql
CREATE TABLE persistent_logins (
	username VARCHAR ( 64 ) NOT NULL,
	series VARCHAR ( 64 ) PRIMARY KEY,
	token VARCHAR ( 64 ) NOT NULL,
    last_used TIMESTAMP NOT NULL 
);
```



（2）在WebSecurityConfig配置类中，注入数据源，注册PersistentTokenRepository，配置记住我：

```java
package com.cyw.demo.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.*;
import org.springframework.security.config.annotation.authentication.configuration.AuthenticationConfiguration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.web.authentication.rememberme.JdbcTokenRepositoryImpl;
import org.springframework.security.web.authentication.rememberme.PersistentTokenRepository;

import javax.sql.DataSource;

@EnableWebSecurity
@Configuration
//@EnableGlobalMethodSecurity(prePostEnabled = true, securedEnabled = true)
public class SecurityConfig {
    
    // 记住我功能
    @Autowired
    private DataSource dataSource;
    
    

    // 记住我功能
    @Bean
    public PersistentTokenRepository persistentTokenRepository(){
        JdbcTokenRepositoryImpl jdbcTokenRepository = new JdbcTokenRepositoryImpl();
        jdbcTokenRepository.setDataSource(dataSource);
        return jdbcTokenRepository;
    }      
    
    @Autowired
    private UserDetailsService userDetailServiceImpl;


    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http.exceptionHandling().accessDeniedPage("/403.html");
        http.logout().logoutUrl("/logout").logoutSuccessUrl("/");
//                .permitAll();
        return
                http.userDetailsService(userDetailServiceImpl)
                        .formLogin()
                        .loginPage("/index.html")
                        .usernameParameter("userName")
                        .loginProcessingUrl("/user/login")
                        .defaultSuccessUrl("/main.html")
                        .permitAll()
                        .and()
                        .authorizeHttpRequests()
//                        .antMatchers("/user/helloAdmin")
//                        .hasAuthority("admins")
//                        .hasAnyAuthority("admins","abc")
//                        .hasRole("admin01")
                        .anyRequest()
                        .authenticated()
                        .and()
                // 记住我功能
                        .rememberMe()
                        .tokenRepository(persistentTokenRepository())
                        .tokenValiditySeconds(60) // 设置记住我功能的有效时间：60 秒
                        .userDetailsService(userDetailServiceImpl)
                // 记住我功能（结束）
                        .and()
                        .csrf()
                        .disable()
                        .build();
    }
    
    
    @Bean
    public BCryptPasswordEncoder bCryptPasswordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Bean
    public AuthenticationManager authenticationManager(AuthenticationConfiguration authenticationConfiguration) throws Exception {
        return authenticationConfiguration.getAuthenticationManager();
    }
}

```



（3）前端页面使用记住我 checkbox：

注意：标签的name属性必须是`remember-me`

```java

    <form method="post" action="/user/login">
        <input type="text" name="userName" placeholder="用户名"> <br/>
        <input type="text" name="password" placeholder="密码"> <br/>
        <input type="checkbox" name="remember-me">记住我 <br>
        <input type="submit" value="登录"> <br/>
    </form>
```





## 2.1、Redis+Security 模拟短信登录



Maven配置：

```xml
  		<dependency>
            <groupId>cn.hutool</groupId>
            <artifactId>hutool-all</artifactId>
            <version>5.8.4</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

<!-- Redis 配置-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
            <version>4.2.3</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>
```









### 2.1.1、封装验证码工具类

`SmsUtil`-生成Redis的验证码生成工具类：

```java
package com.cyw.backendmain.util;

import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.stereotype.Component;

import java.util.concurrent.TimeUnit;


@Component
public class SmsUtil {
    private static RedisTemplate redisTemplate;
    private RedisBean redisBean = new RedisBean();

    // 2小时
    private long expire = 120;
    private TimeUnit timeUnit = TimeUnit.MINUTES;

    public SmsUtil() {
        redisTemplate = RedisBean.redis;
    }

    /**
     * 生成4位数的验证码
     * @return
     */
    private int createCode() {
        int code = (int) Math.ceil(Math.random() * 9000 + 1000);
        return code;
    }

    /**
     * 生成4位数的验证码
     * @param phone
     */
    public String createCode(String phone) {
        int code = createCode();
        saveSms(phone,code);
        return code+"";
    }

    /**
     * 保存验证码到Redis
     * @param phone
     * @param checkCode
     */
    public void saveSms(String phone, int checkCode) {
        redisTemplate.opsForValue().set("checkCode:" + phone, checkCode+"", expire, timeUnit);
    }

    /**
     * 获取Redis中的验证码
     * @param phone
     * @return
     */
    public String getCode(String phone){
        return (String)redisTemplate.opsForValue().get("checkCode:"+phone);
    }

    /**
     * 删除Redis中的验证码
     * @param phone
     */
    public void deleteSms(String phone) {
        redisTemplate.delete("checkCode:" + phone);
    }

    /**
     * 删除Redis中的多个验证码
     * @param phones
     */
    public void deleteManySms(String... phones) {
        for (String phone : phones) {
            redisTemplate.delete(phone);
        }
    }

    /**
     * 设置Redis中的验证码的有效时长
     * @param phone
     */
    public void setSmsExpire(String phone) {
        redisTemplate.expire(phone, expire, timeUnit);
    }

    /**
     * 设置Redis中的验证码的有效时长
     * @param phone
     * @param time
     */
    public void setSmsExpire(String phone, long time) {
        redisTemplate.expire(phone, time, timeUnit);
    }

    /**
     * 设置Redis中的验证码的有效时长
     * @param phone
     * @param time
     * @param timeUnit
     */
    public void setSmsExpire(String phone, long time, TimeUnit timeUnit) {
        redisTemplate.expire(phone, time, timeUnit);
    }

    /**
     * 获取Redis中的验证码的有效时长
     * @param phone
     * @return
     */
    public String getSmsExpire(String phone) {
        Long expire = redisTemplate.getExpire("checkCode:" + phone);
        return expire+"";
    }

    /**
     * 判断Redis中是否已存在验证码
     * @param phone
     * @return
     */
    public boolean hasSmsKey(String phone) {
        return redisTemplate.hasKey("checkCode:" + phone);
    }

}

```





短信配置：

```yaml
txsms:
  appId: 1400714069
  secretId: AKIDGMFtH5fYCblhOpz0oOioAJEwQhjVzGNF
  secretKey: gR2QYR7jJk3IQTOBsUwJk6Khx52AD8qI
  signName: 小cyw软工学习簿
  templateId: 1490127
```





发送验证码的工具类：

```java
package com.cyw.backendmain.util;

import com.tencentcloudapi.common.Credential;
import com.tencentcloudapi.common.exception.TencentCloudSDKException;
import com.tencentcloudapi.common.profile.ClientProfile;
import com.tencentcloudapi.sms.v20210111.SmsClient;
import com.tencentcloudapi.sms.v20210111.models.SendSmsRequest;
import com.tencentcloudapi.sms.v20210111.models.SendSmsResponse;
import lombok.Data;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;


@ConfigurationProperties(prefix = "txsms")
@Data
@Component
public class SendSmsUtil {
    private String appId;
    private String secretId;
    private String secretKey;
    private String signName;
    private String templateId;

    /**
     * @param checkCode      验证码
     * @param expire         有效时长（分钟）
     * @param phoneNumberSet 接收验证码的手机号数组（以+86开头）
     * @return 执行结果
     */
    public String sendSms(String checkCode, String expire, String... phoneNumberSet) {
        String rst = "";
        try {
            // 使用 secretId 和 secretKey
            Credential cred = new Credential(this.secretId, this.secretKey);

            // 固定写法
            ClientProfile clientProfile = new ClientProfile();
            clientProfile.setSignMethod("HmacSHA256");
            SmsClient client = new SmsClient(cred, "ap-guangzhou", clientProfile);
            SendSmsRequest req = new SendSmsRequest();

            // 使用appId
            req.setSmsSdkAppId(this.appId);
            // 使用在腾讯云平台申请的短信签名（也就是验证码短信的中括号中的主体名）
            req.setSignName(this.signName);
            // 使用在腾讯云平台申请的短信内容模板的 ID
            req.setTemplateId(this.templateId);
            // 在短信模板中填充验证码和有效时间（模板有几个变量，数组元素个数就是几个）
            String[] templateParamSet = {checkCode, expire};
            req.setTemplateParamSet(templateParamSet);
            // 设置要接收验证码的手机号数组
            req.setPhoneNumberSet(phoneNumberSet);

            // 开始发送验证码
            SendSmsResponse res = client.SendSms(req);
            rst = SendSmsResponse.toJsonString(res);
        } catch (TencentCloudSDKException e) {
            e.printStackTrace();
        }
        return rst;
    }
}

```







### 2.1.2、解决报错

解决无法在过滤器中自动注入RedisTemplate对象的问题（会标红，但不影响使用）：

```java
package com.cyw.backendmain.util;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.stereotype.Component;

import javax.annotation.PostConstruct;

@Component
public class RedisBean {
    @Autowired
    private StringRedisTemplate stringRedisTemplate;
    public static StringRedisTemplate redis;

    @PostConstruct
    private void getRedisTemplate(){
        redis=this.stringRedisTemplate;
    }
}

```



Redis配置：

```java
@Configuration
public class RedisConfig {

    /**
     * 解决Redis乱码问题
     * @param redisConnectionFactory
     * @return
     */
    @Bean
    public RedisTemplate redisTemplate(RedisConnectionFactory redisConnectionFactory) {
        RedisTemplate redisTemplate = new RedisTemplate();
        redisTemplate.setConnectionFactory(redisConnectionFactory);
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);

        ObjectMapper objectMapper = new ObjectMapper();
        objectMapper.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        objectMapper.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);

        jackson2JsonRedisSerializer.setObjectMapper(objectMapper);

        //重点在这四行代码
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setValueSerializer(jackson2JsonRedisSerializer);
        redisTemplate.setHashKeySerializer(new StringRedisSerializer());
        redisTemplate.setHashValueSerializer(jackson2JsonRedisSerializer);

        redisTemplate.afterPropertiesSet();
        return redisTemplate;
    }
}
```







### 2.1.3、WebSecurityConfig



`WebSecurityConfig`配置类：

```java
package com.cyw.backendmain.config;

import com.cyw.backendmain.other.SmsCodeAuthenticationFilter;
import com.cyw.backendmain.other.CustomAuthenticationFailureHandler;
import com.cyw.backendmain.other.CustomAuthenticationSuccessHandler;
import com.cyw.backendmain.other.SmsCodeAuthenticationProvider;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.authentication.ProviderManager;
import org.springframework.security.config.annotation.SecurityConfigurerAdapter;
import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityCustomizer;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;
import javax.annotation.Resource;

@Configuration
@EnableWebSecurity
public class WebSecurityConfig  extends SecurityConfigurerAdapter {

    @Resource
    private UserDetailsService userDetailsServiceImpl;
    @Autowired
    private CustomAuthenticationSuccessHandler customAuthenticationSuccessHandler;
    @Autowired
    private CustomAuthenticationFailureHandler customAuthenticationFailureHandler;

    /**
     * 使用用户名密码登录时的密码加密器
     * @return
     */
    @Bean
    public BCryptPasswordEncoder bCryptPasswordEncoder() {
        return new BCryptPasswordEncoder();
    }
    @Bean
    public WebSecurityCustomizer webSecurityCustomizer() {
        return web -> web.ignoring();
    }

    /**
     * SpringSecurity的核心配置
     * @param http
     * @return
     * @throws Exception
     */
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        // 执行顺序：
        //      Filter =>
        //          AuthenticationManager【providerManager】 =>
        //              provider =>
        //                  userDetailsServiceImpl【我们自己对框架接口的实现类】
        //                      返回User数据【框架的封装User类，是UserDetails接口的实现类】
        //                  => 决策：登录成功或失败（两个处理器），返回给浏览器数据        //
        SmsCodeAuthenticationProvider smsCodeAuthenticationProvider = new SmsCodeAuthenticationProvider();
        smsCodeAuthenticationProvider.setUserDetailsService(userDetailsServiceImpl);
        ProviderManager providerManager = new ProviderManager(smsCodeAuthenticationProvider);
        SmsCodeAuthenticationFilter smsCodeAuthenticationFilter = new SmsCodeAuthenticationFilter();
        smsCodeAuthenticationFilter.setAuthenticationManager(http.getSharedObject(AuthenticationManager.class));
        smsCodeAuthenticationFilter.setAuthenticationManager(providerManager);
        smsCodeAuthenticationFilter.setAuthenticationSuccessHandler(customAuthenticationSuccessHandler);
        smsCodeAuthenticationFilter.setAuthenticationFailureHandler(customAuthenticationFailureHandler);
        http.addFilterAt(smsCodeAuthenticationFilter,UsernamePasswordAuthenticationFilter.class);


        // 配置退出登录的规则
        http.logout().logoutUrl("/logout")
                .logoutSuccessUrl("/index.html")
                .permitAll();

        // 配置未授权页面的规则
        http.exceptionHandling().accessDeniedPage("/4xx.html");

        // 配置登录规则
        http
                .formLogin()
                .usernameParameter("mobile")
                // 自定义登录页
                .loginPage("/index.html")
                // 登录逻辑处理的 controller
                .loginProcessingUrl("/login")
                // 登录成功后跳转到的页面
                .defaultSuccessUrl("/page/home.html")
                .permitAll()
                // 允许以上url通过
                .and()
                .authorizeHttpRequests()
                .antMatchers("/sms/**", "/js/**", "/css/**","/login","/4xx.html","/error/**")
                .permitAll()
                .anyRequest()
                .authenticated()
                .and()
                .csrf()
                .disable();
        return http.build();
    }

}

```



### 2.1.4、SmsCodeAuthenticationFilter





注意：该过滤器不能注入IOC容器，否则会报错。

`SmsCodeAuthenticationFilter` 类：

```java
package com.cyw.backendmain.other;


import cn.hutool.json.JSONObject;
import cn.hutool.json.JSONUtil;
import lombok.extern.slf4j.Slf4j;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.security.authentication.AuthenticationServiceException;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

@Slf4j
public class SmsCodeAuthenticationFilter extends UsernamePasswordAuthenticationFilter {

    /**
     * 是否仅 POST 方式
     */
    private boolean postOnly = true;

    private StringRedisTemplate redisTemplate;

    public SmsCodeAuthenticationFilter(StringRedisTemplate redisTemplate){
        this.redisTemplate = redisTemplate;
    }

    public SmsCodeAuthenticationFilter(){

    }

    @Override
    public Authentication attemptAuthentication(HttpServletRequest request, HttpServletResponse response) throws AuthenticationException {
        if (postOnly && !request.getMethod().equals("POST")) {
            throw new AuthenticationServiceException(
                    "不支持该请求方法: " + request.getMethod());
        }

        // 读取前端传过来的手机号和验证码
        try {
            BufferedReader br = new BufferedReader(new InputStreamReader(request.getInputStream()));
            StringBuffer sb=new StringBuffer();
            String s=null;
            while((s=br.readLine())!=null){
                sb.append(s);
            }
            JSONObject jsonObject = JSONUtil.parseObj(sb.toString());
            String mobile = jsonObject.getStr("mobile");
            String checkCode = jsonObject.getStr("checkCode");
            if (mobile == null) {
                mobile = "";
            }
            mobile = mobile.trim();
            // 构造一个空的用户信息交给provider操作
            SmsCodeAuthenticationToken userAuthToken = new SmsCodeAuthenticationToken(mobile,checkCode,null,null);
            this.setDetails(request, userAuthToken);
            // 传给Provider
            return this.getAuthenticationManager().authenticate(userAuthToken);

        } catch (IOException e) {
            e.printStackTrace();
        }
        return null;
    }

    protected void setDetails(HttpServletRequest request, SmsCodeAuthenticationToken userAuthToken) {
        userAuthToken.setDetails(authenticationDetailsSource.buildDetails(request));
    }

    @Override
    public void setPostOnly(boolean postOnly) {
        this.postOnly = postOnly;
    }
}
```



### 2.1.5、SmsCodeAuthenticationProvider

`SmsCodeAuthenticationProvider`类

```java
package com.cyw.backendmain.other;

import com.cyw.backendmain.util.SmsUtil;
import com.cyw.backendmain.vo.MyUserDetails;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.security.authentication.AuthenticationProvider;
import org.springframework.security.authentication.BadCredentialsException;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.stereotype.Component;
import org.springframework.web.context.request.RequestContextHolder;
import org.springframework.web.context.request.ServletRequestAttributes;

import javax.annotation.Resource;
import javax.servlet.http.HttpServletRequest;


@Slf4j
@Component
public class SmsCodeAuthenticationProvider implements AuthenticationProvider {
    @Resource
    private UserDetailsService userDetailsServiceImpl;

    private SmsUtil smsUtil = new SmsUtil();


    /**
     * 执行短信验证码的登录逻辑（调用UserDetailsServiceImpl）
     * 由SmsCodeAuthenticationFilter来提供 authenticationToken
     * @param authentication
     * @return
     * @throws AuthenticationException
     */
    @Override
    public Authentication authenticate(Authentication authentication) throws AuthenticationException {
        SmsCodeAuthenticationToken authenticationToken = (SmsCodeAuthenticationToken) authentication;
        if(authenticationToken==null){
            throw new BadCredentialsException("请检查手机号或验证码输入是否正确！");
        }

        // 获取前端的手机号
        String mobile = (String) authenticationToken.getPrincipal();
        String checkCode = authenticationToken.getCheckCode();
        log.info("前端输入的手机号: "+mobile);
        log.info("前端输入的验证码: "+checkCode);

        // 校验验证码的是否一致
        checkSmsCode(mobile,checkCode);
        // 调用Service层，从数据库中查用户数据
        MyUserDetails userDetails = (MyUserDetails)userDetailsServiceImpl.loadUserByUsername(mobile);
         // 此时鉴权成功后，应当重新 new 一个拥有鉴权的 authenticationResult 返回
        SmsCodeAuthenticationToken userToken = new SmsCodeAuthenticationToken(mobile,checkCode,userDetails, userDetails.getAuthorities());
        return userToken;
    }

    /**
     * 验证短信验证码，参数是由本类的authenticate方法传入
     * @param mobileInput
     * @param checkCodeInput
     */
    private void checkSmsCode(String mobileInput,String checkCodeInput) {
        HttpServletRequest request = ((ServletRequestAttributes) RequestContextHolder.getRequestAttributes()).getRequest();

        // 检查Redis中的是否有验证码，
        // 有验证码，则比较，没有，则提示无验证码
        boolean hasSmsKey = smsUtil.hasSmsKey(mobileInput);
        if(!hasSmsKey){
            throw new BadCredentialsException("该手机号未检测到验证码，请检查手机号输入是否正确！");
        }
        String checkCodeFromRedis = smsUtil.getCode(mobileInput);
        if(!checkCodeFromRedis.equals(checkCodeInput)) {
            throw new BadCredentialsException("验证码错误!");
        }
    }

    @Override
    public boolean supports(Class<?> authentication) {
        // 判断 authentication 是不是 SmsCodeAuthenticationToken 的子类或子接口
        return SmsCodeAuthenticationToken.class.isAssignableFrom(authentication);
    }

    public UserDetailsService getUserDetailsService() {
        return userDetailsServiceImpl;
    }

    public void setUserDetailsService(UserDetailsService userDetailsService) {
        this.userDetailsServiceImpl = userDetailsService;
    }
}

```



### 2.1.6、SmsCodeAuthenticationToken

`SmsCodeAuthenticationToken` 类：

```java
package com.cyw.backendmain.other;


import com.cyw.backendmain.vo.MyUserDetails;
import org.springframework.security.authentication.AbstractAuthenticationToken;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.SpringSecurityCoreVersion;
import java.util.Collection;


public class SmsCodeAuthenticationToken extends AbstractAuthenticationToken {

    private static final long serialVersionUID = SpringSecurityCoreVersion.SERIAL_VERSION_UID;

    /**
     * 在 UsernamePasswordAuthenticationToken 中该字段代表登录的用户名，
     * 在这里就代表登录的手机号码
     */
    private final Object principal;
    private MyUserDetails myUserDetails;
    private String checkCode;

    /**
     * 构建一个没有鉴权的 SmsCodeAuthenticationToken
     */
//    public SmsCodeAuthenticationToken(Object principal,String checkCode) {
//        super(null);
//        this.principal = principal;
//        this.checkCode = checkCode;
//        setAuthenticated(false);
//    }

    /**
     * 构建一个有鉴权的 SmsCodeAuthenticationToken
     */
    public SmsCodeAuthenticationToken(Object principal,String checkCode,MyUserDetails myUserDetails, Collection<? extends GrantedAuthority> authorities) {
        super(authorities);
        this.checkCode = checkCode;
        this.principal = principal;
        this.myUserDetails = myUserDetails;
        super.setAuthenticated(true);
    }

    @Override
    public Object getCredentials() {
        return null;
    }

    @Override
    public Object getPrincipal() {
        return this.principal;
    }


    public String getCheckCode() {
        return this.checkCode;
    }

    public MyUserDetails getMyUserDetails() {
        return myUserDetails;
    }

    public void setMyUserDetails(MyUserDetails myUserDetails) {
        this.myUserDetails = myUserDetails;
    }

    @Override
    public void setAuthenticated(boolean isAuthenticated) throws IllegalArgumentException {
        if (isAuthenticated) {
            throw new IllegalArgumentException(
                    "Cannot set this token to trusted - use constructor which takes a GrantedAuthority list instead");
        }

        super.setAuthenticated(false);
    }

    @Override
    public void eraseCredentials() {
        super.eraseCredentials();
    }
}
```



### 2.1.7、登录成功-处理器：

```java
package com.cyw.backendmain.other;

import cn.hutool.json.JSONObject;
import cn.hutool.json.JSONUtil;
import com.cyw.backendmain.pojo.MyUser;
import com.cyw.backendmain.util.JwtUtil;
import com.cyw.backendmain.util.RedisBean;
import com.cyw.backendmain.util.SmsUtil;
import com.cyw.backendmain.vo.MyRst;
import com.cyw.backendmain.vo.MyUserDetails;
import com.fasterxml.jackson.databind.ObjectMapper;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.Authentication;
import org.springframework.security.web.authentication.AuthenticationSuccessHandler;
import org.springframework.stereotype.Component;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@Slf4j
@Component
public class CustomAuthenticationSuccessHandler implements AuthenticationSuccessHandler {
    @Autowired
    private ObjectMapper objectMapper;
    private SmsUtil smsUtil = new SmsUtil();

    @Override
    public void onAuthenticationSuccess(HttpServletRequest request,
                                        HttpServletResponse response,
                                        Authentication authentication)
            throws IOException, ServletException {

        log.info("登录成功！");
        MyUserDetails userDetails = ((SmsCodeAuthenticationToken) authentication).getMyUserDetails();
        // log.info("最终返回的的details: "+userDetails.toString());

        // 这里的用户名就是数据库中的手机号
        String username = (String)authentication.getPrincipal();

        // 生成JWT，设置到给前端的数据里
        MyUser myUser = userDetails.getMyUser();
        String token = JwtUtil.createToken(username);
        myUser.setToken(token);
        userDetails.setMyUser(myUser);

        // 删除Redis中的验证码
        smsUtil.deleteSms(username);
        response.setContentType("application/json;charset=UTF-8");
        response.getWriter().write(objectMapper.writeValueAsString(MyRst.success("登录成功！",userDetails)));
    }
}
```





### 2.1.8、登录失败-处理器

```
package com.cyw.backendmain.other;

import com.cyw.backendmain.util.SmsUtil;
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.web.authentication.AuthenticationFailureHandler;
import org.springframework.stereotype.Component;

import javax.annotation.Resource;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@Slf4j
@Component
public class CustomAuthenticationFailureHandler  implements AuthenticationFailureHandler {
    @Autowired
    private ObjectMapper objectMapper;
    @Resource
    private SmsUtil smsUtil;

    @Override
    public void onAuthenticationFailure(HttpServletRequest request
            , HttpServletResponse response
            , AuthenticationException exception)
            throws IOException {
        log.info("登录失败！");
        response.setStatus(HttpStatus.INTERNAL_SERVER_ERROR.value());
        response.setContentType("application/json;charset=UTF-8");
        response.getWriter().write(objectMapper.writeValueAsString(exception.getMessage()));
    }
}
```



### 2.1.9、控制层：

```java
package com.cyw.backendmain.controller;

import com.cyw.backendmain.util.JwtUtil;
import com.cyw.backendmain.util.SendSmsUtil;
import com.cyw.backendmain.util.SmsUtil;
import com.cyw.backendmain.vo.MyRst;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;
import javax.servlet.http.HttpServletRequest;

@RestController
@Slf4j
public class SmsController {

    private SmsUtil smsUtil = new SmsUtil();
    @Autowired
    private SendSmsUtil sendSmsUtil;

    /**
     * 生成短信验证码
     * @param mobile
     * @return
     */
    @RequestMapping("/sms/code")
    public MyRst sms(@RequestParam("mobile") String mobile) {
        if (mobile == null || "".equals(mobile) || mobile.length() != 11) {
            return MyRst.fail("手机号格式错误！请重新输入！");
        }
        String code = smsUtil.createCode(mobile);
        // 发送短信
//        String rst = sendSmsUtil.sendSms(code, smsUtil.getSmsExpire(mobile), mobile);
//        log.info("SMS调用结果："+rst);
        return MyRst.success("验证码生成成功！", code);
//        return MyRst.success("验证码生成成功！");
    }

    /**
     * 校验JWT令牌的有效性
     * @param request
     * @return
     */
    @GetMapping("/checkToken")
    public boolean checkToken(HttpServletRequest request) {
        String userToken = request.getHeader("Authentication");
        return JwtUtil.checkToken(userToken);
    }
}

```







## 2.2、Redis+Security 短信登录+密码登录



该案例相比`案例2.1 Redis+Security 短信登录`的变动：

> - SpringSecurity `配置文件`的变动
> - `AuthenticationFilter`认证过滤器
> - 新增一个密码登录的`Provider`
> - `登录成功处理器`的变动





（1）SpringSecurity  配置文件的变动：

创建一个密码登录的 **Provider**，设置该 Provider 需要调用的 **UserDetailsService** 服务，将多种登录方式的 **provider列表** 交给**认证管理器**，认证管理器交给认证**过滤器管理**

```java
package com.cyw.backendmain.config;

import com.cyw.backendmain.other.*;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.authentication.AuthenticationProvider;
import org.springframework.security.authentication.ProviderManager;
import org.springframework.security.config.annotation.SecurityConfigurerAdapter;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityCustomizer;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;
import javax.annotation.Resource;
import java.util.ArrayList;

@Configuration
@EnableWebSecurity
public class WebSecurityConfig  extends SecurityConfigurerAdapter {

    @Resource
    private UserDetailsService userDetailsServiceImpl;
    @Autowired
    private CustomAuthenticationSuccessHandler customAuthenticationSuccessHandler;
    @Autowired
    private CustomAuthenticationFailureHandler customAuthenticationFailureHandler;

    /**
     * 使用用户名密码登录时的密码加密器
     * @return
     */
    @Bean
    public BCryptPasswordEncoder bCryptPasswordEncoder() {
        return new BCryptPasswordEncoder();
    }
    @Bean
    public WebSecurityCustomizer webSecurityCustomizer() {
        return web -> web.ignoring();
    }

    /**
     * SpringSecurity的核心配置
     * @param http
     * @return
     * @throws Exception
     */
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        // 执行顺序：
        //      Filter =>
        //          AuthenticationManager【providerManager】 =>
        //              provider =>
        //                  userDetailsServiceImpl【我们自己对框架接口的实现类】
        //                      返回User数据【框架的封装User类，是UserDetails接口的实现类】
        //                  => 决策：登录成功或失败（两个处理器），返回给浏览器数据        //

        // 短信登录
        SmsCodeAuthenticationProvider smsCodeAuthenticationProvider = new SmsCodeAuthenticationProvider();
        
        // 密码登录
        PasswordAuthenticationProvider pwdAuthenticationProvider = new PasswordAuthenticationProvider();

        // 分别设置两种登录方式需要调用的查询服务
        smsCodeAuthenticationProvider.setUserDetailsService(userDetailsServiceImpl);
        pwdAuthenticationProvider.setUserDetailsService(userDetailsServiceImpl);

        // 将多种登录方式的provider交给认证管理器
        ArrayList<AuthenticationProvider> providerList = new ArrayList<>();
        providerList.add(smsCodeAuthenticationProvider);
        providerList.add(pwdAuthenticationProvider);
        ProviderManager providerManager = new ProviderManager(providerList);

        // 认证管理器交给认证过滤器管理
        MyAuthenticationFilter myAuthenticationFilter = new MyAuthenticationFilter();
        myAuthenticationFilter.setAuthenticationManager(providerManager);

        // 配置登录成功和失败的处理器
        myAuthenticationFilter.setAuthenticationSuccessHandler(customAuthenticationSuccessHandler);
        myAuthenticationFilter.setAuthenticationFailureHandler(customAuthenticationFailureHandler);

        http.addFilterAt(myAuthenticationFilter,UsernamePasswordAuthenticationFilter.class);


        // 配置退出登录的规则
        http.logout().logoutUrl("/logout")
                .logoutSuccessUrl("/index.html")
                .permitAll();

        // 配置未授权页面的规则
        http.exceptionHandling().accessDeniedPage("/4xx.html");

        // 配置登录规则
        http
                .formLogin()
                .usernameParameter("mobile")
                .passwordParameter("pwd")
                // 自定义登录页
                .loginPage("/index.html")
                // 登录逻辑处理的 controller
                .loginProcessingUrl("/login")
                // 登录成功后跳转到的页面
                .defaultSuccessUrl("/page/home.html")
                .permitAll()
                // 允许以上url通过
                .and()
                .authorizeHttpRequests()
                .antMatchers("/sms/**", "/js/**", "/css/**","/login","/4xx.html","/error/**")
                .permitAll()
                .anyRequest()
                .authenticated()
                .and()
                .cors()
                .and()
                .csrf()
                .disable();
        return http.build();
    }
}
```



（2）AuthenticationFilter 的变动：

重载一个密码登录的`setDetails()`，

在`attemptAuthentication()`方法中根据登录类型创建一个用于存放登录用户完整数据的令牌，

将该空的令牌交给认证管理器，

由认证管理器通过遍历`Provider列表`来选择一个合适的`Provider`（通过`provider`中的`supports()`来判断是否合适）调用`userDetailsService.loadUserByUsername()`进行数据库的查询。

```java
package com.cyw.backendmain.other;


import cn.hutool.json.JSONObject;
import cn.hutool.json.JSONUtil;
import com.cyw.backendmain.vo.PasswordAuthenticationToken;
import com.cyw.backendmain.vo.SmsAuthenticationToken;
import lombok.extern.slf4j.Slf4j;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.security.authentication.AuthenticationServiceException;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

@Slf4j
public class MyAuthenticationFilter extends UsernamePasswordAuthenticationFilter {

    /**
     * 是否仅 POST 方式
     */
    private boolean postOnly = true;

    private StringRedisTemplate redisTemplate;

    public MyAuthenticationFilter(StringRedisTemplate redisTemplate){
        this.redisTemplate = redisTemplate;
    }

    public MyAuthenticationFilter(){

    }

    @Override
    public Authentication attemptAuthentication(HttpServletRequest request, HttpServletResponse response) throws AuthenticationException {
        if (postOnly && !request.getMethod().equals("POST")) {
            throw new AuthenticationServiceException(
                    "不支持该请求方法: " + request.getMethod());
        }

        // 读取前端传过来的手机号和验证码
        try {
            BufferedReader br = new BufferedReader(new InputStreamReader(request.getInputStream()));
            StringBuffer sb=new StringBuffer();
            String s=null;
            while((s=br.readLine())!=null){
                sb.append(s);
            }
            JSONObject jsonObject = JSONUtil.parseObj(sb.toString());
            String loginType = jsonObject.getStr("loginType");
            String mobile = jsonObject.getStr("mobile");
            String pwd = jsonObject.getStr("pwd");
            String checkCode = jsonObject.getStr("checkCode");
            if (mobile == null) {
                mobile = "";
            }
            if(loginType==null){
                new Exception("登录方式错误，请使用短信验证码或密码登录！");
            }
            mobile = mobile.trim();
			// 判断表单提交的数据中的登录类型
            switch (loginType){
                case "sms":{
                    // 构造一个空的用户信息交给provider操作
                    SmsAuthenticationToken authToken = new SmsAuthenticationToken(mobile,null,null,null);
                    authToken.setCheckCode(checkCode);
                    this.setDetails(request, authToken);
                    return this.getAuthenticationManager().authenticate(authToken);
                }
                case "password":{
                    PasswordAuthenticationToken authToken = new PasswordAuthenticationToken(mobile,null,null,null);
                    authToken.setPwd(pwd);
                    this.setDetails(request, authToken);
                    return this.getAuthenticationManager().authenticate(authToken);
                }
                default:{
                    new Exception("登录方式错误，请使用短信验证码或密码登录！");
                }
            }
        } catch (IOException e) {            
            e.printStackTrace();
        }
        return null;
    }

    // 验证码登录
    protected void setDetails(HttpServletRequest request, SmsAuthenticationToken userAuthToken) {
        userAuthToken.setDetails(authenticationDetailsSource.buildDetails(request));
    }
    
    // 密码登录
    protected void setDetails(HttpServletRequest request, PasswordAuthenticationToken userAuthToken) {
        userAuthToken.setDetails(authenticationDetailsSource.buildDetails(request));
    }

    @Override
    public void setPostOnly(boolean postOnly) {
        this.postOnly = postOnly;
    }
}
```



（3）密码登录的 provider：

```java
package com.cyw.backendmain.other;

import com.baomidou.mybatisplus.core.toolkit.StringUtils;
import com.cyw.backendmain.vo.PasswordAuthenticationToken;
import com.cyw.backendmain.vo.SmsAuthenticationToken;
import com.cyw.backendmain.vo.MyUserDetails;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.authentication.AuthenticationProvider;
import org.springframework.security.authentication.BadCredentialsException;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.stereotype.Component;
import javax.annotation.Resource;
import java.util.Collection;

@Slf4j
@Component
public class PasswordAuthenticationProvider implements AuthenticationProvider {
    @Resource
    private UserDetailsService userDetailsServiceImpl;

    private BCryptPasswordEncoder bCryptPasswordEncoder = new BCryptPasswordEncoder();

    public void setUserDetailsService(UserDetailsService userDetailsService) {
        this.userDetailsServiceImpl = userDetailsService;
    }

    @Override
    public Authentication authenticate(Authentication authentication) throws AuthenticationException {
        // 从Filter传入的用户输入的登录信息
        PasswordAuthenticationToken unAuthToken = (PasswordAuthenticationToken) authentication;
        String username = unAuthToken.getPrincipal();
        String password = unAuthToken.getPwd();
        if(StringUtils.isBlank(username)){
            throw new UsernameNotFoundException("用户名(手机号)不可以为空");
        }
        if(StringUtils.isBlank(password)){
            throw new BadCredentialsException("密码不可以为空");
        }
        //查询数据库获取用户信息
        MyUserDetails userDetails = (MyUserDetails)userDetailsServiceImpl.loadUserByUsername(username);
        //比较前端传入的密码明文和数据库中加密的密码是否相等
        if (!bCryptPasswordEncoder.matches(password, userDetails.getPassword())) {
            throw new BadCredentialsException("密码不正确");
        }
        PasswordAuthenticationToken authToken = new PasswordAuthenticationToken(username, password, userDetails, userDetails.getAuthorities());
        return authToken;
    }

    @Override
    public boolean supports(Class<?> authentication) {
        return PasswordAuthenticationToken.class.isAssignableFrom(authentication);
    }
}

```



（4）登录成功处理器：

```java
package com.cyw.backendmain.other;

import com.cyw.backendmain.pojo.MyUser;
import com.cyw.backendmain.util.JwtUtil;
import com.cyw.backendmain.util.SmsUtil;
import com.cyw.backendmain.vo.MyRst;
import com.cyw.backendmain.vo.MyUserDetails;
import com.cyw.backendmain.vo.PasswordAuthenticationToken;
import com.cyw.backendmain.vo.SmsAuthenticationToken;
import com.fasterxml.jackson.databind.ObjectMapper;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.Authentication;
import org.springframework.security.web.authentication.AuthenticationSuccessHandler;
import org.springframework.stereotype.Component;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@Slf4j
@Component
public class CustomAuthenticationSuccessHandler implements AuthenticationSuccessHandler {
    @Autowired
    private ObjectMapper objectMapper;
    private SmsUtil smsUtil = new SmsUtil();

    @Override
    public void onAuthenticationSuccess(HttpServletRequest request,
                                        HttpServletResponse response,
                                        Authentication authentication)
            throws IOException, ServletException {

        log.info("登录成功！");
        MyUserDetails userDetails = null;
        try {
            // 密码登录
            userDetails = ((PasswordAuthenticationToken) authentication).getMyUserDetails();
            // 这里的用户名就是数据库中的手机号
            String username = (String)authentication.getPrincipal();
            // 生成JWT，设置到给前端的数据里
            MyUser myUser = userDetails.getMyUser();
            String token = JwtUtil.createToken(username);
            myUser.setToken(token);
            userDetails.setMyUser(myUser);

        } catch (Exception e) {
            // 短信登录
            userDetails = ((SmsAuthenticationToken) authentication).getMyUserDetails();
            // 这里的用户名就是数据库中的手机号
            String username = (String)authentication.getPrincipal();
            // 生成JWT，设置到给前端的数据里
            MyUser myUser = userDetails.getMyUser();
            String token = JwtUtil.createToken(username);
            myUser.setToken(token);
            userDetails.setMyUser(myUser);
            // 删除Redis中的验证码
            smsUtil.deleteSms(username);
        }
        response.setContentType("application/json;charset=UTF-8");
        response.getWriter().write(objectMapper.writeValueAsString(MyRst.success("登录成功！",userDetails)));
    }
}
```











# 三、JWT 技术



[阮一峰-JWT](https://ruanyifeng.com/blog/2018/07/json_web_token-tutorial.html)



## 1.1、JWT 概述



> JWT 是什么？

JWT （JSON Web Token）是通过数字签名的方式，在不同的终端之间安全传输JSON数据。







<img src="https://cyw-imgbed.oss-cn-hangzhou.aliyuncs.com/img/image-20220725171454587.png" />





> JWT 的用途：

JWT 一般可以作为一种认证授权的凭证，一旦用户登录，前端就可以接收到后端发送过来的 JWT Token，在之后的每个请求的请求头中，携带JWT Token，服务器在处理请求之前，需要先校验请求头中的 JWT Token。

此外，JWT 可以用来在分布式系统中，解决Session共享困难的问题。





> JWT 的结构：

JWT 的Token 是一个String字符串，由三个部分组成（头部、载荷、签名），每个部分之间用小数点`.`分隔，一般使用`Base64`编码。



（1）JWT Token 头部：

```json
{
	"alg" : "HS256", # 加密算法(HMAC、SHA256、RSA)
	"type" : "JWT"	 # 固定写法
}
```



（2）JWT Token 载荷：

```json
{
	"sub" : "123", 
	"name" : "张三",    // 私有字段
	"admin": true,	   // 私有字段
}

// 官方规定的7个字段（可选）
// iss (issuer)：签发人
// exp (expiration time)：过期时间
// sub (subject)：主题
// aud (audience)：受众
// nbf (Not Before)：生效时间
// iat (Issued At)：签发时间
// jti (JWT ID)：编号
```



（3）JWT Token 签名，防篡改：（由头部、负载、自己定义的密钥等三部分，使用头部中的签名算法计算得到）

```java
// 后端
// JWT签名
String signature = HMACSHA256(
    base64UrlEncode(header) + "." + base64UrlEncode(payload)
    , secret);

```



## 1.2、JWT 使用



[JWT介绍以及java-jwt的使用CSDN博客](https://blog.csdn.net/oscar999/article/details/102728303)

 



<iframe height="550px" src="//player.bilibili.com/player.html?aid=839144769&bvid=BV1i54y1m7cP&cid=221620892&page=3" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>



（1）引入Maven依赖：

```xml
		<dependency>
			<groupId>io.jsonwebtoken</groupId>
			<artifactId>jjwt</artifactId>
			<version>0.9.1</version>
		</dependency>
		<dependency>
			<groupId>javax.xml.bind</groupId>
			<artifactId>jaxb-api</artifactId>
			<version>2.3.0</version>
		</dependency>
		<dependency>
			<groupId>com.sun.xml.bind</groupId>
			<artifactId>jaxb-impl</artifactId>
			<version>2.3.0</version>
		</dependency>
		<dependency>
			<groupId>com.sun.xml.bind</groupId>
			<artifactId>jaxb-core</artifactId>
			<version>2.3.0</version>
		</dependency>
```



（2）工具类：

```java
```









---



（1）引入Maven依赖：

```xml
<!--引入JWT-->
<dependency>
    <groupId>com.auth0</groupId>
    <artifactId>java-jwt</artifactId>
    <version>3.10.0</version>
</dependency>
```



（2）工具类：

```java
package com.cyw.rbac01.util;

import com.auth0.jwt.JWT;
import com.auth0.jwt.JWTVerifier;
import com.auth0.jwt.algorithms.Algorithm;
import com.auth0.jwt.exceptions.JWTVerificationException;
import com.auth0.jwt.interfaces.DecodedJWT;

import java.util.Date;
import java.util.HashMap;

public class JwtUtil {
    // jwt的签名密钥（私有，不要外传）
    private static final String tokenSecret = "123";
    // 设为为5秒
    private static final Date expireTime = new Date(System.currentTimeMillis()+5000);


    // 创建JWT Token,传入jwt载荷的数据
    public static String getToken(HashMap tokenData) {
        String token = JWT.create()
                .withClaim("tokenData",tokenData)
                .withExpiresAt(expireTime)              //设置过期时间(单位：毫秒)
                .sign(Algorithm.HMAC256(tokenSecret));  //使用HMAC算法，111111作为密钥加密
        return token;
    }
    // 验证JWT Token的有效性
    public static boolean verify(String token) {
        try {
            JWTVerifier jwtVerifier = JWT.require(Algorithm.HMAC256(tokenSecret)).build();
            DecodedJWT decodedJWT = jwtVerifier.verify(token);
//            String payload = decodedJWT.getPayload();
//            System.out.println("解析后的jwt 载荷（）: ");
//            System.out.println(payload);
        } catch (JWTVerificationException exception) {
            return false;
        }
        return true;
    }

}
```



（3）工具类测试：

```java
    @Test
    public void test04(){
        
        HashMap map = new HashMap();
        map.put("username", "张三");
        map.put("age", "30");
        
        String tk = JwtUtil.getToken(map);
        
        // {"typ":"JWT","alg":"HS256"}
        System.out.println(tk);	
        
        // 负载： {"tokenData":{"age":"30","username":"张三"},"exp":1658747306}
        boolean verify = JwtUtil.verify(tk);        

        System.out.println(verify);
    }
```





