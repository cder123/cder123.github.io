---
title: Spring-入门（IoC-简化版）
tag: Java
categories:
  - [后端,Java,SSM]
---



# Spring-入门（IoC-简化版）

[toc]



## 0、资料

-   [Spring-官网（只有文档）](https://spring.io/)
-   [Spring-下载页](https://repo.spring.io/ui/packages#/home)
-   [Spring5-视频_尚硅谷](https://www.bilibili.com/video/BV1Vf4y127N5?p=2)





笔记：

>   [Spring框架-笔记](https://blog.csdn.net/weixin_47872288/article/details/117921644?spm=1001.2014.3001.5501)
>
>   1、https://blog.csdn.net/weixin_45496190/article/details/107059038
>   		 2、https://blog.csdn.net/weixin_45496190/article/details/107067200
>   		 3、https://blog.csdn.net/weixin_45496190/article/details/107071204
>   		 4、https://blog.csdn.net/weixin_45496190/article/details/107082732
>   		 5、https://blog.csdn.net/weixin_45496190/article/details/107092107





---





## 1、下载 Spring5

[下载教程](https://blog.csdn.net/sgliuxiu/article/details/104629795)

[下载链接](https://repo.spring.io/ui/repos/tree/General/release%2Forg%2Fspringframework%2Fspring%2F5.3.9%2Fspring-5.3.9-dist.zip)



>   Maven依赖：(导入以下1个个依赖，会自动导入spring所需的依赖)

```maven
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.3.9</version>
        </dependency>
    </dependencies>
```





---



## 2、入门案例（IOC）



### 2.1 案例1：(实体类没有成员变量)

案例目标：利用spring创建对象、执行方法。



步骤：

>   1.   下载Spring的Jar包（案例中使用jar包：commons.log + spring bean + core + conext + expression）
>   2.   创建1个普通的 java项目。
>   3.   项目中添加 jar 包依赖。
>   4.   创建JavaBean：`User`类
>   5.   创建 Spring的配置文件（idea创建的springXML自动生成约束）：`bean1.xml`
>   6.   测试Spring效果（Junit）



User类：

```java
package com.cyw;

public class User {
    public void add(){
        System.out.println("add方法执行了。。。");
    }
}

```



bean1.xml：(resources资源目录下)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    
    
    
    <!-- bean标签中，
 			id：唯一标识，随便取；
            class：JavaBean 的全类名
    -->
        <bean id="userbean" class="com.cyw.User"></bean>
    
</beans>
```



测试效果：

```java
package com.cyw;

import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class userTest {
    @Test
    public void addTest() {
        //加载资源
        ApplicationContext context = new ClassPathXmlApplicationContext("bean1.xml");
        
        // 获取对象（第1个参数为配置文件中bean标签的id，第2个参数为JavaBean的字节码）
        User user = context.getBean("userbean", User.class);
        user.add();

    }
}

```





### 2.2 案例2：(实体类有成员变量)



实体类：

```java
package com.cyw.entity;

public class User {
    private String myName;

    public String getMyName() {
        return myName;
    }

    public void setMyName(String myName) {
        this.myName = myName;
    }

    public void add(){
        System.out.println("add方法执行了。。。");
    }
}

```



beans.xml：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    
    <!-- id是唯一标识，随便起；class是实体类的全类名 -->
    <bean id="user" class="com.cyw.entity.User">
        <!-- 
			name为实体类的成员变量名，
			value为变量值
			（value也可以是ref,ref的值为另外一个实体类的bean标签的id） 
小结：java自带的类型用value，用户自定义的类型用ref
		-->
        <property name="myName" value="张三"></property>
    </bean>
    
</beans>
```







---





## 3、IOC 容器



### 3.1 IOC 简介

1、什么是 IOC ？

IOC也叫控制反转，是面向对象编程中的一种设计原则。在Spring中，IOC指的是==我们把对象的创建和调用交给Spring来管理==。



2、使用 IOC 的目的：解耦



3、IOC的底层：xml 解析、工厂、反射

>   -   以下操作均在`工厂类`中完成。
>   -   XML解析Spring的配置文件，得到 JavaBean的全类名
>   -   利用 Class.ForName(全类名)得到 1个 Class 对象
>   -   利用 Class 对象的 newInstance( ) 方法创建对象，并返回。



4、Spring的 IOC接口

>   -   BeanFactory接口（只能Spring内部使用）：加载配置文件时，不创建对象（使用时创建）。
>   -   ApplicationContext接口（程序员使用）：BeanFactory接口的子接口。加载配置时，就创建对象。

```java
// ApplicationContext接口的使用


	// 相对路径获取配置文件中的context
	ApplicationContext context = new ClassPathXmlApplicationContext("bean1.xml");

	// 绝对路径获取配置文件中的context
	ApplicationContext context1 = new FileSystemXmlApplicationContext("resources/beans.xml");

```





### 3.2 设置 实体类 的成员变量（依赖注入）



依赖注入：

-   依赖：bean对象的创建过程依赖于容器
-   注入：bean对象的成员变量的赋值由容器来完成（值先写在配置文件中，由容器读取配置然后赋值）







以下3种方式在beans.xml中设置 ==实体类== 的成员变量：（1种setter、2种构造器）



>   setter方法（property标签）-设置 实体类 的成员变量【重点】

```xml
    <bean id="user" class="com.cyw.entity.User">
        <!-- name为实体类的成员变量名，value为变量值
 			name标识的成员变量的类型为普通变量时
		-->	
        	<property name="myName" value="张三"/>
        
        <!--
 			name标识的成员变量的类型为自定义的类型时（Address类）
		-->	
        	<property name="myAddress" ref="Address"/>
        
        <!--
 			name标识的成员变量的类型为 array数组时
		-->	
            <property name="mybooks" >
                <array>
                    <value>西游记</value>
                    <value>三国演义</value>
                     <value>红楼梦</value>
                </array>
            </property>
        
        
        <!--
 			name标识的成员变量的类型为 list集合时
		-->	
            <property name="myhobby" >
                <list>
                    <value>唱歌</value>
                    <value>跳舞</value>
                    <value>吃饭</value>
                </list>
            </property>
        
        
        <!--
 			name标识的成员变量的类型为 map集合时
		-->	
            <property name="myCard" >
                <map>
                    <entry key="cid" value="001"/>
                    <entry key="cname" value="张三"/>
                    <entry key="年龄" value="32"/>
                </map>
            </property>
        
        
        <!--
 			name标识的成员变量的类型为 set集合时
		-->	
            <property name="friends" >
                <set>
                    <value>唱歌</value>
                    <value>跳舞</value>
                    <value>吃饭</value>
                </set>
            </property>
        
        
        <!--
 			name标识的成员变量的类型为 null时
		-->	
            <property name="email" >
               <null/>
            </property>
        
        
        <!--
 			name标识的成员变量的类型为 properties配置文件时
		-->	
            <property name="myProp" >
                <props>
                	<prop key="姓名">张三</prop>
                    <prop key="年龄">24</prop>
                </props>
            </property>

    </bean>
```





>  构造器-设置 实体类 的成员变量（按：参数名）

```xml
    <bean id="user" class="com.cyw.entity.User">
      	<constructor-arg name="myName" value="王五"/>
        <constructor-arg name="age" value="22"/>
    </bean>
```





>  构造器-设置 实体类 的成员变量（按：参数下标）

```xml
    <bean id="user" class="com.cyw.entity.User">
         <!-- index为实体类的构造函数的参数下标，value为参数值	-->	
            <constructor-arg index="0" value="李四"/>
            <constructor-arg index="1" value="26"/>
    </bean>
```





>  构造器-设置 实体类 的成员变量（按：参数类型）【不推荐使用】

```xml
    <bean id="user" class="com.cyw.entity.User">
        <constructor-arg type="java.lang.String" value="王五"/>
        <constructor-arg type="int" value="32"/>
    </bean>
```





---





## 4、Spring 的配置



### 4.1 实体类的别名



填写的位置：`beans.xml`文件



取别名的2种方式：

-   bean 标签的 name属性
-   alias 标签



```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">


<!--
	（第一种取别名的方式）
	bean标签的 name属性来取别名（name可以取多个别名，且别名之间用符号分隔【分隔符：空格、逗号、分号】）
		- 以下示例中的 U和Us都是别名
-->
    <bean id="user" class="com.cyw.entity.User" name="U,Us">
        <constructor-arg name="myName" value="王五"/>
        <constructor-arg name="age" value="22"/>
    </bean>
<!-- 
	（第二种取别名的方式）
	alias标签：取别名
		- name：上面的bean标签中的实体类的id
		- alias: 用户自己取的别名，取完后，凡是使用实体类id的地方都可以用别名来替换
-->
    <alias name="user" alias="U"/>

</beans>
```



### 4.2 导入多个配置文件

使用场景：团队开发时使用

前提：存在多个实体类的xml配置文件，想要导入到1个配置文件中，代码只加载汇总后的配置文件

发生的位置：`beans.xml`（正规的名字是：`ApplicationContext.xml`）

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">



    <bean id="user" class="com.cyw.entity.User" name="U,U2">
        <constructor-arg name="myName" value="王五"/>
        <constructor-arg name="age" value="22"/>
    </bean>
    
<!--
	导入配置文件 
-->
    <import resource="bean1.xml"/>
 	<import resource="bean2.xml"/>
    <import resource="bean3.xml"/>
</beans>
```





---



## 5、p命名空间、c命名空间的依赖注入





p命名空间、c命名空间是为了简化 3.2 小节的依赖注入。



### 5.1 p命名空间



p命名空间的作用：（简化 set 方式的依赖注入）将bean标签的子标签property 以 bean标签属性的形式出现。



使用 p命名空间 **前**：

```xml
<bean id="myDataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
    <!-- results in a setDriverClassName(String) call -->
    <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
    <property name="url" value="jdbc:mysql://localhost:3306/mydb"/>
    <property name="username" value="root"/>
    <property name="password" value="root"/>
</bean>
```





>p命名空间的使用步骤：
>
>-   导入命名空间（`   xmlns:p="http://www.springframework.org/schema/p"`）
>-   在 bean 标签中使用（如：` p:password="root"`）



使用 p命名空间 **后**：

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:p="http://www.springframework.org/schema/p"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="myDataSource" class="org.apache.commons.dbcp.BasicDataSource"
        destroy-method="close"
        p:driverClassName="com.mysql.jdbc.Driver"
        p:url="jdbc:mysql://localhost:3306/mydb"
        p:username="root"
        p:password="root"/>

</beans>
```







### 5.2 c命名空间

c命名空间的作用：（简化 构造器 方式的依赖注入）



c命名空间的使用步骤：

-   前提：在实体类中已经定义好了 有参构造函数。

-   导入命名空间（`     xmlns:c="http://www.springframework.org/schema/c"`）
-   在 bean 标签中使用（如：` c:email="123"`，其中的email为构造函数的参数名）

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:c="http://www.springframework.org/schema/c"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    
    <bean id="thingOne" class="x.y.ThingTwo"/>

    <!-- 传统方式-->
    <bean id="thingOne" class="x.y.ThingOne">
        <constructor-arg ref="thingTwo"/>        
        <constructor-arg value="[emailprotected]"/>
    </bean>

    <!-- c命名空间方式 -->
    <bean id="thingOne" class="x.y.ThingOne" c:thingTwo-ref="thingTwo"  c:email="[emailprotected]"/>

</beans>
```









## 6、Bean的作用域



| Scope                                                        | Description                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [singleton](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/core.html#beans-factory-scopes-singleton) | **(默认)**将每个 Spring IoC 容器的单个 bean 定义范围限定为单个对象实例。 |
| [prototype](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/core.html#beans-factory-scopes-prototype) | 将单个 bean 定义的作用域限定为任意数量的对象实例。           |
| [request](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/core.html#beans-factory-scopes-request) | 将单个 bean 定义的范围限定为单个 HTTP 请求的生命周期。也就是说，每个 HTTP 请求都有一个在单个 bean 定义后面创建的 bean 实例。仅在可感知网络的 Spring `ApplicationContext`中有效。 |
| [session](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/core.html#beans-factory-scopes-session) | 将单个 bean 定义的范围限定为 HTTP `Session`的生命周期。仅在可感知网络的 Spring `ApplicationContext`上下文中有效。 |
| [application](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/core.html#beans-factory-scopes-application) | 将单个 bean 定义的范围限定为`ServletContext`的生命周期。仅在可感知网络的 Spring `ApplicationContext`上下文中有效。 |
| [websocket](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/web.html#websocket-stomp-websocket-scope) | 将单个 bean 定义的范围限定为`WebSocket`的生命周期。仅在可感知网络的 Spring `ApplicationContext`上下文中有效。 |



使用示例（` bean标签的 scope属性`）：`resources/beans.xml`

```xml
<bean id="user" class="com.cyw.User" scope="singleton"/>

<bean id="user" class="com.cyw.User" scope="prototype"/>

<bean id="user" class="com.cyw.User" scope="request"/>

<bean id="user" class="com.cyw.User" scope="session"/>

<bean id="user" class="com.cyw.User" scope="application"/>

<bean id="user" class="com.cyw.User" scope="websocket"/>
```





## 7、自动装配



自动装配：spring 在context 中自动查找属性，并给 bean 的属性 赋值。



>   Spring 的 三种装配方式：

1.   XML 中手动配置
2.   java注解手动配置
3.   自动装配（**重要**）



>   自动装配的模式（`bean标签的 autowire 属性的取值`）：

| Mode          | Explanation                                                  |
| ------------- | ------------------------------------------------------------ |
| `no`          | (默认)无自动装配。 Bean 引用必须由`ref`元素定义。对于大型部署，建议不要更改默认设置，因为明确指定协作者可以提供更好的控制和清晰度。在某种程度上，它记录了系统的结构。 |
| `byName`      | 按属性名称自动布线。 Spring 寻找与需要自动装配的属性同名的 bean。例如，如果一个 bean 定义被设置为` byName`自动装配，并且包含一个`master`属性(即，它具有`setMaster(..)`方法)，那么 Spring 将查找一个名为`master`的 bean 定义并使用它来设置属性。 |
| `byType`      | 如果容器中恰好存在一个该属性类型的 bean，则使该属性自动装配。如果存在多个错误，则会引发致命异常，这表明您可能不对该 bean 使用`byType`自动装配。如果没有匹配的 bean，则什么也不会发生(未设置该属性)。 |
| `constructor` | 类似于`byType`，但适用于构造函数参数。如果容器中不存在构造函数参数类型的一个 bean，则将引发致命错误。 |







### 7.1 XML自动装配

（`bean 标签的 autowire属性`）

```xml
<bean id="dog" class="com.cyw.Dog"/>

<bean id="user" class="com.cyw.User"  autowire="byName">
	<property name="name" value="大白">
</bean>
```



### 7.1 注解自动装配



