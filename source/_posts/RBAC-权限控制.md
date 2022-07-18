

<h1>RBAC-权限控制</h1>

[toc]



# 零、参考资料



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





### 1.1.2 Authorization 授权



Authorization 授权：分配权限





## 1.2、SpringBoot 整合 SpringSecurity



SQL：

```sql
create database jwt_demo;
use jwt_demo;
CREATE TABLE `sys_user` (
  `id` BIGINT(20) NOT NULL AUTO_INCREMENT COMMENT '主键',
  `user_name` VARCHAR(64) NOT NULL DEFAULT 'NULL' COMMENT '用户名',
  `nick_name` VARCHAR(64) NOT NULL DEFAULT 'NULL' COMMENT '昵称',
  `password` VARCHAR(64) NOT NULL DEFAULT 'NULL' COMMENT '密码',
  `status` CHAR(1) DEFAULT '0' COMMENT '账号状态（0正常 1停用）',
  `email` VARCHAR(64) DEFAULT NULL COMMENT '邮箱',
  `phonenumber` VARCHAR(32) DEFAULT NULL COMMENT '手机号',
  `sex` CHAR(1) DEFAULT NULL COMMENT '用户性别（0男，1女，2未知）',
  `avatar` VARCHAR(128) DEFAULT NULL COMMENT '头像',
  `user_type` CHAR(1) NOT NULL DEFAULT '1' COMMENT '用户类型（0管理员，1普通用户）',
  `create_by` BIGINT(20) DEFAULT NULL COMMENT '创建人的用户id',
  `create_time` DATETIME DEFAULT NULL COMMENT '创建时间',
  `update_by` BIGINT(20) DEFAULT NULL COMMENT '更新人',
  `update_time` DATETIME DEFAULT NULL COMMENT '更新时间',
  `del_flag` INT(11) DEFAULT '0' COMMENT '删除标志（0代表未删除，1代表已删除）',
  PRIMARY KEY (`id`)
) ENGINE=INNODB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8mb4 COMMENT='用户表'




/*Table structure for table `sys_menu` */

DROP TABLE IF EXISTS `sys_menu`;

CREATE TABLE `sys_menu` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `menu_name` varchar(64) NOT NULL DEFAULT 'NULL' COMMENT '菜单名',
  `path` varchar(200) DEFAULT NULL COMMENT '路由地址',
  `component` varchar(255) DEFAULT NULL COMMENT '组件路径',
  `visible` char(1) DEFAULT '0' COMMENT '菜单状态（0显示 1隐藏）',
  `status` char(1) DEFAULT '0' COMMENT '菜单状态（0正常 1停用）',
  `perms` varchar(100) DEFAULT NULL COMMENT '权限标识',
  `icon` varchar(100) DEFAULT '#' COMMENT '菜单图标',
  `create_by` bigint(20) DEFAULT NULL,
  `create_time` datetime DEFAULT NULL,
  `update_by` bigint(20) DEFAULT NULL,
  `update_time` datetime DEFAULT NULL,
  `del_flag` int(11) DEFAULT '0' COMMENT '是否删除（0未删除 1已删除）',
  `remark` varchar(500) DEFAULT NULL COMMENT '备注',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8mb4 COMMENT='菜单表';

/*Table structure for table `sys_role` */

DROP TABLE IF EXISTS `sys_role`;

CREATE TABLE `sys_role` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `name` varchar(128) DEFAULT NULL,
  `role_key` varchar(100) DEFAULT NULL COMMENT '角色权限字符串',
  `status` char(1) DEFAULT '0' COMMENT '角色状态（0正常 1停用）',
  `del_flag` int(1) DEFAULT '0' COMMENT 'del_flag',
  `create_by` bigint(200) DEFAULT NULL,
  `create_time` datetime DEFAULT NULL,
  `update_by` bigint(200) DEFAULT NULL,
  `update_time` datetime DEFAULT NULL,
  `remark` varchar(500) DEFAULT NULL COMMENT '备注',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8mb4 COMMENT='角色表';

/*Table structure for table `sys_role_menu` */

DROP TABLE IF EXISTS `sys_role_menu`;

CREATE TABLE `sys_role_menu` (
  `role_id` bigint(200) NOT NULL AUTO_INCREMENT COMMENT '角色ID',
  `menu_id` bigint(200) NOT NULL DEFAULT '0' COMMENT '菜单id',
  PRIMARY KEY (`role_id`,`menu_id`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8mb4;

/*Table structure for table `sys_user` */

DROP TABLE IF EXISTS `sys_user`;

CREATE TABLE `sys_user` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '主键',
  `user_name` varchar(64) NOT NULL DEFAULT 'NULL' COMMENT '用户名',
  `nick_name` varchar(64) NOT NULL DEFAULT 'NULL' COMMENT '昵称',
  `password` varchar(64) NOT NULL DEFAULT 'NULL' COMMENT '密码',
  `status` char(1) DEFAULT '0' COMMENT '账号状态（0正常 1停用）',
  `email` varchar(64) DEFAULT NULL COMMENT '邮箱',
  `phonenumber` varchar(32) DEFAULT NULL COMMENT '手机号',
  `sex` char(1) DEFAULT NULL COMMENT '用户性别（0男，1女，2未知）',
  `avatar` varchar(128) DEFAULT NULL COMMENT '头像',
  `user_type` char(1) NOT NULL DEFAULT '1' COMMENT '用户类型（0管理员，1普通用户）',
  `create_by` bigint(20) DEFAULT NULL COMMENT '创建人的用户id',
  `create_time` datetime DEFAULT NULL COMMENT '创建时间',
  `update_by` bigint(20) DEFAULT NULL COMMENT '更新人',
  `update_time` datetime DEFAULT NULL COMMENT '更新时间',
  `del_flag` int(11) DEFAULT '0' COMMENT '删除标志（0代表未删除，1代表已删除）',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8mb4 COMMENT='用户表';

/*Table structure for table `sys_user_role` */

DROP TABLE IF EXISTS `sys_user_role`;

CREATE TABLE `sys_user_role` (
  `user_id` bigint(200) NOT NULL AUTO_INCREMENT COMMENT '用户id',
  `role_id` bigint(200) NOT NULL DEFAULT '0' COMMENT '角色id',
  PRIMARY KEY (`user_id`,`role_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;



CREATE TABLE persistent_logins (
	username VARCHAR ( 64 ) NOT NULL,
	series VARCHAR ( 64 ) PRIMARY KEY,
	token VARCHAR ( 64 ) NOT NULL,
last_used TIMESTAMP NOT NULL 
);
```









### 1.2.1、整合

（1）POM.xml中导入依赖：

```xml
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-security</artifactId>
		</dependency>
```



（2）在`resources/static`目录下创建`index.html`：

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



（3）在控制台找到自动生成的一个密码，用户名为`user`,直接在网页上登录（`http://localhost:8080/logout`可以退出登录）。





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







## 1.6、授权-配置类



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

