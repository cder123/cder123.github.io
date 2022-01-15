---
title: Spring4-入门
tag: Java
categories:
  - [后端,Java,SSM]

---

# Spring4

[toc]



## 0、参考资料

书：

-   SSM从零开始学
-   Offer来了



## 1、Spring基础



### 1.1、Spring 概述



#### 1.1.1、什么是 Spring

Spring 是一种JavaEE 开发一站式的解决方案。以 `IOC` 和`AOP`为核心，该框架提供的功能贯穿*表示层、业务层、持久层*。



<font style="color:red;">Spring框架的特点：</font>

>   -   **轻量**：核心Jar包只有`1.4 MB`，占用的系统资源（CPU和内存）少。
>   -   **框架灵活**：可以按需引入模块来实现不同的功能。
>   -   **面向容器**：（以XML或注解的形式）配置化实现对象，管理对象的生命周期。
>   -   **控制反转（IOC）**：创建对象时，若需要依赖其他对象，则自动传入其依赖的对象。（目的：解耦）
>   -   **面向切面（AOP）**：将 *业务逻辑* 和 *系统逻辑* 分离，提高系统的内聚。





<font style="color:red;">Spring的组成架构：</font>



![Spring-组成架构图](https://www.runoob.com/wp-content/uploads/2015/07/673670c9a34075831373b711cb8f21b7.png)



spring 框架的核心模块包括：

>   -   核心容器层：beans、core、context、spEl（核心容器的运行还需要commons-logging包）
>   -   数据访问层
>   -   Web应用层





Jar包介绍：

>   主要为三类：
>
>   -   class字节码文件的压缩包
>   -   API文档的压缩包
>   -   源码压缩包





---





#### 1.1.2、 IoC 与 DI

IoC（Intervision of Control）也叫 *控制反转*。

DI（dependency Inject）也叫 *依赖注入*。

`IoC`是由Spring的 IoC容器来管理Java对象的生命周期，把控制权交给IoC容器，直接从IoC容器中获取对象，而不需要像传统的面向对象编程那样由用户手动new出对象。IoC理论实际上是在借助第三方的 IoC容器实现多个对象之间的解耦。

`DI`是在IoC容器创建对象的过程中，若A对象依赖B对象，则由 IoC容器主动创建B对象并传给（注入）A对象。



`IoC` 和`DI`是从不同的角度对同一件事情的描述。IoC是从容器的角度，DI 是从APP的角度。



<font style="color:red">使用 `IoC/DI` 的好处:</font>

>   -   可维护性好
>   -   专注业务
>   -   可复用性好
>   -   创建对象具有热拔插性







---







### 1.2、Spring 的核心容器

由上面`1.1.1小节`的`Spring组成架构图`可以看出，Spring的核心容器的模块分为：

>   -   beans：工厂模式，管理对象
>   -   core：IoC、AOP
>   -   context：在beans和core的基础上构建，继承了beans模块，添加了国际化、事件传播等功能
>   -   spEL：扩展JSP的EL表达式



Spring框架的核心容器中，主要的两个包为：`org.springframework.beans.factory`和`org.springframework.context`。其中，beans 包 中主要的接口为`BeanFactory`，负责管理Java类；context 包 中主要的接口为`ApplicationFactory`。



<font style="color:red">Spring 中 的 `IoC`容器的组成部分主要有：</font>

>   -   `对象`
>
>   -   配置文件`applicationContext.xml`：bean的id、类、属性、属性值。xml配置文件名可以随意。
>   -   `BeanFactory 接口`及相关组件：工厂模式（Bean容器模式），负责读取配置文件、管理对象的生成和加载。维护Bean对象之间的依赖关系，负责Bean的生命周期。
>   -   `ApplicationContext 接口`及其相关类：相比BeanFactory接口来说，该方式可以更方便的访问资源、支持国际化消息、提供文字消息解释方法等。



<font style="color:red">`BeanFactory 接口`的实现类：</font>

>   `org.springframework.beans.factory.xml.XmlBeanFactory`：需要传入一个Bean配置文件文件（applicationContext.xml）的输入流，然后调用`getBean(String id) `或`getBean(String id，Class requiredType)`来创建相应的对象。
>
>   





<font style="color:red">`ApplicationContext 接口`的实现类</font>

>   -   `FileSystemXmlApplicationContext`：从**文件系统**中的XML配置文件加载（以盘符为根目录），只能在指定路径中查询配置文件
>   -   `ClassPathXmlApplicationContext`：从**类路径**中的XML配置文件加载（以src为根目录），可以在整个类路径中查询配置文件
>   -   `XmlWebApplicationContext`：从**Web系统**中的XML配置文件加载





#### 1.2.1、 Bean的装配





<font style="color:red">`IoC/DI`的实现方式（Bean的装配）：</font>

>   -   类的`setter方法`（常用）：xml中bean节点的`property`子标签，必须要有无参构造和setter方法
>   -   类的`构造方法`：xml中bean节点的`constructor-arg`子标签
>   -   注解装配：`@Repository`（dao层）、`@Service`、`@Controller`、`@Autowired`
>   -   xml+bean标签的自动装配属性



<font style="color:red">Bean标签的常用属性和子标签：</font>

>   <font style="color:red">bean标签常用的属性：</font>
>
>   -   id:    唯一标识
>      -   name:  别名，可以有多个，多个别名之间用逗号或分号分割
>      -   class: javabean 的全类名（包名.类名）
>      -   scope: 作用域，scope属性的7个取值：
>             -   singleton :	单例模式，每次访问的都是同一个bean对象
>             -   prototype :	原型，每次访问都创建一个新的bean对象
>             -   request	:	请求域
>             -   session
>             -   globalSession
>          -   application
>          -   websocket
>
>    
>
>   <font style="color:red">bean标签常用的子标签：</font>
>
>   -   property标签
>   -   constructor-arg标签 
>
>    
>
>   <font style="color:red">property标签 和 constructor-arg标签 的常用属性：</font>
>
>   -   id：标识
>   -   name：标识
>   -   value：注入java自带的数据类型
>   -   ref：注入用户定义的JavaBean的全类名或bean标签的id
>
>    
>
>   <font style="color:red">bean的子标签的常用子标签：</font>
>
>   -   list : 配合value标签使用
>   -   set   
>      -   map
>   			- entry







<font style="color:red">Bean的装配-XML（调用setter方法装配）：</font>

-   引入Spring 的 beans约束
-   在配置文件中写bean标签
-   利用ApplicationContext对象的getBean方法获取对象
-   调用bean对象的方法

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xsi:schemaLocation="http://www.springframework.org/schema/beans
 http://www.springframework.org/schema/beans/spring-beans.xsd
 ">
    
 
	<!-- 该bean的属性都是java自带的属性，不需要依赖注入 -->
 	<bean id="dao" name="userdao;UserDao" class="com.ssm.ioc.dao.impl.UserDaoImpl" scope="singleton"/>
    
    
    <!-- 该bean的属性中有javaBean,需要依赖注入，注入方式：property子标签 -->
    <!-- property子标签的name属性表示按照bean的对象名来装配，需要换成value属性 -->
    <!-- property子标签的ref属性表示javabean的id,若是java自带的数据类型，需要换成value属性 -->
 	<bean id="service" class="com.ssm.ioc.service.impl.UserServiceImpl">
        <property name="id" value="1" />
 		<property name="dao" ref="dao" />
 	</bean> 	
 
 	
</beans>
```





<font style="color:red">Bean的装配-XML（调用构造方法装配）：</font>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xsi:schemaLocation="http://www.springframework.org/schema/beans
 http://www.springframework.org/schema/beans/spring-beans.xsd
 ">
    
    <!--
 		构造方法的方式装配Bean对象有2种写法：【任选其一】
			- 利用参数的索引传参
			- 利用参数的形参名传参
	-->
    <bean id="dao" class="com.ssm.ioc.dao.impl.UserDaoImpl" />
 	<bean id="service" class="com.ssm.ioc.service.impl.UserServiceImpl">
 		<constructor-arg index="0" value="1"/>
 		<constructor-arg name="dao" ref="dao" />
 	</bean>
 
	
 	
</beans>
```





<font style="color:red">Bean的装配-注解：</font>

-   利用注解的形式装配必须结合aop的jar包来完成
-   导入xml的约束
-   配置文件的beans标签中：`<context:annotation-config/>`
-   配置bean标签（省略property和constructor-arg子标签）
-   在Java类上使用注解
-   创建ApplicationContext对象并获取Bean对象
-   调用Bean对象的方法

>   <font style="color:red">Bean的装配-注解方式注意事项：</font>
>
>   -   自动装配-需要引入的约束：
>       -   `xmlns:context="http://www.springframework.org/schema/context"`
>       -   `http://www.springframework.org/schema/context`
>       -   `http://www.springframework.org/schema/context/spring-context.xsd`
>
>     
>
>   -   导入某个包的所有Bean
>       -   `<context: component-scan base-package="包名"/>`
>
>   
>
>   -   自动装配-开启自动装配设置：
>       -   `<context:annotation-config/>`
>
>    
>
>   -   自动装配-Java类上设置注解：
>       -   标识分层：`@Repository(beanId)`、`@Service(beanId)`、`@Controller(beanId)`
>       -   标识自动装配：`@Autowired 与 @Qualifier`、`@Resource(name="beanId")`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xmlns:context="http://www.springframework.org/schema/context"
 xsi:schemaLocation="http://www.springframework.org/schema/beans
 http://www.springframework.org/schema/beans/spring-beans.xsd
 http://www.springframework.org/schema/context
  http://www.springframework.org/schema/context/spring-context.xsd
 ">
 
	<!--
 		自动装配-需要引入的约束：
			 - xmlns:context="http://www.springframework.org/schema/context"
 			 - http://www.springframework.org/schema/context
  			 - http://www.springframework.org/schema/context/spring-context.xsd


		自动装配-开启自动装配设置：
			- <context:annotation-config/>


		自动装配-Java类上设置注解：
			- 标识分层：
				- @Repository(beanId)
				- @Service(beanId)
				- @Controller(beanId)
			- 标识自动装配：
				- @Autowired
				- @Resource(name="beanId")
	--> 
    
 	<context:annotation-config/>
 	<bean id="dao" class="com.ssm.ioc.dao.impl.UserDaoImpl" />
 	<bean id="service" class="com.ssm.ioc.service.impl.UserServiceImpl" /> 		
 	
 	
 	
 	
</beans>
```

Service层：

```java
package com.ssm.ioc.service.impl;

import javax.annotation.Resource;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.ssm.ioc.dao.UserDao;
import com.ssm.ioc.service.UserService;

@Service("service")
public class UserServiceImpl implements UserService{
	private int id;
	@Autowired
	private UserDao dao;
	
	@Override
	public void login() {
		// TODO Auto-generated method stub
		dao.login();
		System.out.println("userService: ...login");
	}
	public UserServiceImpl() {
		super();
		// TODO Auto-generated constructor stub
	}
	public UserServiceImpl(int id,UserDao dao) {
		super();
		this.id = id;
		this.dao = dao;
	}
	public UserDao getDao() {
		return dao;
	}
	public void setDao(UserDao dao) {
		this.dao = dao;
	}
	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	@Override
	public String toString() {
		return "UserServiceImpl [id=" + id + ", dao=" + dao + "]";
	}
}

```



<font style="color:red">注解+xml的装配-注解（利用bean标签的autowire属性）：</font>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xmlns:context="http://www.springframework.org/schema/context"
 xsi:schemaLocation="http://www.springframework.org/schema/beans
 http://www.springframework.org/schema/beans/spring-beans.xsd
 http://www.springframework.org/schema/context
  http://www.springframework.org/schema/context/spring-context.xsd
 ">
 		
    <!--
		autowire属性的取值：
			- default ：由beans标签的default-autowire属性确定
			- byName  ：按name属性值装配
			- byType  : 按数据类型装配
			- constructor ：按构造函数的参数类型，进行byName的自动装配
			- no : 不自动装配
	 -->
 		 
 <bean id="dao" class="com.ssm.ioc.dao.impl.UserDaoImpl" />
 <bean id="service" class="com.ssm.ioc.service.impl.UserServiceImpl" autowire="byName" />
 	
 	
 	
</beans>
```





---





### 1.3、AOP 概述



#### 1.3.1  什么是AOP

AOP也叫 `面向切面编程`，是对`OOP`（面向对象编程）的补充。AOP是为了解决实现某个功能时，相同的代码分散到各个方法中，采用`横向抽取机制`。

AOP常用的两个框架：

-   `Spring AOP`：纯Java实现，不需要额外的编译器
-   `AspcetJ`：需要有专门的编译器





#### 1.3.2 AOP术语

-   Aspect：切面，已经在bean标签中指定过的Java类，
-   Joinpoint：连接点，方法的调用。
-   Pointcut：切点，类名、方法名、满足条件的规则。
-   Advice：通知增强处理，切面类的方法。
-   TargetObject：目标对象（增强对象），所有被通知的对象。
-   Proxy：代理，通知应用到对象后，被动态创建出的对象。
-   Weaving：织入，切面代码插入到目标对象后，生成的代理对象的过程。





### 1.4、AspectJ 框架实现AOP



<font style="color:red;">AspectJ 框架实现AOP的两种方式：</font>

-   XML声明
-   注解声明





#### 1.4.1 XML声明AOP

XML声明：切面（类）、切入点（方法调用）、通知（规则）



XML声明AOP时，所有的 切面（类）、切入点（方法调用）、通知（规则）都必须定义在`<aop:config>`标签内，`beans`标签可以有多个`<aop:config>`标签，每个`<aop:config>`标签内可以有多个切面（`<aop:aspect>`）、切点（`<aop:cutpoint>`）、通知（`<aop:advisor>`）











#### 1.4.2 注解声明AOP

