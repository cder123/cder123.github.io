---
title: Spring4-入门
tag: Java
categories:
  - [后端,Java,SSM]

---

# Spring4-入门

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

`IoC`是由Spring的 IoC容器来管理Java对象的生命周期，把控制权交给IoC容器，直接从IoC容器中获取对象，而不需要像传统的面向对象编程那样由用户手动new出对象。IoC理论实际上是在借助第三方的 IoC容器实现多个对象之间的解耦。【相当于：先整体，再局部】

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
>   -   spEL：Spring表达式（类似El表达式的写法）



Spring框架的核心容器中，主要的两个包为：`org.springframework.beans.factory`和`org.springframework.context`。其中，beans 包 中主要的接口为`BeanFactory`，负责管理Java类；context 包 中主要的接口为`ApplicationFactory`。



本质上IoC容器就是一个工厂。使用了`XML解析、工厂模式、反射`。



<font style="color:red">Spring 中 的 `IoC`容器的组成部分主要有：</font>

>   -   `对象`
>
>   -   配置文件`applicationContext.xml`：bean的id、类、属性、属性值。xml配置文件名可以随意。
>   -   `BeanFactory 接口`及相关组件：工厂模式（Bean容器模式），负责读取配置文件、管理对象的生成和加载。维护Bean对象之间的依赖关系，负责Bean的生命周期。加载配置文件时，不会创建对象，使用对象时，才创建对象【Spring使用】
>   -   `ApplicationContext 接口`及其相关类：相比BeanFactory接口来说，该方式可以更方便的访问资源、支持国际化消息、提供文字消息解释方法等。加载配置文件时，创建对象【程序员使用】



<font style="color:red">`BeanFactory 接口`的实现类：</font>

>   `org.springframework.beans.factory.xml.XmlBeanFactory`：需要传入一个Bean配置文件文件（applicationContext.xml）的输入流，然后调用`getBean(String id) `或`getBean(String id，Class requiredType)`来创建相应的对象。
>
>   





<font style="color:red">`ApplicationContext 接口`的实现类</font>

>   -   `FileSystemXmlApplicationContext`：从**文件系统**中的XML配置文件加载（以盘符为根目录），只能在指定路径中查询配置文件
>   -   `ClassPathXmlApplicationContext`：从**类路径**中的XML配置文件加载（以src为根目录），可以在整个类路径中查询配置文件
>   -   `XmlWebApplicationContext`：从**Web系统**中的XML配置文件加载





## 2、Bean



### 2.1、 Bean的装配-概述





<font style="color:red">`IoC/DI`的实现方式（Bean的装配）：</font>

>   两种方式【XML和注解】
>
>   -   类的`setter方法`（常用）：xml中bean节点的`property`子标签，必须要有无参构造和setter方法
>   -   类的`构造方法`：xml中bean节点的`constructor-arg`子标签
>   -   注解装配（必须导入AOP的jar包））：`@Repository`（dao层）、`@Service`、`@Controller`、`@Autowired`
>   -   xml中bean标签的自动装配属性





<font style="color:red">`IoC/DI`的实现方式（Bean的装配）-简写-命名空间：</font>

>   -   p：相当于property标签
>   -   c：相当于constructor-arg标签
>
>    
>
>   命名空间的方式与子标签的方式的区别：
>
>   -   命名空间的方式需要引入xml的命名空间
>   -   命名空间的方式是作为bean标签的属性来写的，子标签是作为bean标签的内部节点写的







<font style="color:red">Bean标签的常用属性和子标签：</font>

>   <font style="color:red">bean标签常用的属性：</font>
>
>   -   id:    唯一标识【javaBean的属性名，setter方法名去掉set，第一个字母改小写】
>   -   name:  别名，可以有多个，多个别名之间用逗号或分号分割
>   -   class: javabean 的全类名（包名.类名）
>   -   scope: 作用域，scope属性的7个取值：
>          -   singleton :	单例模式，每次访问的都是同一个bean对象
>          -   prototype :	原型，每次用getBean方法来访问该JavaBean都创建一个新的bean对象
>          -   request	:	请求域
>          -   session
>          -   globalSession：portlet环境中
>       -   application
>       -   websocket
>   -   init-method：指定JavaBean的初始化方法的方法名
>   -   destroy-method：指定JavaBean的销毁方法的方法名
>   -   factory-method：利用工厂类的方法获取对象【class属性配为工厂类的全类名，factory-method指定工厂类的静态方法】（代替bean标签的无参装配的方式）
>   -   factory-bean：动态工厂的id属性，配合factory-method属性使用
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
>   -   name：标识，必须与Java类中的**成员变量的名字一致**
>   -   value：注入java自带的数据类型的数据
>   -   ref：注入用户定义的JavaBean的**全类名**或bean标签的id
>
>   
>
>   <font style="color:red">bean的子标签property的常用子标签【集合数据类型】：</font>
>
>   -   list : 配合value标签使用
>   -   set   
>   -   map：配合entry子标签使用（entry标签的属性：key 和 value-ref）
>   -   props：配合prop子标签使用（key属性；值放标签内）
>   -   null
>   -   value
>
>    
>
>   <font style="color:red">property标签的value为特殊值时：</font>
>
>   -   转义字符
>   -   `<![CDATA[ 特殊字符 ]]>`
>
>   



### 2.2、 Bean的XML装配

<font style="color:red">Bean的装配-XML的方式（调用setter方法装配）：</font>

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





<font style="color:red">Bean的装配-XML的方式（调用构造方法装配）：</font>

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







### 2.3、 Bean的注解装配

<font style="color:red">Bean的自动装配-注解的方式：</font>

-   使用注解的形式装配，必须结合`AOP的 jar包`来完成
-   XML配置文件中导入`名称空间`和`约束`
-   配置文件的beans标签中：`<context:annotation-config/>`
-   配置bean标签（省略property和constructor-arg子标签）**或者**开启组件扫描
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
>       -   标识分层：`@Repository(beanId)`、`@Service(beanId)`、`@Controller(beanId)`、`@Component`【注解的参数可省略，省略后默认为*首字母小写的类名*】
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
			- 标识分层：【创建对象的注解】
				-- @Repository(beanId)
				-- @Service(beanId)
				-- @Controller(beanId)
			- 标识自动装配：【自动装配的注解】
				-- @Autowired	// 根据 类型 来注入
				-- @Qualifier	// 根据 name属性 来注入【配合 @Autowired 使用】
				-- @Resource(name="beanId") / 根据 name属性类型或 来注入
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



<font style="color:red">Bean的自动装配-XML的方式：</font>

-   （利用bean标签的autowire属性，必须导入aop的jar包）

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
			- byName  ：按bean标签的name属性值装配
			- byType  : 按数据类型装配
			- constructor ：按构造函数的参数类型，进行byName的自动装配
			- no : 不自动装配
	 -->
 		 
 <bean id="dao" class="com.ssm.ioc.dao.impl.UserDaoImpl" />
 <bean id="service" class="com.ssm.ioc.service.impl.UserServiceImpl" autowire="byName" />
 	
 	
 	
</beans>
```





### 2.4、 Bean-命名空间装配



<font style="color:red">Bean的装配-p命名空间（调用setter方法装配）：</font>

-   引入命名空间：` xmlns:p="http://www.springframework.org/schema/p"`
-   编写bean配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:p="http://www.springframework.org/schema/p"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd" >



<!-- 第一种：setter【property子标签】+无参构造 -->
	   <!--  
		<bean id="userDao" class="com.ssm.ioc.dao.UserDaoImpl" />	    
	    <bean id="userService" class="com.ssm.ioc.service.UserServiceImpl" >
	   		<property name="idx" value="1"/>
	   		<property name="dao" ref="userDao"/>
	    </bean> -->
  
<!-- 第2种：setter【p命名空间】+无参构造 -->
    	<bean id="userDao" class="com.ssm.ioc.dao.UserDaoImpl" /> 
	    <bean id="userService" 
	    	class="com.ssm.ioc.service.UserServiceImpl" 
	    	p:idx="1" 
	    	p:dao-ref="userDao" 
       />
	
</beans>
```







<font style="color:red">Bean的装配-c命名空间（调用构造方法装配）：</font>

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





----



### 2.5、纯注解装配

不使用XML配置文件【SpringBoot 常使用纯注解方式】



步骤：

>   -   编写一个配置类，使用`@Configuration`修饰
>   -   配置类上使用`@Component-Scan(base-Packages="包路径")`修饰
>   -   创建`AnnotationConfigApplicationContext(配置类的字节码)`对象来代替`ClassPathXmlApplicationContext`
>   -   







---





### 2.6、 引入其他配置文件

在beans标签内：

```xml
<import resource="applicationContext-123.xml" />
```





---



### 2.7、 Bean的生命周期



-   调用无参构造
-   setter方法
-   初始化方法
-   使用
-   销毁方法







---





## 3、AOP



### 3.1、  什么是AOP

AOP也叫 `面向切面编程`，是对`OOP`（面向对象编程）的补充。AOP是为了解决实现某个功能时，相同的代码分散到各个方法中【隔离业务逻辑，解耦，提高内聚性】，采用`横向抽取机制`，利用`动态代理技术`。

简而言之，AOP就是用不修改源码的方式在主干功能中增加新功能。







### 3.2、AOP的底层原理

<font style="color:red;">AOP的底层原理【使用的动态代理技术】有以下两种使用情况：</font>

> 创建代理对象，由代理对象实现功能：
>
> - 有接口：JDK的动态代理，创建接口实现类的代理对象，由代理对象来增强方法。【调用`java.lang.reflect.Proxy`类的`newProxyInstance(接口类加载器，接口的字节码数组，实现InvocationHandler接口来编写增强方法)`】
> - 无接口：CGlib的动态代理，创建当前类的子类的代理对象，由子类代理对象来完成代理。





#### 3.2.1、 JDK动态代理

JDK动态代理，适用于有接口的情况。

步骤：

>   -   创建接口
>   -   创建接口的实现类，实现接口的方法
>   -   创建`InvocationHandler`接口的实现类
>   -   `Proxy.newProxyInstance(接口的类加载器，接口的字节码数组，InvocationHandler实现类的对象);`得到对象
>   -   对象调用方法











---





### 3.3、 AOP术语

-   `Aspect`：切面【把**通知**应用到**切点**的**过程**（代码应用到方法）】，切面类需要在beans标签内注册。
-   `Joinpoint`：连接点，【一个**可以**被动态代理技术增加功能的方法】。
-   `Pointcut`：切点，类名、方法名、满足条件的规则【**实际**被增加功能的方法】。
-   `Advice`：通知（增强），切面类的方法【功能中被增加的**代码**】。
    -   前置通知：在被增强的方法运行之前的时候执行
    -   后置通知：在被增强的方法运行之后的时候执行
    -   环绕通知：在被增强的方法运行前后都执行
    -   异常通知：在被增强的方法运行出现异常时执行
    -   最终通知：最终一定会执行的代码
-   `TargetObject`：目标对象（增强对象），所有被通知的对象。
-   `Proxy`：代理，通知应用到对象后，被动态创建出的对象。
-   `Weaving`：织入，切面代码插入到目标对象后，生成的代理对象的过程。



<font style="color:red;">AOP常用的两个框架：</font>

-   `Spring AOP`：纯Java实现，不需要额外的编译器
-   `AspcetJ`：一个单独的AOP框架，需要有专门的编译器【推荐】





### 3.4、AspectJ 框架实现AOP



<font style="color:red;">AspectJ 框架实现AOP的两种方式：</font>

-   XML声明
-   注解声明





#### 3.4.1、 XML声明AOP

XML声明：切面（类）、切入点（方法调用）、通知（规则）



XML声明AOP时，所有的 切面（类）、切入点（方法调用）、通知（规则）都必须定义在`<aop:config>`标签内，`beans`标签可以有多个`<aop:config>`标签，每个`<aop:config>`标签内可以有多个切面（`<aop:aspect>`）、切点（`<aop:cutpoint>`）、通知（`<aop:advisor>`）







---



#### 3.4.2、 注解声明AOP

















---





## 4、数据库开发









---





## 5、事务管理













---



## 6、Spring5 新特性
