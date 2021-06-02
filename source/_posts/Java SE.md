---
title: Java SE-入门笔记
tag: Java
categories:
  - 后端
  - Java
  - Java SE
---


# Java SE -入门笔记

[toc]

## 0. 前置知识

文档：

-   [Java 教程 | 菜鸟教程 (runoob.com)](https://www.runoob.com/java/java-tutorial.html)
-   [在线文档-jdk-zh](https://tool.oschina.net/apidocs/apidoc?api=jdk-zh)
-   [JavaSE-博客-笔记-网传](https://blog.csdn.net/Zzh1110/article/details/105069644)



### 0.1 Java 的**文件结构** :

>   项目 ->  模块（包）->  .java文件 -> 类 
>
>    
>
>   包的命名规范：
>
>   1.  由字母、数字下划线组成，不能以数字开头，字母全部小写。
>   2.  不同路径通过点(.)来分割，如：`java.lang`
>   3.  为了保证包名唯一性，一般使用域名反写来命名包，如：`com.baidu.music`
>
>    
>
>     
>
>   
>
>   新建包：`project -> 右键 src -> new -> package`
>
>    
>
>   
>
>    按包名展开：`左侧导航栏 -> 齿轮 ->  去掉 compact middle packages的勾`
>
>   

 

### 0.2 运行环境介绍

-   JVM：运行，java虚拟机
-   JRE：运行，（JVM+lib类库 =》真正可以执行.class文件）
-   JDK：开发，4个主要的文件夹：`bin、include、lib、 jre`



范围：

>    JDK > JRE >JVM



`JDK`是用于java程序的`开发`,而`JRE`则是只能运行class而`没有编译的功能`。





Java的能够“一次编译，到处运行”的原因：

>   Java虚拟机在执行字节码（.class文件）时，把字节码解释成具体平台上的机器指令执行。





### 0.3 cmd中的Java指令

```bash
javac 文件名

java 类名
```





## 1. 环境配置 + 快捷键



### 1.1 JDK-下载：

-    [JDK-官网下载](https://www.oracle.com/java/technologies/javase-downloads.html) 
-    [JDK-下载url](https://www.programmer-box.com/?ref=jdk_1.6)
-   [JDK的下载、安装和环境配置教程](https://blog.csdn.net/Marvin_996_ICU/article/details/106240065)
-   [JDK12安装配置-Win10](https://blog.csdn.net/panjiabin321/article/details/89391210)



### 1.2 IDEA下载：

-   [IntelliJ IDEA下载](https://www.jetbrains.com/zh-cn/idea/download/#section=windows)
-   [IDEA 2019 下载+安装教程](https://mp.weixin.qq.com/s?__biz=MzU0MTg5NDkzNA==&mid=2247491865&idx=1&sn=9085c37f2b10a3495d68f5d0b66623a2&chksm=fb205160cc57d876a540fb31dcf8990b2d600855d3a195ccc565683899de64bb055401345920&scene=21#wechat_redirect)
-   [JetBrains 2021 最新版本全家桶激活](https://pan.baidu.com/s/1Yhq_7dP0MOayyEJ-g4_27A)  提取码：ute8



#### 1.2.1 IDEA的配置：

-   [IDEA-配置-视频](https://www.bilibili.com/video/BV1FZ4y1H7rh?p=174&spm_id_from=pageDriver)

>   1.  新建Project时，需要选择已安装的JDK
>2.  更改字号：file -> settings -> editor  -> font 或者 general->mouse control -> 勾选change font size whith ctrl mouse
>   3.  更改快捷键【例如】：file -> settings -> keymap -> 齿轮图标 -> duplicate -> 展开 main Menu -> 展开 code -> code complete -> 展开 -> 选中 -> 右键 -> add keyboard shutcut -> 输入快捷键 -> ok
>4.  方法分隔符：file -> settings -> editor -> general -> appearance -> show method seperator
>   
>

#### 1.2.2 常用快捷键：

| 功能                                 | 快捷键           |
| ------------------------------------ | ---------------- |
| 打印 | sout |
| main 函数 | psvm 或 main |
| 代码提示                             | alt + /          |
| 自动导包（修正代码）                 | alt  + enter     |
| 复制光标所在行，并插入到光标的下一行 | ctrl + d         |
| 删除一行 | ctrl + y |
| 格式化代码                           | ctrl + alt +L    |
| 单行注释                             | ctrl + /         |
| 多行注释                             | ctrl + shift + / |
| 自动生成代码（get / set /toString）  | alt  + insert    |
|移动当前代码行 | alt + shift +  上下箭头|
|快速写 遍历数组的代码|数组名.fori + enter|
|快速生成代码块，如：for、if、try-catch| ctrl + alt + T |
|快速生成 for 代码块| fori |
|快速生成 增强型 for| iter   或者 foreach |
|搜索类| ctrl + n |
|查看子类| ctrl + h |











## 2. 数据类型

![数据类型](https://z3.ax1x.com/2021/05/24/gjUjoj.png)





### 2.1 char 和 byte 的差别：

-   char 是无符号型的，可以表示一个整数，不能表示负数；char可以表中文字符，
-   byte 是有符号型的，可以表示 -128—127 的数，byte不可以表中文字符















## 3. 运算符



![运算符](https://z3.ax1x.com/2021/05/24/gjaKl6.png)







## 4. 数组

>   1.  数组 直接打印: 得到的是`地址`
>
>   2.  数组反转：对称位置的元素交换
>
>       



### 4.1 数组的初始化：【4种】

```java
// 静态[指定内容]：

		// 格式： 数据类型 [] 数组名= new 数据类型[]{数组的内容};
		
		int[] arr = new int[]{1,2,4,6};

//==============================================
//动态[指定长度]

		// 格式：数据类型[] 数组名称 = new 数据类型[长度]
		
		int[] arr = new int[10];
		arr[0] =12;
		arr[1] = 15;
		arr[2] = 45;



//==============================================
//省略：
		
		// 数据类型[] 数组名称 = {数组的内容}；

		int[] arr = {1,2,5,6};



//==============================================
//拆分成2步：
		
		// 格式: 数据类型[] 数组名称；
		// 数组名称 = new 数据类型[]{数组内容}； (采用省略格式则不能分步骤写)

		int[] arr;
		arr = new int[10]{1,2,5,7,9};

```









## 5. 内存区域的划分



-   栈(stack)：

>   存放方法的局部变量，运行方法
>   【局部变量 的特点：一旦超出作用域就会从内存中消失】



-   堆(heap)：

>   new出来的东西都放在堆中(为：引用类型)，如 ：数组
>   【堆内的东西都有一个16进制的地址值】



-   方法区(method area)：

>   存储`.class`的相关信息，包含：方法信息



-   本地方法栈(native method stack)



-   寄存器(register)：与cpu相关











## 6. 面向对象



 面向对象的三大特性：

-   封装性(如： private, 方法等)
-   继承性(extend ,super等)
-   多态性(**子类**继承**父类**的方法后可以 **覆盖重写**override)







### 6.1 类：

>   1.  `类` = `属性` +` 方法` 	// 方法也就是行为、函数。
>
>   
>
>   2.  JAVA的`类`：`成员变量`(在类内部，即：属性) **+** `成员方法`+`构造方法`		// 普通变量：写在函数的内部。 
>
>   
>
>   3.  `方法` ：只能有`1个返回值`。如果想**返回多个数**，可以将**返回值定义为数组**，并返回数组首地址。 
>
>   
>
>   4.  JavaBean：标准类，类中至少要包括：`无参构造方法`，`全参构造方法`，`属性 + getter + setter`
>
>    
>
>   5.  构造方法：在创建对象(new)时，自动调用。【快捷键：`alter + insert`】

 

#### 6.1.1 构造方法：

>   1.  `构造方法的名称`必须与所在的`类的名称`完全相同，大小写也要一样。
>       【普通方法首字小写，构造方法首字大写】 
>   2.   构造方法`不能写返回值类型`，连void也不要写。
>   3.  构造方法`不能return `返回值。
>   4.  如果`没有写`构造方法，那么编译器会`自动赠送`1个构造方法。
>   5.  只要自己编`写了构造方法`，编译器就`不会赠送`构造方法。
>   6.  可以在定义构造方法时，将所需的参数定义在方法的参数列表中， 可以在创建对象时传入参数。但是还是需要写`getter和setter 方法`，目的是`方便修改参数的值`。



#### 6.1.2 成员变量 与 局部变量 的区别：

>   1.  `成员变量`：定义在 `类` 的内部，在**类**中都可以**使用**，有`默认值 ` 。
>
>   
>
>   2.  `局部变量`：定义在 `方法内部`，只能在 **方法内部**使用，`没有默认值`，使用要**先赋值** 。





#### 6.1.3 成员变量 与 局部变量 重名时 的优先级：

>   1.  默认根据`就近原则`，`优先`使用 `局部变量`。
>   2.  如果想使用`成员变量`，可以使用`this关键字`，如： `this.name` 。
>   3.  `this` 一定是写在`方法内部`的，用于在`变量名相同`时，`做区分` 。









#### 6.1.4 Override 方法重写 + Overload 方法重载：

-   [方法重写与方法重载的区别-url](https://blog.csdn.net/weixin_44502804/article/details/90523478)

>   `Override 方法重写`： 
>
>   1.  子类继承父类/接口 后，换掉同名的方法中的处理语句。
>
>   2.  方法重写前后：返回值类型、方法名、参数必须一致。
>   3.  重写后，访问权限必须更宽松，如：重写前 => protected，重写后 => public
>
>   
>
>   `Overload 方法重载`：
>
>   1.  在一个类里面，返回类型可以相同也可以不同，方法名一致，而参数不同。



覆盖重写Override：【需要先：继承】

```java
// 父类：属性 + 无参构造 + 全参构造 + getter + setter + Eat()
// 子类：属性 + 无参构造 + 全参构造 + getter + setter + 覆盖重写的Eat()






// 父类
public class Father {
    private String name;

    public Father(String name) {
        this.name = name;
    }

    public Father() {

    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}






// 子类
public class Son extends Father{
    public Son(String name) {
        super(name);
    }

    public Son() {
    }

    @Override
    public void eat() {
        System.out.println("son：我要吃 -> 水果");
    }
}





//main
public class FaterAndSonDemo {
    public static void main(String[] args) {
        Father father = new Father("刘备");
        Son son = new Son("阿斗");
        father.eat();
        son.eat();
    }
}


```



覆盖重载Overload：

```java
// 以下代码 省略了 构造方法+ getter + setter

public class Calc {
    private int a;
    private int b;
    private float c;


	//方法重载1
    public int sum(int a, int b){
        return a+b;
    }
	
    //方法重载2
    public float sum(int a,int b,float c){
        return a+b+c;
    }    
    
    

}

```





#### 6.1.5 内部类：【4种】

>   `内部类`：1个类 定义在 另一个类 的内部 =》`内部类`(public / protected / private)。 外面的类叫`外部类`(只能public)。
>
>    
>
>   **注意**：`内部类`可以访问`外部类`的所有`成员变量`和`成员方法`。
>
>   



-   [Java内部类](https://blog.csdn.net/weixin_42762133/article/details/82890555?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522160906466316780279191862%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=160906466316780279191862&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_v2~rank_v29-3-82890555.pc_search_result_cache&utm_term=Java)



1.  成员内部类：

```java
/*
 * 1. 定义 外部类、外部类属性、构造方法、getter、setter
 * 2. 定义 成员内部类、内部类属性、构造方法、getter、setter
 * 3. 在 main 中，new 外部类对象
 * 4. 格式：外部类名.内部类名 变量名 = 外部类对象.new 内部类名();
*/

// main
public class OutAndInnerClassDemo {
    public static void main(String[] args) {
        
        Outer outer = new Outer();
        Outer.Inner inner = outer.new Inner();
        
    }
}

//外部类
class Outer{  
 
    public Outer() {
         System.out.println("外部类");
    }

    //内部类
    class Inner{
   
        public Inner() {
             System.out.println("内部类");
        }      
    }
}

```



2.  静态内部类：

```java
/*
 * 1. 定义 外部类、外部类属性、构造方法、getter、setter
 * 2. 定义 静态内部类、内部类属性、构造方法、getter、setter
 * 3. 在 main 中，
 * 4. 格式：外部类名.静态内部类名 变量名 = new 外部类名.静态内部类名();
 * 5. 
*/



// main
public class OutAndInnerClassDemo2 {
    public static void main(String[] args) {

        Outer2.Inner inner = new Outer2.Inner();

    }
}

//外部类
class Outer2 {

    public Outer2() {
        System.out.println("外部类");
    }


    //静态内部类
    static class Inner{
        public Inner() {
            System.out.println("静态内部类");
        }
    }
}

```





3.  方法内部类：【在方法内部：定义、创建new、使用(调用)】

```java
/*
 * 1. 定义 外部类、外部类属性、构造方法、getter、setter
 * 2. 定义 外部类的方法，该方法中的内部类、内部类属性、构造方法、getter、setter
 * 3. 在 外部类的方法中 new 内部类的对象：内部类名 变量名 = new 内部类名();
 * 4. 在 main 中，
 * 5. 格式：
 			外部类 变量名= new 外部类();
 			变量名.外部类方法名();

*/



//main
public class OutAndInnerClassDemo3 {
    public static void main(String[] args) {
        Outer3 o3 = new Outer3();
        o3.Func1();
    }
}



// 外部类
class Outer3{
    public Outer3() {
        System.out.println("外部类 -> 创建");
    }
	
    // 外部类方法
    public  void Func1(){

        System.out.println("外部类的方法 -> 调用");

        // 内部类
        class Inner3{
            public Inner3() {
                System.out.println("内部类 -> 创建");
            }
			
            // 内部类方法调用
            public void say(){
                System.out.println("内部类的方法 -> 调用");
            }
        }

        Inner3 i3 = new Inner3();
        i3.say();
    }

}


```





4.  匿名内部类：【方法中 return 1个 new出的对象】  

**注意**：匿名内部类 没有 构造方法。

```java
/*
 * 1. 定义 接口
 * 2. 定义 外部类、外部类属性、构造方法、getter、setter
 * 3. 定义 外部类的方法，该方法返回1个匿名内部类的对象的定义，在该匿名内部类的对象的定义中重写接口中的方法
 * 4. 在 main 中，
 * 5. 格式：
 			外部类 变量名= new 外部类();
 			变量名.外部类方法名().重写的方法名();

*/


// main
public class OutAndInnerClassDemo4 {
    public static void main(String[] args) {

        Outer4 o4 = new Outer4();
        o4.getInner4().eat();				//注意 连续调用 方法

    }
}



// 接口
public interface Inner4 {
    void eat();
}


//外部类
class Outer4{
    public Outer4() {
        System.out.println("外部类 -> 创建");
    }

    //外部类的方法
    public Inner4 getInner4(){

        return  new Inner4(){
            @Override
            public void eat() {
                System.out.println("匿名内部类 -> 方法");
            }
        };			// 注意分号
    }
}




```



#### 6.1.6 类的使用：

>   1.  导包：`import java.util.*;`
>   2.  创建：`Animals cat = new Animals();`
>   3.  使用：`cat.eat();`



注意：

-   `java.lang` 下的包`不用导`
-   `同一package`下的类也`不用导`
-   `导包语句`必须写在 `package 包名`后，`类名前`

-   包名-格式：`package net.java.util;`







#### 6.1.7 静态变量 与 静态方法：

>   `静态变量`：带有 `static `关键字的`变量`
>
>   `静态方法`：带有 `static` 关键字的`方法`

**注意**：【凡是带`static` 的`变量、方法、代码块`】

-   `属于`整个`类`，而不仅仅 属于一个**对象**
-   只创建`1次`
-   可以直接用 `类名.变量`调用
-   不能使用 `this`,`super`关键字：因为静态方法可以通过类名来调用，而这是可能还没有创建对象，更谈不上继承。





```java
// 静态代码块：
static{
		//语句块
}




// 静态方法
// 格式： 
    public static void PrintMe(){.......}	

// 调用：[对于本类当中的静态方法，可以省略类名称，直接静态方法]
    类名称.静态变量;
    类名称.静态方法();

```



### 6.2 接口：

>   1.  接口就是各个类的规范，
>   2.  接口是一种 `引用的数据类型` ，其中：最重要的是`抽象方法`



#### 6.2.1 格式：

```java
  public interface 接口名称{
        // 接口内容  
  }  
```



#### 6.2.2 接口内容：

>   -   JDK 1.7 的接口可以包含：
>       -   常量
>       -   抽象方法
>   -   JDK 1.8 的接口还可以额外包含
>       -   默认方法
>       -   静态方法
>   -   JDK 1.9 的接口还可以额外包含
>       -   私有方法





**注意** ：

>   1.  接口中的抽象方法，修饰符必须是 public abstract 
>
>       【如：`public abstract void methodAbstract( );`】， 可以省略但不能换成别的 。 
>
>   
>
>   2.  `接口`无法直接使用，只能让1个 “实现类” 来 “实现” 该接口 。
>
>       【类似 抽象类的继承】
>
>    
>
>   3.  `实现类` 除非是1个由abstract修饰的抽象类，否则`必须覆盖重写`所有`抽象方法`。







#### 6.2.3 接口的使用：

```java
/*
 *	1. 定义接口 + 接口内容
 *	2. 定义接口的 "实现类"
 *	3. 在 "实现类" 中，覆盖重写 接口的抽象方法
 *	4. main方法中 new出 "实现类的对象"，"实现类的对象" 调用 重写后的方法
*/



// 定义接口
public interface MyIntface {
    public abstract void Say();
}






// 定义实现类
public class ToDoClass implements MyIntface {
    
    @Override
    public void Say() {
        System.out.println("你好！");
    }
    
}





// main方法
public class IntfaceDemo {
    public static void main(String[] args) {
        
        
        ToDoClass a = new ToDoClass();
        a.Say();
        
        
    }
}


```











### 6.3 四大特性：



 面向对象的三大特性：

>   -   封装性(如： private, 方法等)
>   -   继承性(extend ,super等)
>   -   多态性(**子类**继承**父类**的方法后可以 **覆盖重写**override)





#### 6.3.1 封装性：

封装性有两种：【变量，方法】



private关键字： 	

>   1.  只有在写了private 的类中可以直接访问该属性,	
>   2.  有`private 关键字`就必须在类中`定义` 2个专门 访问、设置 该属性的方法` getter、setter` 。
>
>   
>
>   setNum(int num)【有参数，无返回值】 		
>
>   getNum( )【无参数，有返回值】





#### 6.3.2 继承性：



继承性：

>   1.  继承是`多态性的前提`；
>   2.  承解决了共性的问题（把`几个类`中都有的`属性、方法`放到`1个父类中`，每个`子类 继承 父类`）





父类与子类：

>   1.  父类：基类，超类
>   2.  子类：派生类







在继承关系中，子类就是父类的一种。

例如： 父类是员工，子类是老师，老师也是一种员工 =》 老师 is 员工







继承过程中，成员变量的访问特点：

>   1.  子类有，则`优先使用 子类的成员变量`。
>   2.  `子类 `知道 `父类的变量、方法`，父类不知道子类的变量、方法。
>   3.  父子的`变量、方法` 重名时，优先使用`子类`









`父类的变量`，`子类的变量`，方法的`局部变量` 重名时：

>   1.  访问 父类的变量：super.变量名
>   2.  访问 本类的变量： this.成员变量名
>   3.  访问 方法的局部变量： 直接写变量名











父类、子类、继承的使用：

```java
// 父类【普通的类的定义】：	
	public class 父类名称{}	

// 子类：
	public class 子类名称 extends 父类名称{}



// 例如：
		//父类
		public class Employee {
			String name;
			int age;
		}
				
		//子类 -> 继承父类
		public class Teacher extends Employee {
			String Tno;
		}
```





#### 6.3.3 抽象性：

抽象：

>   一个父类有一个没法具体描述的方法：
>   如：
>
>   -   `子类 cat` 有 `eatFish( )` 的方法，
>   -   `父类 Animal `有`eat( ) `的方法，但没法细说具体执行过程。





抽象方法-格式：

```java
// 方法 返回值前 加上 abstract 关键字，去掉大括号{}，直接以分号；结束。


//抽象方法
public abstract void Eat();
```





**注意** ：

>   1.  `抽象方法` 只能 定义在`抽象类内` 【在 class前加上 abstract】，但 `抽象类`不一定有 `抽象方法`，没有抽象方法的 抽象类 同样 不能直接new,它有特殊用途。  
>
>    
>
>   2.  子类 必须 覆盖重写 父类 所有的 抽象方法。 【除非子类也是抽象类】
>
>       抽象方法覆盖重写 即：去掉 抽象方法的 abstract关键字，补上{ } 
>
>       【可以鼠标点击 extends 行 按下 alt + enter】
>
>   





抽象类：

```java
//抽象类
public abstract class Animals {
    
	//抽象方法,没有普通方法的{}
	public abstract void Eat();		
    
}
```





`抽象类` 与 `抽象方法`的使用：

>   抽象类的 对象 无法直接 new 。
>
>   即： 不能 Animal a = new Animal( );
>
>   必须`先`用一个子类 `继承`一个父类，`再覆盖重写`其抽象方法。





`main方法中`使用`抽象类`的`执行顺序`：

1.  子类 调用 super( )，执行抽象父类构造方法。
2.  执行 子类构造方法。
3.  执行 子类的抽象方法。

```java
/*
 *	1. 在抽象类中 定义 抽象方法
 *	2. 子类继承抽象类，覆盖重写 抽象类的 所有抽象方法
 * 	3. main方法中，new 子类对象
 *
*/



// 抽象类
public abstract class Animals {
	//抽象方法,没有普通方法的{}
	public abstract void Eat();			
}


// 子类 继承 抽象类
public class Cat extends Animals {		
    @Override
    public  void Eat(){
        System.out.println("cat eat".toUpperCase());
    }
}


// main方法
public class Demo {
    public static void main(String[] args) {
        
        
        Cat c = new Cat();
        c.Eat();
        
    }
}

```







#### 6.3.4 多态性：



-   [Java多态性理解](https://www.cnblogs.com/jack204/archive/2012/10/29/2745150.html)



多态性的条件：

-   继承
-   重写
-   父类变量 引用 子类对象



实现多态的技术：

>   动态绑定（dynamic binding），是指在`执行期间判断`所引用对象的`实际类型`，根据其实际的类型调用其相应的方法。

```java
// 左边是父类/接口，右边new的是子类
// 编译时，arr是List；
// 运行时，arr是ArrayList

	List<Integer> arr = new ArrayList<>();

```







### 6.4 包装类



-   [Java包装类-url](https://www.yiibai.com/java/wrapper-class-in-java.html)
-   [Java泛型和包装类](https://blog.csdn.net/Hushboom/article/details/104798466)



#### 6.4.1 包装类是什么：【自动装箱 + 自动拆箱】

包装类 = 基本类型 + 常用方法

将一个类型变成一个类。

>   -   `原始数据类型 `=转换为=》`对象`
>
>   -   `对象` =转换为=》 `原始数据类型`
>   -   可用于实现` 多态性`
>
>    
>
>    
>
>   *`JDK1.5`*开始可以进行`自动装箱，自动拆箱`：
>
>   -   自动装箱： 自动将 `基本类型 => 包装类`
>   -   自动拆箱： 自动将 `包装类 => 基本类型`

![包装类](https://z3.ax1x.com/2021/05/24/gja30e.png)



#### 6.4.2 泛型：

`泛型 <=> <包装类>` 

泛型常用于：指定`某个集合`只能保存`某种数据类型的数据`。

```java
// 泛型就是下面的 <String>，其中String是包装类

List<String> list = new ArrayList<String>();`	// 一种体现多态性的写法，左边是父类型，右边是子类型
```







## 7. 常用的类



### 7.1 Math类：



Math的所属路径：`java.lang.Math`

```java
package com.cyw2;

public class Demo {
    public static void main(String[] args) {
        
// 取整        
        double a = Math.floor(1.4);	//向下取整，无论正负数(取小的)
        double b = Math.round(1.4); //四舍五入(负数入的时候向0进位)
        double c = Math.ceil(1.4);	//向上取整，无论正负数(取大的)    

        System.out.println("1.4 => floor "+a);  // 1.0
        System.out.println("1.4 => round "+b);  // 1.0
        System.out.println("1.4 => ceil "+c);   // 2.0
        System.out.println("==============");
        
    
        
        
// 随机数          
        double d = Math.random();   //返回1个[0,1)之间的double类型的小数
        
        System.out.println(d);
   
        
        

// 比大小        
        int max = Math.max(12,34);  // 返回参数中的最大值
        int min = Math.min(12,34);  // 返回参数中的最小值
        
        System.out.println(max);
        System.out.println(min);
        System.out.println("==============");

        
        
// 三角函数

        double e = Math.toRadians(45.0);    // 将角度制 转为 弧度制
        double f = Math.tan(e);             // 计算tan(PI/4)
        double g = Math.rint(f);
        
        System.out.println(g);   // 将计算结果 转化成 最接近的整数
        System.out.println("==============");
 
        
        
// 数值 与 字符串 相互转化
        String h = Integer.toString(123);
        System.out.println(h);
        
        String i = "123";
        int j = Interager.parseInt(i);
        
        System.out.println(j);
        System.out.println("==============");
        
        
// 多次方 与 开根号     
               
        double k = Math.sqrt(100.0);	// 开根号
        double L = Math.pow(10.0,2.0);	// 多次方
        System.out.println(k);
        System.out.println(L);

    }
}

```





### 7.2 Charater类：

常用方法：

-   `判断`字符类型：是否是 `字母、数字、空白符、大写、小写`
-   转化大小写：转化成 `大写、小写、字符串`

```java
// 定义	

	char ch = 'a';

	Character ch = new Character('a');

	char uniChar = '\u039A';  // Unicode 字符表示形式
 
	// 字符数组
	char[] charArray ={ 'a', 'b', 'c', 'd', 'e' };

	Character ch = new Character('a');


// 常用方法：
	char a = 'A';
	boolean isLetter = Character.isLetter(a);

    System.out.println(isLetter);	// true
    System.out.println(Character.isDigit(a));	//false

```







### 7.3 String类：

String 类是不可改变的，一旦创建了 String 对象，那它的值就无法改变了。

要想改变` String 对象的值`，可以使用 `StringBuffer` 和 `StringBuilder `类。

```java
// 存放在 堆中的字符串常量池中
	String str = "Runoob";

// 使用构造函数
	String str2 = new String("Runoob");


// 字符串长度
	str = "abc";
    int len = str.length(); 
	System.out.println(len);	// 3


// 字符串的拼接
	String str2 = "我的名字是 ".concat("Runoob");	
	String str3 = "我的名字是 "+"Runoob";


// 格式化拼接字符串
	String fs;
	fs = String.format("a的值为%f, 整型变量的值为" 
                    %d, 字符串变量的值为          
                    %s", floatVar, intVar, stringVar);



// 返回指定位置的字符
	String str1 = "abc";
	char ch = str1.charAt(0);
	System.out.println(cg);	// a


// 返回字符串中指定字符的第一个索引
	String str1 = "abc";
	int index = str1.indexOf('b');
	System.out.println(index);	// 1


// 截取从参数位置到最后的字符串: substring(索引)
// 截取从[索引1,索引2)的字符串: substring(索引1,索引2)		

	String str1 = "hello";
	String str2 = str1.substring(0);  	//str2 = "hello"
																						
														
	String str3 = str1.substring(1,4);	//str3 = "ell";


// 字符串的转化
	//将字符串拆为字符数组,并将字符数组作为返回值: toCharArray();
	//获取当前字符串底层的字节数组: getBytes();
	//替换字符串，返回新字符串: replace(old,new);


//字符串的分割
	//split(正则表达式)：按指定规则分割字符串，并返回字符数组，若想用. 作为分割符，需要写 \\.
		
```





字符串在内存中的创建过程：

>   以 String str1 = "abc" 为例 
>
>   内存中情况：  	
>
>   1.  在 `栈`中开辟 `变量的空间`， 	
>
>   2.   在 `堆`中的 `字符串常量池` 中`创建String 对象` 	
>   3.  在 `堆`中 创建 `byte[] 	`
>   4.  `String 对象`引用` byte[] 的地址` 	
>   5.  `栈`中的变量 引用 `String 对象 的地址`





### 7.4 StringBuffer 类 与 StringBuilder 类：

StringBuffer 和 StringBuilder 都可用来创建字符串，
区别：

-   StringBuffer ：速度慢，但在多线程时安全
-   StringBuilder ：速度快，但在多线程时不安全【StringBuilder 更常用】

```java
public class Demo{
    public static void main(String args[]){
        
// 10 为容量
        StringBuilder sb_1 = new StringBuilder(10);
        sb_1.append("大家好");
        System.out.println(sb_1);  
       
        sb.insert(8, "Java");
        System.out.println(sb_1); 
        
// 5为开始位置，8为结束位置
        sb.delete(5,8);
        System.out.println(sb_1);  
        
    }
}
```









### 7.5 Date类：

`import java.util.Date;`

```java
// 2个构造函数

	Date d1 = new Date();

	Date d1 = new Date(long millisec);	// millisec: 从 1970-1-1 到 现在 的毫秒数


// 获取millisec：long getTime( )
// 设置millisec：void setTime(long time)


//比较时间：假设A、B是两个时间

	A.before(B);	//  时间A 在 时间B 前，则 返回true
	A.after(B);		//  时间A 在 时间B 后，则 返回true
	A.equals(B);	//  时间A 与 时间B 相等，则 返回true


// 格式化的时间、日期

	Date dNow = new Date( );
	SimpleDateFormat sdf = new SimpleDateFormat ("yyyy-MM-dd HH:mm:ss");
	System.out.println("当前时间为: " + sdf.format(dNow));
	
		
```







### 7.6 Calender类：



```java
//当前日期
	Calendar c = Calendar.getInstance();

//创建指定日期
	Calendar c1 = Calendar.getInstance();

	c1.set(2009, 6 - 1, 12);	// 设置 年、月、日


// 获取日期的相关信息
	Calendar c1 = Calendar.getInstance();
	// 获得年份
		int year = c1.get(Calendar.YEAR);

	// 获得月份
		int month = c1.get(Calendar.MONTH) + 1;

	// 获得日期
		int date = c1.get(Calendar.DATE);

	// 获得小时
		int hour = c1.get(Calendar.HOUR_OF_DAY);

	// 获得分钟
		int minute = c1.get(Calendar.MINUTE);

	// 获得秒
		int second = c1.get(Calendar.SECOND);

	// 获得星期几【注意（这个与Date类是不同的）：1代表星期日、2代表星期一、3代表星期二，以此类推】
		int day = c1.get(Calendar.DAY_OF_WEEK);
```









### 7.7 Scanner 类：

`import java.util.Scanner;  `

```java
/*
 * Scanner sc = new Scanner(System.in);
 * ``sc.hasNextInt()   	 <=>  sc.nextInt()
 * ``sc.hasNextFloat()   <=>  sc.nextFloat()
 * ``sc.hasNextDouble()  <=>  sc.nextDouble()
 * 
*/


// 从键盘接收数据【next() 按字符】
        Scanner sc1 = new Scanner(System.in);

        if (sc1.hasNext()) {
                String str1 = sc1.next();
                System.out.println("输入的数据为：" + str1);
        }    
        sc1.close();





// 从键盘接收数据【nextLine() 按行，接收回车之前的所有字符，包括空白符】
	    Scanner sc2 = new Scanner(System.in);
        // 从键盘接收数据
 
        // nextLine方式接收字符串
        System.out.println("nextLine方式接收：");
        // 判断是否还有输入
        if (sc2.hasNextLine()) {
            String str2 = sc2.nextLine();
            System.out.println("输入的数据为：" + str2);
        }
        scan.close();

```











### 7.8 Regex-正则表达式：



`import java.util.regex.*;`



java.util.regex 包主要包括以下三个类：

-   Pattern 类：`表达式`
-   Matcher 类：`匹配引擎`
-   PatternSyntaxException类：`语法错误`



Java中的`\\` 等价于 其他语言中的 `\`: 

-    `\b`表示匹配删除，`\\b`表示匹配边界
-   例如：要匹配 str1 = ”(hello)“，正则必须为`\\(hello\\)`





```java
/*
*	1. 匹配1个字符：.
*	2。匹配前面的1个字符 0次~多次：* 等价于{0,}，例如：ab* 即：ab{0,}   =>匹配：[a,ab]
*	3。匹配前面的1个字符 1次~多次：+ 等价于{1,}，例如：ab+ 即：ab{1,}   =>匹配：[ab，abb]
*	4。匹配前面的1个字符 0次~1次：? 等价于{0,1}，例如：ab? 即：ab{0,1}  =>匹配：[a,ab]
*	4。非贪心匹配，尽可能短，用于匹配修饰符之后：? ，例如：ab? 即：a*? =>匹配：a
*	6. 匹配特殊字符：\ ,例如：\\d 表示匹配 \d ,即：匹配1个数字【java中的 \\ 相当于 其他语言中 \】
*	7. 匹配以指定字符开头的字串：^字符,例如：^a 表示匹配以a开头的字串
*	8. 匹配以指定字符结尾的字串：字符$,例如：a$ 表示匹配以a结尾的字串
*	9. 匹配其中的一个，相当于"或"：(字符1|字符2),例如：fu(c|n)  => 匹配：[fuc,fun]
*	10. 匹配指定字符集中的任意一个：[字符1字符2字符3],例如：fu[abc] => 匹配：[fua,fub,fuc]
*	11. 匹配指定连续的字符集中的任意一个：[字符1-字符3],例如：fu[a-c] => 匹配：[fua,fub,fuc]
*	12. 不匹配指定数组中的任意一个：[^字符1字符2字符3],例如：fu[^abc] => 不匹配：[fua,fub,fuc]
*	12. 不匹配指定数组中的任意一个：[^字符1-字符3],例如：fu[^a-c] => 不匹配：[fua,fub,fuc]
*	13. 匹配以指定字符结尾的字串：字符$,例如：a$ 表示匹配以a结尾的字串
*	14. 只匹配边界位置的字符：\b 例如："er\b" 匹配=> never , 不匹配=> verb
*	15. 不匹配边界位置的字符：\B 例如："er\B" 不匹配=> never , 匹配=> verb
*/




        String str1 = "Hello World 123,Hello Man 456! ";
        String pattern = ".*Hello.*";
        boolean isMatch = Pattern.matches(pattern,str1);

        System.out.println("字符串中是否能否找到Hello："+isMatch);	//true
```









### 7.9 try-catch 异常处理：

所有的异常类是从 `java.lang.Exception 类`继承的子类

[Java 异常处理 | 菜鸟教程 (runoob.com)](https://www.runoob.com/java/java-exceptions.html)

异常：

-   `检查性异常`：最容易犯，编译时不能忽略，如：打开不存在的文件
-   `运行时异常`：可以避免，编译时可被忽略，如：空指针异常、下标越界异常、算数异常（如：除数为0）、类型转化异常
-   `错误`（编译错误、运行错误、逻辑错误）：脱离控制，如：堆栈溢出、虚拟机错误、线程死锁

![异常处理-使用技巧](https://z3.ax1x.com/2021/05/24/gjachn.png)



声明异常：

```java
/* 声明异常：try-catch 或 在定义方法时：方法名()throws 异常{ }
*	声明异常的注意事项：
*		1. 只有父类声明了某个异常，子类才能声明该种异常/该种异常的子类
*		   	【父类有，子类才能有】
*
*		2. 子类重写父类的方法时，也要声明 父类已经声明的异常 或 父类已经声明的异常的子类异常
*		
*/


// 越是子类的异常，越要先catch

// 使用 try-catch 来声明异常
try {
    file = new FileInputStream(fileName);
    x = (byte) file.read();
} catch(FileNotFoundException f) { // Not valid!
    f.printStackTrace();
    return -1;
} catch(IOException i) {
    i.printStackTrace();
    return -1;
}
finally{
    // 无论是否有异常，都会执行的代码
}




// 使用 throws关键字 来声明异常
import java.io.*;
public class className
{
  public void deposit(double amount) throws RemoteException
  {
    // Method implementation
    throw new RemoteException();
  }
  //Remainder of class definition
}
```





在 Java 中你可以自定义异常。编写自己的异常类时需要记住下面的几点。

-   所有异常都必须是 Throwable 的子类。
-   如果希望写一个`检查性异常类`【编译不通过的异常】，则需要继承 Exception 类。=》try-catch 或 throws
-   如果你想写一个`运行时异常类`【运行不通过的异常】，那么需要继承 RuntimeException 类。



出现算数异常：除数不能为0

```java

// 出现算数异常：除数不能为0
public class Demo {
    public static void main(String[] args) {

        int a=10,b=0;
        System.out.println(div(a,b));

    }

    // 除法
    public static int div(int a,int b){
        return a/b;

    }
}

```



自定义异常-步骤：

```java
/*
*	自定义异常-步骤：
*		1. 创建异常类，该异常类继承 Exception类
*		2. 重写 异常类的构造方法
*
*	手动抛出异常-步骤：
*		1. 选择1个合适的异常类
*		2. 创建异常类的对象
*		3. 抛出对象
*/


// 异常类
    public class myException extends Exception{

        public myException() {
            super();
        }

        public myException(String msg) {
            super(msg);
        }

    }




// 父类
    public class Father {

        public void eat() throws myException{
            System.out.println("我是父类的-eat");
        }
        
    }




// 女儿类
    public class Daughter extends Father{
        
//main方法
            public static void main(String[] args) {
                   Father daughter = new Daughter();

                   try{
                       daughter.eat();
                   }catch (myException e){
                        e.printStackTrace(); // 打印堆栈的轨迹
                   }
            }
        
//女儿类的 eat()
            @Override
            public void eat() throws myException {	//子类声明的异常必须 小于等于 父类声明的异常
                System.out.println("我是女儿类的-eat");
                System.out.println("我是女儿类的-eat");
                System.out.println("我是女儿类的-eat");

                // 打印3次"我是女儿类的-eat"后，马上抛出异常
                throw new myException("我吃太多了~");	
            }

    }

```



![异常-层次图](https://z3.ax1x.com/2021/05/24/gjaI74.png)





## 8. Stream、IO、File



`import java.io.*;`



需要定义` IO异常`



-   [Java-IO-视频](https://www.bilibili.com/video/BV1Tz4y1X7H7?from=search&seid=14230994899231979002)
-   [Java-IO-笔记](https://www.cnblogs.com/coderzjz/p/13670088.html)



### 8.1 Stream的介绍：

`流-Stream`:

>   一个`数据的序列`。【以 **内存** 为参照物】
>
>   -   `输入流`表示**计算机**从一个源`读取数据 => 内存`，
>
>   -   `输出流`表示**计算机**向一个目标`写入数据 => 存储设备`。【必须 flush( ) 才能写入】



字节流：可以复制 文本、图片、二进制文件

字符流：可以复制 文本文件（包括中文）



流的分类：

>   1.  按方向：
>
>       -   输入流：`读取数据 => 内存`
>       -   输出流：`写入数据 => 存储设备`
>
>       
>
>   2.  按单位：
>
>       -   字节流
>       -   字符流:（只能：读写文本）
>
>        
>
>   3.  按功能：
>
>       -   节点流：读写 数据
>       -   过滤流：增强 功能



![IO流](https://z3.ax1x.com/2021/05/24/gjaXjK.png)

### 8.2 Stream的常用类：

```java
/*
*	字节输入流：InputStream【读数据】	
*		1. read()								// 读取下一个字节
*		2. read(byte[] b)						// 读取下面多个字节
*		3. read(byte[] b,int offset,int maxlen)	// 读取下面多个字节
*		4. close()								// 关闭资源
*	步骤：
*			1. 创建 FileInputStream
*			2. 创建 byte[]
*			3. 在 FileInputStream调用 read(byte数组)上
*
*	字节输出流：OutputStream【写数据】
*		1. write(int b)								// 输出指定字节
*		2. write(byte[] b)							// 输出指定字节数组
*		3. write(byte[] h,int offset,int len)		// 输出指定位置和长度的多个字节
*		4. flush()									// 刷新缓冲区，并 写入数据
*		5. close()									// 关闭资源
*
*
*/


// FileInputStream : InputStream的子类

    import java.io.FileInputStream;

    public class Demo {
        
// main方法，声明IO异常        
        public static void main(String[] args) throws Exception{
            
            // 假设'E:\abc.txt' 已经存在，且内容为：abc012
            
// 创建文件输入流
            FileInputStream fis = new FileInputStream("e:\\abc.txt");
            
            
            
            // 如果 存在字节，则输出，继续循环,
            // 最终输出：97 98 99 48 49 50 读取完毕！【其中，数字：表示字节的ASCII码】
// 读取单个字节
            int data;
            while ( (data=fis.read()) != -1   ){
                    System.out.println(data);
            }
            fis.close();
            System.out.println("读取完毕！");
            
            
// 读取多个字节            
            byte[] buf  = new  byte[3];
            int count = 0 ;				// 返回 读取的字节个数
            while( (count = fis.read(buf) ) ！=-1){
                   System.out.println(new String(buf));	// 输出 读取的字节的字符形式
            }         
            fis.close();
             System.out.println("读取完毕！");

            
            
            
            
            
// FileOutputStream : OutputStream的子类
        // 构造方法：FileOutputStream(路径,[append=false])
            
        FileOutputStream fos = new FileOutputStream("e:\\abc.txt");
        
        // 写入单个字节
        fos.write(97);		// 输出：a
        
        // 写入多个字节
        byte[] buf = new byte[]{'d','e','f'};
        fos.write(buf);		// 输出：def
            
            
		// 写入多个字符
        String str1 = "这是一个字符串";
        byte[] buf2 = str1.getBytes();
        fos.write(buf2);	// 输出："这是一个字符串"
            
        fos.close();    
        System.out.println("输出完毕");	// 最终输出：adef这是一个字符串
            
        }
    }


```





### 8.3 案例-文件复制：

【先输入流-读取，再输出流-写入】

```java
// 目标：将"E:\A,jpeg " 复制到 ”E:\0\B。jpg“

import java.io.FileInputStream;
import java.io.FileOutputStream;

public class Demo {
    
    public static void main(String[] args) throws Exception{

// 创建输入、输出流
            FileInputStream fis= new FileInputStream("E:\\A.jpeg");

            FileOutputStream fos = new FileOutputStream("E:\\0\\B.jpeg");
        
// 创建缓存区 67KB
            byte[] buf = new byte[67*1024];
        
// 读取的字节个数
            int count = 0;

           while((count = fis.read(buf)) != -1) { // 固定写法
// 写入文件
               fos.write(buf,0,count);
           }
// 关闭资源        
        	fis.close();
        	fos.close();
            System.out.println("复制完毕");

    }
}


```





### 8.4 字节缓存流：

`java.io.BufferedInputStream`

`java.io.BufferedOutputStream`



需要与 InputStream或 其子类 配合使用。

目的：

>   1.  调高IO效率，减少访问磁盘
>
>   2.  数据存储在缓冲区，flush( )将数据写入文件，也可直接close( )
>
>       【close( )也会调用flush( ),将内容写入文件】

```java
/*
*	步骤：
*			1. new InputStream
*			2. new BufferedInputStream
*			3. 读取缓存，输出
*			4.关闭资源
*/


// 缓冲输入流
// BufferedInputStream 
        FileInputStream fis = new FileInputStream("e:\\abc.txt");

        BufferedInputStream bis = new BufferedInputStream(fis);

        int data=0;
		
		// 读取："adef"
        while((data = bis.read()) !=-1){
            // 打印结果：97:a  100:d  101:e  102:f 
            System.out.print(data+":"+(char)data+"  ");
            
        }

        bis.close();
		fis();




// 缓冲输出流
// BufferedOutputStream【最后必须 flush() 或 close() 才能 写入成功】
	    FileOutputStream fos = new FileOutputStream("e:\\def.txt");
        BufferedOutputStream bos  = new BufferedOutputStream(fos);

        int i=0;
        while(i<10){
            i++;
            bos.write("Hello\n".getBytes());
            bos.flush();
        }
		bos.close();
		fos();
```





### 8.5 对象流（序列化 / 反序列化）：

-   ObjectOutputStream

-   ObjectInputStream



对象流：【需要结合 InputStream / OutputStream】

>   1.  增强了 缓冲区的功能
>   2.  增强了 读写 基本数据类型 + 字符串
>   3.  增强了 读写对象：readObject( ) 和 writeObject( obj)



`使用 Stream 传输 Object 的过程`：

-   序列化【写入】：ObjectOutputStream，`1个类要想序列化，必须 implements Serializable接口`

-   反序列化【读取】：ObjectInputStream



#### 8.5.1 序列化：【写入Object 到 文件】

**注意**： 

-   被 写入文件（序列化）的类 必须实现 `可序列化接口` 。
-   如果`某个属性不想被序列化`，可以使用transient来修饰 某个类里 的 某个属性：`private transient int age;` 。
-   `静态属性`不可以序列化。

```java
/*
*	类：Student
*		1. 属性：name,age【如果是class类型的属性，则该属性也需要implements Serializable接口】
*		2. implements Serializable接口
		3. 类要有序列化版本ID（快捷键自动添加），该ID可以保证序列化、反序列化的类是同一个
*	
*	序列化-步骤：
*		1. new 文件输出流、对象输出流
*		2. 序列化(写入对象)
*		3. 关闭资源
*/

// Student类
// 【注意：】1个类要想序列化，必须 implements Serializable接口
    public class Student implements Serializable {
		private static final long serialVersionUID = 100L;
        private String name;
        private int age;
        
// 以下省略了 ：构造方法、getter、setter
   		// ……
    }


// 创建文件输出流、对象输出流
	FileOutputStream fos = new FileOutputStream("e:\\ghi.txt");
    ObjectOutputStream oos = new ObjectOutputStream(fos);

// 序列化： ObjectOutputStream
    Student ZhangSan = new Student("张三",20);
    oos.writeObject(ZhangSan);

// 关闭资源
    oos.close();

```



#### 8.5.2 反序列化：【读取文件中的Object】

```java
/*
*	类：上面已经 序列化的类Student
*	
*	反序列化-步骤：
*		1. new 文件输入流、对象输入流
*		2. 反序列化(读取对象，并强制转化为Student类)
*		3. 关闭资源
*/       


// 反序列化
	 FileInputStream fis = new FileInputStream("e:\\ghi.txt");
     ObjectInputStream ois = new ObjectInputStream(fis);

     Student stu1 = (Student) ois.readObject();
     System.out.println(stu1.getName());	// 张三
     System.out.println(stu1.getAge());		// 20
	 ois.close();
```





### 8.6 字符流：



#### 8.6.1 字符编码：



-   `ISO-8859-1`：【1个Byte表示】ASCII，希腊语、阿拉伯语、泰语等
-   `UTF-8`：UniCode的可变长编码
-   `GB-2312`：【1-2 Btye】简体中文
-   `GBK`：【1-2 Btye】简体中文 + 扩展（GB-2312的升级版）
-   `Big5`：台湾-繁体中文



**注意：**` 编码方式` 与 `解码方式` 不一致 =》`乱码`







#### 8.6.2 字符流：

字符流 =》 `java.io.Reader` 和 `java.io.Writer`

字符流常用子类：`FileReader`  和 `FileWriter` 





FileReader：【可显示中文(默认UTF-8编码)】

```java
// 每次读取1个 

		FileReader fr = new FileReader("e:\\0\\1.txt");
        int data= 0;
        while( (data=fr.read())!=-1){
            System.out.println((char)data);		// 打印：好好学习
        }
		fr.close();




// 使用缓冲区读取

       FileReader fr = new FileReader("e:\\0\\1.txt");

        char[] buf = new char[4];

        int count= 0; 

        while( (count = fr.read(buf)) != -1){
            
            System.out.println(new String(buf,0,count));	// 打印：好好学习
            
        }
		fr.close();
```





FileWriter：

```java
        
		FileWriter fw = new FileWriter("e:\\0\\1.txt");
        fw.write("天天向上");
        fw.close();			// 输出：天天向上
```







#### 8.6.2 字符流-复制文本文件：

`FileReader  + FileWriter  `： 复制**文本文件**【`无法复制：图片、二进制文件`】

```java
// 字符流-复制文本文件        
		FileReader fr = new FileReader("e:\\1.txt");
        FileWriter fw = new FileWriter("e:\\0\\2.txt");

        char[] buf = new char[1024];
        int count = 0;

        while((count = fr.read(buf)) != -1){

            fw.write(new String(buf,0,count));
            fw.flush();
        }
        fw.close();
        fr.close();


```





#### 8.6.3 字符缓冲流:

-   BufferedReader：输入
-   BufferedWriter：输出【原样打印】
-   PrintWriter：输出【原样打印、换行打印】



```java
// 字符缓冲-输入流【第一种】:BufferedReader

        FileReader fr = new FileReader("e:\\1.txt");

        BufferedReader br = new BufferedReader(fr);

        char[] buf = new char[1024];

        int count =0;

        while((count=br.read(buf))!= -1){
            System.out.println(buf);	// 打印：天天向上-1 天天向上-2 天天向上-3
        }




// 字符缓冲-输入流【第二种】:BufferedReader
        FileReader fr = new FileReader("e:\\1.txt");

        BufferedReader br = new BufferedReader(fr);

        char[] buf = new char[1024];

        String line = null;

        while((line=br.readLine()) != null){
            
            System.out.println(line);
            
        }

//================================================


// 字符缓冲-输出流: BufferedWriter

        FileWriter fw = new FileWriter("e:\\1.txt");
        BufferedWriter bw = new BufferedWriter(fw);
        bw.write("好好学习");
        bw.newLine();	// 换行
        bw.close();



//================================================



// PrintWriter
	// 原样打印
        PrintWriter pw = new PrintWriter("e:\\2.txt");
        pw.println(97);
        pw.println(true);
        pw.flush();


	// 
```







### 8.7 桥转换流：

`java.io.InputStreamReader` 和 `java.io.OutputStreamWriter`

转化：

-   `字节流` =转为=》`字符流`
-   可设置`编码`



```java
// 1个1个读取   
		FileInputStream fis = new FileInputStream("e:\\1.txt");
        InputStreamReader isr = new InputStreamReader(fis,"utf-8");	// 指定打开的编码
        int data=0;
        while((data= isr.read())!=-1){
            System.out.println((char)data);
        }
        System.out.println(isr.getEncoding());		// 获取当前编码
        isr.close();
        fis.close();



// 输出：
        FileOutputStream fos = new FileOutputStream("e:\\1.txt");
        OutputStreamWriter osw = new OutputStreamWriter(fos);
        osw.write("霓虹，你好\r\n我是第二行");
        osw.flush();
		fos.close();
```







### 8.8 File类：

`File类`：代表物理磁盘中的`文件、文件夹`。



File类-使用：

-   分隔符
    -   File.pathSeparatorChar：路径分隔符（\）
    -   File.separator：名称分隔符（;）
-   文件操作
-   文件夹操作





#### 8.8.1 文件操作：

```java
    public static void creFile() throws Exception{
        
// 创建文件对象，无论是否：真实存在        
        File file = new File("e:\\3.txt");	
        
        
// 按段是否：创建成功 createNewFile()
        boolean isCreSuccess = file.createNewFile();	
        System.out.println(isCreSuccess);	
        
        
// 判断是否：已存在 file.exists()        
        System.out.println(file.exists());	
   
        
// 判断是否：删除  file.delete()       
        System.out.println(file.delete());
     
        
// JVM退出时，删除        
        file.deleteOnExit();
 
        
// 获取绝对路径   file.getAbsoluteFile()
         System.out.println(file.getAbsoluteFile());  // e:\3.txt
  
        
// 获取路径 file.getPath()      
         System.out.println(file.getPath()); // e:\3.txt
     
        
// 获取路径 file.getName()        
        System.out.println(file.getName());	// 3.txt
        
        
// 获取父目录 file.getParent()           
        System.out.println(file.getParent());	// e:\
        
        
// 获取文件的创建时间 file.length()       
        System.out.println(file.length());

        
// 获取文件的 最后修改时间的毫秒数 file.lastModified()
        System.out.println(new Date(file.lastModified()).toString());
        
        
        
// 查看读、写权限        
        System.out.println(file.canRead());
        System.out.println(file.canWrite());
        System.out.println(file.canExecute());
        
// 判断 文件、文件夹        
        System.out.println(file.isFile());
        
// 判断是否 隐藏        
        System.out.println(file.isHidden());
    }
```







#### 8.8.2 文件夹操作：

```java
   public static void creDir() throws Exception{
       
//创建文件夹对象
        File dir =new File("e:\\0\\1");
// 判断是否存在
        if(!dir.exists()){
            
// 创建单层目录: dir.mkdir();	        
            
            
// 创建多层目录: dir.mkdirs();
            dir.mkdirs();	// 返回 true
        }

// 删除文件夹       
        dir.delete();

       
       
// 列出所有的文件名              
        String[] arr = dir.list();
       
        for(String f : arr){
            System.out.println(f);            
        }
       
       
       
// ... 所有的方法都与文件的一样

    }

```



#### 8.8.3 FileFilter接口：

【按条件 筛选出 文件、文件夹】

```java




// 传入FileFilter匿名对象，如果是jpg,

       File[] arr = dir.listFiles(new FileFilter() {
            @Override
            public boolean accept(File pathname) {
                if(pathname.getName().endsWith(".txt")){
                    // 符合条件的返回true
                    return true;
                }else{
                    return false;
                }

            }
        });

// 输出：符合条件的文件名
       for(File f:arr){
           System.out.println(f.getName());
       }

```

















## 9. 集合框架

-   [JavaSE--集合介绍](https://blog.csdn.net/Zzh1110/article/details/105518682)
-   [千锋-Java集合框架详解-bilibili](https://www.bilibili.com/video/BV1zD4y1Q7Fw?p=2)
-   [Java集合-简要笔记](https://www.cnblogs.com/coderzjz/p/13587167.html)
-   [JAVA集合框架详解](https://lazydog036.gitee.io/2020/10/29/JAVA集合框架/)



### 9.1 引入：



`数组、链表、集合`等，都是`存放多个数据`的1种容器，都用于在`内存中存储`（而不是持久化存储：txt,abi,jpg）。





`数组`：【有序存储，元素可重复】

-   特点：指定长度后，长度不可再次更改，只能存放同一种类型的数据。 int [ ] arr = new int[ ];
-   缺点：长度固定，不可更改；添加、删除元素时，效率低；没有现成的方法或属性来获取数组长度



为解决`数组`的上述`缺点`，引入了`集合`。









-   集合只能存储`引用类型`   ：

>   => 必须`自动装箱、自动拆箱`来将  `基本类型 ` 转化为  `引用类型` 。     





-   为什么学习不同的集合？

>   不同的集合，在底层的数据结构的实现不同。







-   数组 与 集合 的区别：

>   1.  数组长度固定，集合长度不固定。
>   2.  数组可以存储基本类型+引用类型，集合只能存储引用类型（因此需要 自动装箱、自动拆箱）。





-   集合的包的位置：`import java.uitl.*;`



![collection集合](https://z3.ax1x.com/2021/05/24/gjdpAH.png)



### 9.2 Collection集合的操作：



Collection、List、Set是接口，不能直接new，而要借助他们的子类如：ArrayList、LinkedList、HashSet、TreeSet。



#### 9.2.1 Collection 集合接口：

```java
/*
* 集合的操作：
*	1. 添加：add(obj),
*	2. 删除：remove(),clear();
*	3. 遍历：foreach语句，迭代器
*	4. 判断：contains(),isEmpty(),equals()
*	5. 获取：get()
*/



// 添加：

            Collection coll = new ArrayList();
            coll.add("苹果");
            coll.add("西瓜");
            System.out.println(“元素个数：”+coll.size());
            System.out.println(coll);


// 删除：

            coll.remove("苹果");
            System.out.println(coll);


// 清空:
            coll.clear();
            System.out.println(coll);






// 遍历的2种方式【重点】：
//		1. 增强的for
//		2. 迭代器【专门用于遍历集合的接口】：
//				haNext(),有元素则返回true
//				next(),获取下一个元素
//				remove()，移除当前元素
		
		// 增强for
            for (Object obj:coll) {
                String s = (String)obj;	// 强制转化为真实的类型
                System.out.println(obj);
            }	



		// 使用迭代器【注意：在迭代时，不能: 集合.remove(),
		// 否则会报：并发修改异常;但可以 it.remove()】
            Iterator it = coll.iterator();
            while(it.hasNext()){
                String s = (String)it.next();
                System.out.println(s);
                it.remove();
            }
            System.out.println(coll.size());	// 0

		
// 判断：存在-> coll.contains();   
//      是否为空 -> coll.isEmpty();

			System.out.println(coll.contains("香蕉"));、
            System.out.println(coll.isEmpty(香蕉));
```



#### 9.2.2 List 集合子接口：

有序、可重复、有下标

```java
/*
* List集合的操作：
*	1. 添加：add(obj),addAll(index,collection)，add(index,obj);
*	2. 删除：remove(),clear();
*   3. 保留元素：retainAll(collection);
*	4. 遍历：foreach语句，
			迭代器:
				Iterator()、
				listIterator()、
				listIterator(index)【迭代方向可 先从前面开始，也可先从后开始】
				
*	5. 判断：contains(obj),isEmpty(),equals(obj)
*	6. 获取：get(index),subList(from_index,to_index), indexOf(obj)
*	7. 修改元素：set(index,obj)
*	8. 转化为数组：toA
*/

import java.util.List;
import java.util.ListIterator;

// 以下内容放入main中

        List list = new ArrayList<>();
        list.add("苹果");
        list.add("西瓜");
        list.add("香蕉");
        System.out.println(list);

		ListIterator it = list.listIterator();


// listIterator迭代器，从 前往后 迭代
		while(it.hasNext()){
            System.out.println(it.next());
        }


// listIterator迭代器，从 后往前 迭代【先将指针移到最后，再从后往前打印】

		while(it.hasNext()){
           it.next();
        }
        
        while(it.hasPrevious()){
            System.out.println(it.previous());
        }


// indexOf()
 		System.out.println(list.indexOf("香蕉")); // 2


// remove(index)  ; remove((Object)obj)

		List list = new ArrayList<>();
        list.add(20);
        list.add(30);
        list.add(40);
        list.add(50);
        list.add(60);

        System.out.println(list);

        list.remove((Object)20);
		list.remove(0);  // 与上1行效果一致

        System.out.println(list);


// subList(from_idx,to_idx) 返回子集,范围 [from_idx,to_idx)

		

		
```





#### 9.2.3 List 接口的常用实现类：





-   ArrayList【重点】：数组，查询快、增删慢，线程不安全【jdk1.2】

>    源码分析：
>
>   1.  默认容量（default_capacity）：当没有元素时，0；有1个元素时，10
>   2.  数组（elementData）
>   3.  当前大小（size）：





-   LinkedList：双链表，增删快、查询慢

```java
/*
*	LinkedList常用方法：
*		1. add()，addAll()
*		2. remove(),removeAll()
*		3. addFirst(),addLast()
* 		4. removeFirst(),removeLast()
* 		5. clear()
*/



        LinkedList ll = new LinkedList();

        Student s1 = new Student("张1",10);
        Student s2 = new Student("张2",20);
        Student s3 = new Student("张3",30);

// 添加
        ll.add(s1);
        ll.add(s2);
        ll.add(s3);
        System.out.println(ll);

//删除
        ll.remove(s2);
        System.out.println(ll);

// 头插
        ll.addFirst(s2);
        System.out.println(ll);

// 头删
        ll.removeFirst();
        System.out.println(ll);

// 尾插
        ll.addLast(s2);
        System.out.println(ll);

// 尾删
        ll.removeLast();
        System.out.println(ll);


// 遍历【向后】
        ListIterator it = ll.listIterator();

        while(it.hasNext()){
            System.out.println(it.next());
        }


// 遍历【向前】
         while(it.hasPrevious()){
                System.out.println(it.previous());
            }

```





-   Vector：数组，查询快、增删慢，线程安全，【jdk1.0】

```java
/*
*	Vector集合 演示：
*		add()
*		remove()
*		size()
*
*		遍历： 枚举器
* 
*/


        Vector v = new Vector();
        v.add("张三");
        v.add("里斯");
        v.add("威武");

// 枚举器，遍历Vector
        Enumeration en = v.elements();

        while(en.hasMoreElements()){
            System.out.println(en.nextElement());
        }



// firstElement()
// lastElement()
// elementAt(idx)

        System.out.println( v.firstElement());
        System.out.println( v.lastElement());
        System.out.println( v.elementAt(1));


```







#### 9.2.4 Set 集合：

只有包含 Collection 集合中的方法，没有自己额外的方法。



-   HashSet：基于hashcode来保证不重复。当hashcodec重复时，equals方法被调用，如果equals方法返回true，则 拒绝添加 新的那个重复元素。【存储结构：哈希表，数组+链表】

-   TreeSet：基于排列顺序来保证不重复。【存储结构：红黑树】







#####  HashSet：

```java
/*
*  HashSet:
*	存储过程：【需要在类里 重写 hashcode()、equals()】
		【1】 根据hashcode查找保存的位置，位置处无元素，则存入，否则执行第二步
		【2】 执行euqals(),若返回false,则表示元素不重复，以链表形式存入。
		小结：先比较hashcode【同一地址】,再equals()【同一值】

* 		1. add(obj)
* 		2. remove(obj)
* 		3. clear()
*		4. contains(obj)
*		4. isEmpty()
*		6. 遍历：
			 - 增强型for
			 - 迭代器
*
*/



        HashSet<Student> s = new HashSet<>();

        Student s1 = new Student("张1",10);
        Student s2 = new Student("张2",20);
        Student s3 = new Student("张3",30);

        s.add(s1);
        s.add(s2);
        s.add(s3);

        System.out.println(s);



// 添加
        s.add(s3);
        System.out.println(s);



// 删除
        s.remove(s2);
        System.out.println(s);



// 包含元素
  		System.out.println( s.contains(s2) ); // true


// 是否为空
  		System.out.println( s.isEmpty() ); // false


// 遍历【增强for】
        for (Student item : s) {
            System.out.println(item);
        }


// 遍历【迭代器】
        Iterator<Student> it = s.iterator();
        
        while(it.hasNext()){
            System.out.println(it.next());
        }



// 【Student类中】重写hashcode方法，
// 注意：此处写法不严谨，使用质数31来参与运算，解决散列冲突: 31*i = (31<<5)-i
    @Override
    public int hashCode() {
        int n1 = this.name.hashCode();
        int n2 = this.age;
        return n1+n2;
    }

// 【Student类中】重写equals方法
   @Override
    public boolean equals(Object obj) {
        if(obj == this){
            return true;
        }
        else if(obj == null){
            return false;
        }
        else if(obj instanceof Student){
            Student s = (Student) obj;
            if(this.name.equals(s.getName())&&this.age==s.getAge()){
                return true;
            } 
        }
         return false;
    }

```









#####  TreeSet：

【红黑树，即：二叉查找树】

-   按 `排列顺序` 实现  `元素不重复`
-   实现了` SortedSet 接口`， 对元素 进行`自动排序`
-   元素类 必须`实现 Comparable 接口`，指定 `排序规则`

```java
/*
*  TreeSet:


* 		1. add(obj)
* 		2. remove(obj)
* 		3. clear()
*		4. contains(obj)
*		4. isEmpty()
*		6. 遍历：
			 - 增强型for
			 - 迭代器
*
*/



// Student类中 实现 Comparable 接口，重写 CompareTo() 
    @Override
    public int compareTo(Student o) {
        int n1 = this.name.compareTo(o.getName());
        int n2 = this.age-o.getAge();
        return n1==0?n2:n1;	// 返回0，表示元素重复
    }



// 或者 main方法中，在new Student类时,传入匿名内部类，指定 比较规则

        TreeSet<Student> s = new TreeSet<>(new Comparator<Student>() {
            @Override
            public int compare(Student o1, Student o2) {
                int n1 = o1.getAge() - o2.getAge();
                int n2 = o1.getName().compareTo(o2.getName());
                return n1==0?n2:n1;
            }
        });





// main
		TreeSet<Student> s = new TreeSet<>();

        Student s1 = new Student("张1",10);
        Student s2 = new Student("张2",20);
        Student s3 = new Student("张3",30);

        s.add(s1);
        s.add(s2);
        s.add(s3);

        System.out.println(s);



// 添加
        s.add(s3);
        System.out.println(s);



// 删除
        s.remove(s2);
        System.out.println(s);



// 包含元素
  		System.out.println( s.contains(s2) ); // true


// 是否为空
  		System.out.println( s.isEmpty() ); // false


// 遍历【增强for】
        for (Student item : s) {
            System.out.println(item);
        }


// 遍历【迭代器】
        Iterator<Student> it = s.iterator();
        
        while(it.hasNext()){
            System.out.println(it.next());
        }

```





#####  TreeSet-案例：

```java
package CollectionDemo;

import java.util.Comparator;
import java.util.TreeSet;

public class TreeSetDemo {
    /*  todo:
    *       按照字符串的长度进行排序
    *           beijing:7,guangzhou:9,shanghai:8
    * */

    
// main
    public static void main(String[] args) {

        TreeSet<String> t = new TreeSet<>(new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {
                int n1 = o1.length() - o2.length();
                int n2 = o1.compareTo(o2);
                return n1==0?n2:n1;
            }
        });


        t.add("beijing:7");
        t.add("guangzhou:9");
        t.add("shanghai:8");

        System.out.println(t);	// [beijing:7, shanghai:8, guangzhou:9]
    }
}

```





### 9.3 泛型

注意：`6.4.2 `中也提到了泛型



1.  泛型-定义：

>   把`数据类型`当作`参数`,且`传入`的数据类型必须是`引用类型`【`基本类型` 必须使用 其`包装类 `作为参数】
>
>   例如：
>
>    `ArrayList<Integer> arr = new ArrayList< >( );`



2.  常见形式：

>   -   泛型类
>   -   泛型接口
>   -   泛型方法





3.  语法：`<T,… >`  ，其中的 T可以换成`E,K,V`





4.  好处：

>   -   提高代码复用性
>   -   提高代码安全性，防止 类型转化异常





5.  泛型在集合框架中的应用：

>   可以指定某个集合只能传入某个类型的数据。
>
>   【不指定`泛型`，`默认`传入`Object类型`，因此需 `强制转化`类型】





#### 9.3.1 泛型类：

```java
/*
*	泛型类的语法： 类名<T>{ }
*		1. T为数据类型的 占位符，可以有多个，每个占位符之间用逗号隔开
*		2. T可以用来 声明 变量
*		3. T可以用来 作为 参数
*		4. T可以用来 作为 返回值
*		5. 不能 使用T的构造方法：不能 new T()
*		6. 不同的泛型对象 不能 相互赋值
*
*/

// 泛型类
	public class MyGeneral<T>{    

            T t1; 

            public void show(T t1){
                System.out.println(t1);
            }

            public T getT1(){
                return t1;
            }   
    }

// main

        MyGeneral<String> m1 = new MyGeneral<>("张三");
        System.out.println( m1.getT1()); // 张三
        m1.show("你好");	//你好
        

        MyGeneral<Integer> m2 = new MyGeneral<>(123);
        System.out.println(m2.getT1()); // 123
        m2.show(456); // 456



```





#### 9.3.2 泛型接口：

```java
/* 	泛型接口：
*		1. 不能创建 泛型常量
*/



// 泛型接口
        public interface myInt<T>{
            T show(T t1);
        }





        // 1. 泛型接口-实现类【在实现接口时，确定要传入的类型】
                public class myIntClass implements myInt<String>{
                    @Override
                    public String show(String t1) {
                        System.out.println(t1);
                        return t1;
                    }
                }
        // main
                myIntClass myInt_1 = new myIntClass();
                myInt_1.show("你好");	// 你好







        // 2. 泛型接口-实现类【在创建对象时，确定要传入的类型】
                public class myIntClass2<T> implements myInt<T>{
                    @Override
                    public T show(T t1) {
                        System.out.println(t1);
                        return t1;
                    }
                }


        // main
                myIntClass2<Integer> myInt_2 = new myIntClass2<>();
                myInt_2.show(123);	// 123
```







#### 9.3.3 泛型方法：

```java
/*
*
*
*
*
*
*
*
*/


public class myGer{
    
 // 泛型方法：   
    public <T> void show(T t1){
        System.out.println("这是泛型方法:" + t1);
    }
    
}


// 泛型方法 T由 传入的数据的类型 来决定
        myGer ger = new myGer();
        ger.show(123);		//这是泛型方法:123
        ger.show("你好");		//这是泛型方法:你好

```









### 9.4 Map 集合：



![Map集合](https://z3.ax1x.com/2021/05/24/gjdYbF.png)





1.  特点：

>   -   用于存储 `无序、无下标`的`键值对`
>   -   键：无序、无下标、不重复
>   -   值：无序、无下标、可重复



```java
/*
*	Map:
*		entrySet();
*		put(key,value);
*		get(key);
*		keySet();
*		values();
*/



        Map<String,String> m = new HashMap<>();

// 添加
        m.put("a","a->10");
        m.put("b","b->20");
        m.put("c","c->30");

        System.out.println(m);


// 删除
		m.remove("c");
        System.out.println(m);


// 遍历-【keySet】
	    Set<String> keyset = m.keySet();

        for (String k : keyset) {
            System.out.println(k+"---"m.get(k));
        }


// 遍历-【entrySet】
		Set<Map.Entry<String,String>>  entries = m.entrySet();

        for (Map.Entry<String,String> item: entries ) {
            
            System.out.println(item.getKey()+"---"+item.getValue());
        }

```





#### 9.4.1 HashMap

默认容量：16。

75%时，开始扩容。

数组长度>8 且 链表长度>64时，使用红黑树。



`为实现每一项的键和值都不一样，需要重写 hashcode()、equals() 【可使用IED的快捷键】`



HashMap源码分析-小结：

>   -   HashMap 刚创建时，table为null【为节省空间】，当添加第一个元素时，table的容量为16。
>   -   元素个数大于阈值（容量的75%)时，会进行扩容为原来的2倍，目的是减少需要调整的元素个数。
>   -   JDK1.8 ，当每个链表长度 大于8，元素个数 大于等于64时，调整为红黑树，目的是提高元素的效率。
>   -   JDK1.8 ，当每个链表长度 小于6时，使用链表。
>   -   JDK1.8以前，使用头插法；JDK1.8以后，使用尾插法。



案例：

统计字符串中每个字符的出现次数

```java
import java.util.HashMap;
import java.util.Scanner;


public class Demo {
    public static void main(String[] args){

        Scanner sc = new Scanner(System.in);

        String s = sc.nextLine();

        HashMap<Character,Integer> hm = new HashMap<>();
        
        char[] arr = s.toCharArray();

        for (char item: arr) {
// hashmap中如果有该字符，则更新value
// 没有该字符，则直接插入当前字符的键值对            
            if(hm.containsKey(item)){
                int t = hm.get(item);
                hm.put(item,++t);
            }else{
                hm.put(item,1);
            }
        }



        System.out.println("现在的字符串："+s);
        System.out.println("现在的HashMap："+hm);

    }
}

```











#### 9.4.2 TreeMap

存储：红黑树

对 key 自动排序

```java
/*
*
*	注意：参数类Student 必须 implents Comparable接口
*	
*     put()
*	  remove()

遍历：
	1. keySet();
	2. Entry
*/     

		Student s1 = new Student("张1", 10);
        Student s2 = new Student("张2", 20);
        Student s3 = new Student("张3", 30);

        TreeMap<Student, String> tm = new TreeMap<>();
        tm.put(s1, "1");
        tm.put(s2, "2");
        tm.put(s3, "3");
        System.out.println(tm);


// 遍历-1
        Set<Student> set = tm.keySet();
        for (Student k:set ) {
            System.out.println(k+"---"+tm.get(k));            
        }


// 遍历-2

  		Map.Entry<Student,String>> entries = tm.entrySet();

        for (Map.Entry<Student,String> item: entries)
        {
            System.out.println(item.getKey()+"---"+item.getValue());
        }
```







### 9.5 Collections 工具类：



**方法**：

-   `public static void reverse(List<?> list)`//反转集合中元素的顺序
-   `public static void shuffle(List<?> list)`//随机重置集合元素的顺序
-   `public static void sort(List<T> list)`//升序排序（元素类型必须实现Comparable接口）



```java
      	
		ArrayList<Integer> arr = new ArrayList<>();
        arr.add(10);
        arr.add(20);
        arr.add(30);
        arr.add(40);

        System.out.println(arr);

// 打乱顺序
        Collections.shuffle(arr);        
        System.out.println(arr);

//排序
    	Collections.sort(arr);
        System.out.println(arr);

// 二分查找
        System.out.println( Collections.binarySearch(arr,20) );

//反转
    	Collections.reverse(arr);
        System.out.println(arr);

```









## 10. 多线程

-   [狂神说Java多线程详解](https://www.bilibili.com/video/BV1V4411p7EF?)
-   [赵姗姗-b站-多线程](https://www.bilibili.com/video/BV1cb4y1X7kz?p=115)
-   [Java多线程详解](https://www.cnblogs.com/13roky/p/14707360.html#1-基本概念)





1.   并发 与 并行：

>   -   并发：在一段**时间段**内执行多个程序
>
>   -   并行：在一个**时间点**执行多个程序



2.   进程 和 线程 ：

>   进程：1个正在运行的程序【资源分配的基本单位】
>   线程：1个进程通常由多个线程组成（最少有1个main线程)【资源调度的基本单位】
>
>    
>
>   **多线程的好处** ：效率高，多个线程之间互不影响



3.   线程的调度：

>   -   分时调度：所有线程轮流使用CPU
>   -   抢占调度：让`优先级`高的线程先使用CPU，如果优先级一样，则随机选一个【`Java使用: 抢占调度`】



4.  主线程：

>   -   主线程：执行 main 方法的线程
>   -   单线程程序：java默认情况只有1个线程=》main线程
>   -   JVM 的 main方法进栈 并 执行main 方法 =》产生1条进栈的路（main线程）





5.  最常见的线程操作：

>   -   设置线程名：setName( )
>
>   -   获取线程名：getName( )
>   -   获取当前线程：Thread.currentThread( )



### 10.1 多线程的实现：





#### 10.1.1 方式1【继承Thread类-重点】

步骤：

-   `继承Thread类`【java.lang.Thread类】形成子类。
-   override `重写 `Thread类的 `run 方法`【线程要干什么】。
-   main中 `new 1个 线程对象`。
-   `线程对象.start( ) `【启动线程】，JVM会自动调用 run( )来执行任务。
-   最终：main线程 和 新的线程 并发执行。



**注意：**

-   多次重复启动1个 线程是非法的。【尤其是在 该线程已经 执行完毕后】
-   java 是执行线程是` 抢占式`，线程的`优先级越高`，越`优先执行`。



```java
package ThreadsDemo;

// 1. 继承Thread类
public class TestThread extends Thread{

// 2. 重写 run()方法
    @Override
    public void run() {

        for (int i = 0; i < 1000; i++) {
            System.out.println("多线程执行-"+i);
        }

    }

// 3. main 函数中 new 线程对象，调用 start()
    public static void main(String[] args) {

        Thread th1 = new TestThread();

        th1.start();


        for (int i = 0; i <1000 ; i++) {
            System.out.println("main线程执行~~");
        }
    }

}

```





##### 案例1-多线程下载图片：

-   前提：

    -   下载、导入jar包： commons-io.jar包 

    >   导入Jar包的步骤：
    >
    >   1.  新建 lib文件夹
    >   2.  将 jar包 托入lib文件夹
    >   3.  打开 file -> project Structure -> lib -> 点击 “+” -> 应用

    -   导入该案例所需的工具类：
        -    `import org.apache.commons.io.FileUtils;`
        -   `import java.net.URL;`  
        -    `import java.io.File;`
        -   `import java.io.IOException;`

```java
/*
*	1. 类： 
			WebDownLoader类，TestThread2类
*
*	
*/


import java.io.IOException;
import java.net.URL;
import java.io.File;
import org.apache.commons.io.FileUtils;


// 下载器类
        class WebDownLoader{

            public void download(String url,String name){
                try {
                    FileUtils.copyURLToFile(new URL(url),new File(name));
                } catch (IOException e) {
                    e.printStackTrace();
                    System.out.println("IO异常-在 download 方法中");
                }
            }
        }



// 多线程-下载图片
    public class TestThread2 extends Thread {
        private String url;
        private String name;

        public TestThread2(String url, String name) {
            this.url = url;
            this.name = name;
        }

// 重写线程类的 run方法        
        @Override
        public void run() {

            WebDownLoader wd = new WebDownLoader();
            wd.download(this.url,this.name);
            System.out.println("下载的文件名为："+this.name);
        }
        
// main方法
        public static void main(String[] args) {
            
            String base_url="https://t7.baidu.com/it/u=";
            
            TestThread2 downTh1 = new TestThread2(base_url+"1595072465,3644073269&fm=193&f=GIF","图片-1.jpg");
            TestThread2 downTh2 = new TestThread2(base_url+"825057118,3516313570&fm=193&f=GIF","图片-2.jpg");
            TestThread2 downTh3 = new TestThread2(base_url+"3435942975,1552946865&fm=193&f=GIF","图片-3.jpg");
 
// 开启多线程，进行下载            
            downTh1.start();
            downTh2.start();
            downTh3.start();
        }
    }



```











#### 10.1.2 方式2【实现Runnable接口-重点-推荐】

`推荐使用`

步骤：

-   实现Runnable接口，重写run( )方法
-   在main方法中，new 1个 实现类【`实现类 就是多线程要抢的 资源`】
-   将 实现类的对象 作为参数，传入 new Thread( ) 构造方法中
-   线程类.start( )来自动执行run( )

```java
package ThreadsDemo;

// 重写 实现类的 run方法
   	 public class TestThread3 implements Runnable{

            @Override
            public void run() {
                System.out.println("我是多线程");
            }

// main函数
            public static void main(String[] args) {
              // 创建实现类的对象
                TestThread3 tt3 = new TestThread3();
              // 将实现类的对象 传入 Thread类的构造方法中
                Thread th1 = new Thread(tt3);

              // 开启线程  
                th1.start();
            }
    }

```



#####  案例2-下载图片

```java
package ThreadsDemo;

import org.apache.commons.io.FileUtils;
import java.io.File;
import java.io.IOException;
import java.net.URL;

// 下载器
    class WebDownLoader2{


        public void download(String url,String name){
            try {
                FileUtils.copyURLToFile(new URL(url),new File(name));

            } catch (IOException e) {
                e.printStackTrace();
                System.out.println("download方法-> IO异常");
            }

        }
    }

// 多线程
    public class TestThread4 implements Runnable{

        private String url;
        private String name;

        public TestThread4(String url, String name) {
            this.url = url;
            this.name = name;
        }

        @Override
        public void run() {

            WebDownLoader2 wd2 = new WebDownLoader2();
            wd2.download(this.url,this.name);
            System.out.println("当前正在下载："+this.name);

        }

        public static void main(String[] args) {

            String url_1 = "https://t7.baidu.com/it/u=3779234486,1094031034&fm=193&f=GIF";
            String url_2 = "https://t7.baidu.com/it/u=3908717,2002330211&fm=193&f=GIF";
            String url_3 ="https://t7.baidu.com/it/u=3785402047,1898752523&fm=193&f=GIF";

            TestThread4 tt1 = new TestThread4(url_1,"1.jpg");
            TestThread4 tt2 = new TestThread4(url_2,"2.jpg");
            TestThread4 tt3 = new TestThread4(url_3,"3.jpg");

            new Thread(tt1).start();
            new Thread(tt2).start();
            new Thread(tt3).start();
            
        }
    }




```





##### 案例3-模拟抢票：

```java
package ThreadsDemo;

// 多线程操作同一对象时，出现线程不安全，数据紊乱【重复 抢到 同一张票】

// 票-类
public class TestThread5 implements Runnable{

    static int ticketNum=10;
// 重写run方法
    @Override
    public void run() {
        while(true){
            if(ticketNum<=0)break;
            System.out.println(Thread.currentThread().getName()+" => 拿到了第"+(ticketNum--)+"张票");
            try {
                Thread.sleep(200);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
// main方法
    public static void main(String[] args) {
        TestThread5 t1 = new TestThread5();
        TestThread5 t2 = new TestThread5();
        TestThread5 t3 = new TestThread5();

        new Thread(t1,"小虹").start();
        new Thread(t2,"小白").start();
        new Thread(t3,"小黄").start();

    }
}

```



##### 案例4-龟兔赛跑：

```java
package ThreadsDemo;

// 多线程案例：龟兔赛跑
// "实现类" =》跑道 =》资源
public class RaceDemo {
    public static void main(String[] args) {
        Race race = new Race();
        new Thread(race,"兔子").start();
        new Thread(race,"乌龟").start();
    }
}

class Race implements Runnable{
    private static String winner;
// 重写run方法
    @Override
    public void run() {

        for (int i = 1; i <=100; i++) {

            if(Thread.currentThread().getName().equals("兔子") && i%10==0){
                try {
                    Thread.sleep(1);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }


            boolean flag = isFinished(i);

            if(flag)break;

            System.out.println(Thread.currentThread().getName()+"-> 跑了"+i+"步");

            isFinished(i);

        }
    }
// 判断是否完成
    public boolean isFinished(int steps){
        if(winner != null){
            return true;
        }else {
            if(steps >= 100){
                winner = Thread.currentThread().getName();
                System.out.println("胜利者："+winner);
                return true;
            }
        }
        return false;
    }
}

```





#### 10.1.3 方式3【实现Callable接口-了解】



好处：

>   -   可以 `抛出异常`
>
>   -   可以 有`返回值`





步骤：

-   实现 Callcable接口
-   重写 call( )方法
-   创建目标对象：
-   创建执行服务：`  ExecutorService es = Executors.newFixedThreadPool(2);`
-   提交执行：  `Future<Boolean> res1 = es.submit(tc1);`
-   获取结果：` boolean r1 = res1.get();`
-   关闭服务：`  es.shutdownNow();`







##### 案例5：下载图片

```java
package ThreadsDemo;

import org.apache.commons.io.FileUtils;

import java.io.File;
import java.io.IOException;
import java.net.URL;
import java.util.concurrent.*;

// 下载器类
        class webImageDownLoader{


            public void download(String url,String name){
                try {
                    FileUtils.copyURLToFile(new URL(url),new File(name));
                } catch (IOException e) {
                    e.printStackTrace();
                }

                System.out.println("下载的文件名为 "+name);
            }
        }

        public class TestCallable implements Callable {
            private String url;
            private String name;


            public TestCallable(String url, String name) {
                this.url = url;
                this.name = name;
            }
// 重写call()
            @Override
            public Boolean call() throws Exception {

                webImageDownLoader wid = new webImageDownLoader();
                wid.download(this.url,this.name);
                return true;
            }

// main函数
            public static void main(String[] args) {
                TestCallable tc1 = new TestCallable("https://t7.baidu.com/it/u=1595072465,3644073269&fm=193&f=GIF","图片-1.jpg");
                TestCallable tc2 = new TestCallable("https://t7.baidu.com/it/u=825057118,3516313570&fm=193&f=GIF","图片-2.jpg");

                // 创建执行服务
                ExecutorService es = Executors.newFixedThreadPool(2);

                // 提交执行操作
                Future<Boolean> res1 = es.submit(tc1);
                Future<Boolean> res2 = es.submit(tc2);

                try {
                    // 获取执行结果
                    boolean r1 = res1.get();
                    boolean r2 = res1.get();

                } catch (InterruptedException e) {
                    e.printStackTrace();
                } catch (ExecutionException e) {
                    e.printStackTrace();
                }


                // 关闭执行服务
                es.shutdownNow();
            }
        }





```





### 10.2 静态代理

多线程的原理 就利用了 静态代理。

```java
/*
*	静态代理：
*		1.真实对象、代理对象，都要实现 同一个方法
*		2. 好处：代理对象可以做很多招生对象做不了的事。
*
*
*/

public class StaticProxy {

    public static void main(String[] args) {
        WeddingCompany weddingCompany = new WeddingCompany(new You());
        weddingCompany.HappyMarry();
    }
}

// 接口
interface Marry{
    void HappyMarry();
}

// 结婚者【代理对象】
class You implements Marry{

    @Override
    public void HappyMarry() {
        System.out.println("我结婚了，你哼开心~~");
    }
}

// 婚庆公司【真实对象】
class WeddingCompany implements Marry{

    private Marry target;

    public WeddingCompany(Marry target) {
        this.target = target;
    }
    
     private void before(){
        System.out.println("布置");
    }

    private void after(){
        System.out.println("收钱");
    }
// 重写接口的方法
    @Override
    public void HappyMarry() {
        this.before();
        this.target.HappyMarry();
        this.after();
    }
    
   
}

```







### 10.3 Lambda 表达式：

Lambda表达式，，是函数式编程，它可以作为**匿名内部类**的替代品。



函数式接口：

>   一个接口，如果`只有1个抽象方法`，则为 `函数式接口` 。【接口只有1个方法】



类似：`        new Thread( ()-> System.out.println("多线程的学习")).start();`



格式：【函数式接口 =》需要保证：接口中只有1个抽象方法】

-   `(参数)-> {表达式1;表达式2;};`

```java
@FunctionalInterface
public interface myInt{
    public abstract void Eat();
    //...其他内容
}
```



步骤：

>   -   定义1个只有1个方法的接口
>   -   使用lambda实现方法并创建对象
>   -   调用方法

函数式接口 =》作为方法的`参数 和 返回值类型`。

```java
class Demo{
    public static void show(myInt a){
        a.Eat();
    }
     public static void main(String[] args){
       // 方式1：传入接口的实现类  
          this.show(new myIntImpCls());
         
       // 方式2：传入接口  
         this.show(new myIntInmp(){
             // 重写接口的方法
         });
         
      // 方式3：传入Lambda 
         this.show(()->{System.out.println("你好");});
    }
    
}
```





##### 案例6：

-   优化前

```java

    interface Ilike{
        void lambda(); 
    }

    class Like implements Ilike{
        @Override
        public void lambda() {
            System.out.println("我喜欢lambda ");
        }
    }


public class Demo {
    public static void main(String[] args) {
// 优化前        
       Ilike a = new Like();
       a.lambda();
        
    }
}



```



-   优化后

```java

interface Ilike{
    void lambda(); 
}

class Like implements Ilike{
    @Override
    public void lambda() {
        System.out.println("我喜欢lambda ");
    }
}

	public class Demo { 
        public static void main(String[] args) {
    
// 普通 
           Ilike a = new Like();
           a.lambda();
       
//静态内部类
           Like2 b = new Like2();
           b.lambda();

//局部内部类
            class Like3 implements Ilike{
                @Override
                public void lambda() {
                    System.out.println("我喜欢lambda ");
                }
            }       
            Like3 c = new Like3();
            c.lambda();
        
        
// 匿名内部类
            Ilike d = new Ilike() {
                @Override
                public void lambda() {

                }
            };        
            d.lambda();
        
// lambda
            Ilike e = ( )->{
                System.out.println("I like lambda");
            };
            e.lambda();

        }
    }


```





##### lambda表达式的简化：

```java
package LandaDemo;



interface Ilike{
    void lambda(int i);
}

public class Demo {

    public static void main(String[] args) {


// 普通 lambda
       Ilike a = (int i)->{
           System.out.println("lambda: "+i);
           System.out.println("lambda: "+i);
       };

       a.lambda(123);

        
        
        
// 简化参数 lambda
        Ilike b = (i)->{
            System.out.println("lambda: "+i);
            System.out.println("lambda: "+i);
        };

        b.lambda(123);

        
        
        
// 简化括号 lambda【多个参数必须有小括号】
        Ilike c = i->{
            System.out.println("lambda: "+i);
            System.out.println("lambda: "+i);
        };

        c.lambda(123);

        
        
        
// 简化括号 lambda【只适合一行代码的情况下使用】
        Ilike d = i->System.out.println("lambda: "+i);

        d.lambda(123);
        

    }
}




```















### 10.4 线程的生命周期：

`java.lang.Thread.State`

-   新生：new Thread( )
-   就绪：线程调用 start( ) 后进入 该状态。【还没 轮到 该线程 使用cpu】
-   运行：线程自动调用run(  )后进入 该状态
-   阻塞：等待
-   消亡：程序正常结束、出现异常、手动 调用已被废弃不用的stop( )



![](https://www.runoob.com/wp-content/uploads/2014/01/java-thread.jpg)





![多线程-状态](https://z3.ax1x.com/2021/05/24/gvPy6S.png)





#### 10.4.1 线程常用方法：

>   -   start( )：开启线程，自动调用线程中的 run( )
>   -   run( )：继承自Runnable接口的方法，必须在线程中 覆盖重写。
>   -   join( )：先start( )，再join( )。一旦 join( )，优先执行完后，才轮到别的线程执行。 【插队、阻塞】
>   -   currentThread( )：获取当前正在运行的线程。
>   -   getName( )：获取线程名
>   -   setName( 线程名 )：设置线程名
>   -   setPriority( 优先级 )：设置线程的 优先级，传入1~10，默认5。值越大，越可能被调用。
>   -   sleep(  毫秒数 )：阻塞
>   -   setDaemon( true )：将子线程 设置为 主线程的伴随线程。主线程停止时，子线程继续执行一段时间后也停止。【先设置，再start( )】
>   -   wait(毫秒数 )：阻塞，【老板等顾客】，毫秒数过后仍然没有线程调用锁对象的notify( )，则自动唤醒。
>   -   notify( )：唤醒，【包子做好给顾客】



几个方法的使用顺序：

>   -   setDaemon( true )
>   -   start( )
>   -   join( )





#### 10.4.2 线程优先级：

>   -   优先级：1~10，默认优先级为 5
>   -   优先级 相同时，按时间片，先到先得
>   -   优先级越高，线程 被CPU先调用 的机率更高
>   -   join( )可以无视优先级，直接插队【先start( )，再join( )】





### 10.5 线程的同步：



-   多线程 产生的问题：

>   多个线程抢夺到同一个资源
>
>   如：买票时，买到同一张票。



-   解决多线程安全问题：

>   加 “锁”【同步、同步监视器】





三种方式：

#### 10.5.1 同步代码块



```java
// 线程类
class TestThread implements Runnable{
    @Override
    public void run() {
        
        for (int i = 0; i <10 ; i++) {

// 同步代码块-形式1           
            synchronized (this){	
                // 这里的this就是要锁住的对象，锁 多了会降低效率。
                // 如果是实现Runnable接口的线程，由于只需new一个线程对象，并将该对象传入多个Thread(),
                // 		因此,锁住的是同一个对象。
                // 但如果是继承Thread类的多线程，由于new了多个线程对象，因此锁住的不是同一个对象，
                // 		因此,锁没有真的生效。
                // 综上，锁对象 必须要是同一个对象。
                System.out.println(Thread.currentThread().getName());
            }
            
            
// 同步代码块-形式2 
            static Object obj = new Object();
            
             synchronized (obj){              
                System.out.println(Thread.currentThread().getName());
            }
            
            
            
// 同步代码块-形式3 【推荐】
           // 将 线程类的字节码 作为 锁对象。
             synchronized (TestThread.class){              
                System.out.println(Thread.currentThread().getName());
            }            
            
        }
    }
}
```



小结：

>   -   语法：`synchronized( 锁对象){ 语句 }`
>   -   锁对象【同步监视器】 必须是 `引用类型`，且最好使用`final 修饰`。
>   -   不要 将String、包装类Interger 作为锁
>   -   在同步代码块中，`不应该 改变锁对象`的引用。
>   -   可以使用一个`static类型的无确切的含义的对象`来充当 `锁对象`【同步监视器】



执行过程：

>   -   线程A来到同步代码块，发现“ 锁”处于open状态，于是进入，并close“锁”
>   -   线程B来到同步代码块，CPU资源切换到线程B，但B发现 “锁” close，于是阻塞。
>   -   线程A 继续接管CPU资源，执行同步代码块的内容，执行完毕后，open“锁”
>
>   
>
>   **小结：**
>
>   -   同步代码块中，可以切换CPU资源，但不能执行同步代码块的内容，因为“锁”，仍处于close状态。





#### 10.5.2 同步方法

```java
class TestThread implements Runnable{
    
// 同步方法  
    private static synchronized void sayWhoAmI(){
        
        System.out.println(Thread.currentThread().getName());
        
    }
    
    @Override
    public void run() {
        
        for (int i = 0; i <10 ; i++) {
// 调用            
            sayWhoAmI();
            
        }
    }
    
   
}
```





#### 10.5.3 Lock锁



`import java.util.concurrent.locks.Lock;`

-   在类的成员位置：Lock lock1 = new ReentrantLock( )
-   在线程问题的语句前【try-catch内】：lock1.lock( )
-   语句块
-   在线程问题的语句后【finally语句块中】：lock1.unlock( )

```java
class TestThread implements Runnable{

// 锁对象    
    Lock lock1 = new ReentrantLock();

    @Override
    public void run() {

        for (int i = 0; i <10 ; i++) {
// 加锁
            lock1.lock();

            try {
// 处理                
                System.out.println(Thread.currentThread().getName());
            } catch (Exception e) {
                e.printStackTrace();
            }finally {   
// 解锁                
                lock1.unlock();
            }

        }
    }  
}

```



小结：

>   Lock的优点：
>
>   -   Lock效率更高
>   -   可由用户控制，而之前的synchronzied由JVM控制
>   -   扩展性好，Lock是一个接口，有多个实现类
>
>    
>
>   使用优先级：
>
>   -   Lock【推荐】 -> 同步代码块 -> 同步方法



线程安全性问题：

>   可能导致`死锁` =》 `尽可能不`使用 同步资源的`嵌套`







### 10.6 线程通信问题：



#### 10.6.1生产者与消费者问题

经典问题：

-   [生产者与消费者问题-视频1：](https://www.bilibili.com/video/BV1op4y1S7KK?from=search&seid=13986664181595566652)
-   [生产者与消费者问题-视频2](https://www.bilibili.com/video/BV1Lb411z71t/?spm_id_from=trigger_reload)

>   生产者：生产商品，放入仓库
>
>   消费者：消费商品，取出仓库 
>
>    
>
>   以上两个线程共享资源【仓库】，但每个线程 执行的操作不同，需要线程之间的通信，来同步仓库中的商品数。

代码分析：

-   生产者：
-   消费者：
-   商品：品牌、名字

![多线程-生产者和消费者](https://z3.ax1x.com/2021/05/24/gx9XeH.png)



#### 10.6.2 解决线程通信问题：

例子：

>   生产者：包子铺老板。
>
>   消费者：顾客
>
>    
>
>   顾客：告诉老板 购买的包子数，顾客调用`wait( )`，放弃CPU执行，进入`waiting无限等待`状态
>
>   老板：花5s做包子，调用`notify( )`，告知唤醒 顾客来拿包子。
>
>   **注意：**
>
>   -   老板、顾客都要使用 `同步代码块` 包裹。
>   -   同步代码块的`锁对象`必须`唯一`。
>   -   只有`锁对象`才能调用` wait( )`，和`notify( )`。

综上：

-   吃、做包子【包子和包子铺互斥，因此包子为 锁对象】
-   修改标志
-   唤醒对方





-   同步代码块

```java
/*
*	分别在生产者类、消费者类中使用同步代码块
*
*/



// main方法
    public class Demo2 {

        public static void main(String[] args) {

            Product p = new Product();

            ProcuderThread pt = new ProcuderThread(p);

            ClientThread ct =new ClientThread(p);

            new Thread(pt).start();
            new Thread(ct).start();
        }
    }

// 产品
    class Product{
        private String brand;

        private String name;

        public String getBrand() {
            return brand;
        }

        public void setBrand(String brand) {
            this.brand = brand;
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public Product() {
        }

        public Product(String brand, String name) {
            this.brand = brand;
            this.name = name;
        }
    }

// 生产者
    class ProcuderThread implements Runnable{
        private Product p;

        public ProcuderThread(Product p) {
            this.p = p;
        }

        @Override
        public void run() {
            for (int i = 1; i <=10 ; i++) {

                synchronized (p){

                    if(i%2==0){
                        // 生产巧克力
                        this.p.setBrand("德芙");
                        try {
                            Thread.sleep(100);
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                        this.p.setName("巧克力");
                    }else{
                        // 生产啤酒
                        this.p.setBrand("青岛");
                        try {
                            Thread.sleep(100);
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                        this.p.setName("啤酒");
                    }


                    System.out.println("生产者生产了："+this.p.getBrand()+"--->"
                            +this.p.getName());

                }

            }
        }
    }

//消费者
    class ClientThread implements Runnable {
        private Product p;

        public ClientThread(Product p) {
            this.p = p;
        }

        @Override
        public void run() {
            for (int i =1; i <=10 ; i++) {

                synchronized (p){
                    System.out.println("消费者消费了："+this.p.getBrand()+"--->"
                            +this.p.getName());
                }
            }
        }
    }
```





-   同步方法

```java
/*
*	在产品类中使用同步代码块
*
*
*/

public class Demo2 {

    public static void main(String[] args) {

        Product p = new Product();

        ProcuderThread pt = new ProcuderThread(p);

        ClientThread ct =new ClientThread(p);

        new Thread(pt).start();
        new Thread(ct).start();


    }
}

// 产品
    class Product{
        private String brand;

        private String name;

        public String getBrand() {
            return brand;
        }

        public void setBrand(String brand) {
            this.brand = brand;
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public Product() {
        }

        public Product(String brand, String name) {
            this.brand = brand;
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            this.name = name;

            System.out.println();
        }

        public synchronized void setProduct(String brand, String name){
            this.setBrand(brand);
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            this.setName(name);

            System.out.println("生产者生产了："+this.getBrand()+"--->"
                    +this.getName());

        }

        public synchronized void getProduct(){

            System.out.println("消费者消费了："+this.getBrand()+"--->"
                    +this.getName());

        }
    }
// 生产者
    class ProcuderThread implements Runnable{
        private Product p;

        public ProcuderThread(Product p) {
            this.p = p;
        }

        @Override
        public void run() {
            for (int i = 1; i <=10 ; i++) {

                    if(i%2==0){
                        p.setProduct("德芙","巧克力");
                    }else{
                        p.setProduct("青岛","啤酒");
                    }

                    System.out.println("生产者生产了："+this.p.getBrand()+"--->"
                            +this.p.getName());

            }
        }
    }

// 消费者
    class ClientThread implements Runnable {
        private Product p;

        public ClientThread(Product p) {
            this.p = p;
        }

        @Override
        public void run() {
            for (int i =1; i <=10 ; i++) {

                p.getProduct();
            }
        }
    }

```



-   Lock锁













### 10.7 线程池：



>   -   第一次使用时，创建多个线程，存入集合中【集合中的线程可以复用】
>   -   使用时，取出线程
>   -   用完后，重写存入线程池
>
>   
>
>    
>
>   JDK1.5 之后，自带线程池，无需 用户 自己使用 集合 创建线程池。
>
>   -   `java.util.concurrent.Executors;` =》生产 线程池 的工厂类
>   -   `ExecutorService newFixedThreadPool( 线程数 );` 生成线程池的方法
>   -   `submit( 线程);`
>   -   `shudown();`
>
>    
>
>   线程池的好处：
>
>   -   提高速度
>   -   降低消耗





```java
package ThreadPoolDemo;

import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

// 线程类
    class TestThread implements Runnable{
        @Override
        public void run() {
            System.out.println("这是:"+Thread.currentThread().getName());
        }
    }

// main方法
    public class Demo {
        public static void main(String[] args) {

// 创建 1个可存放3个线程 的线程池
            ExecutorService service = Executors.newFixedThreadPool(3);

// 创建2个线程，并提交给 线程池
            service.submit(new TestThread());
            service.submit(new TestThread());

            service.shutdownNow();

        }
    }

```

















## 11. 网络编程

整体过程：

-   服务器：启动，监听端口
-   客户端：主动连接 服务器



Java中专门用于TCP通信的类：

-   客户端：`java.net.Socket` ，创建对象，发起连接
-   服务器：`java.net.ServerSocket`，创建对象，开启服务



在TCP通信中，传输了`字节流对象`, 该流对象是`属于客户端`的流，服务器共用客户端的流。



Socket：

>   套接字，是 包含了 IP地址+端口号的网络单位。



Socket的常用方法：

>   构造方法：
>
>   -   客户端：`public Socket(String host目标主机, int port目标端口);`
>   -   服务器：`public ServerSocket(int port);`
>
>    
>
>   客户端-成员方法：
>
>   -   `getInputStream();`  // 获取输入流
>   -   `getOutputStream();` // 获取输出流
>   -   `close()`  // 关闭套接字
>
>    
>
>   服务器-成员方法：
>
>   -   `accept()` // 获取客户端的Socket
>   -   `getInputStream();` // 获取输入流
>   -   `getOutputStream();` // 获取输出流
>   -   `close()`  // 关闭套接字



```java
/*
*	步骤：【客户端】
*		1. 创建客户端Scocket，构造方法中传入IP、端口
*		2. 获取客户端的输出流 getOutputStream()
*		3. 输出流的 write()来向服务器发送数据
*		4. 获取客户端的输入流 getInputStream()
*		5. 输入流的 read()来读取数据
*		6. 释放资源 close()



*	步骤：【服务器】
*		1. 创建服务器Scocket对象，构造方法中传入端口
*		2. accept()获取客户端的Socket,
*		3. socket.getInputStream() 来获取输入流  =》 read()
*		4. socket.getOutputStream() 来获取输出流 =》 write()
*		6. 释放资源【客户端的Socket、服务器的Socket】 close()
*



*  注意：
*		1. 网络通信，必须使用Socket提供的流对象，不能使用自己创建的流。
*		2. Socket创建时，会向服务器发起请求，如果服务器未开启，则出现异常，否则可以正常通信。
*/


// 服务端：=======================================

        package TCPDemo;

        import java.io.IOException;
        import java.io.InputStream;
        import java.io.OutputStream;
        import java.net.ServerSocket;
        import java.net.Socket;
        import java.nio.charset.StandardCharsets;

        public class Demo2 {
            public static void main(String[] args) throws IOException{               
             
                ServerSocket socketServer = new ServerSocket(8888);
              
                Socket client = socketServer.accept();
                
                InputStream is = client.getInputStream();

                byte[] buffer = new byte[1024];
                int len=is.read(buffer);

                System.out.println(new String(buffer,0,len));


                OutputStream os = client.getOutputStream();
                os.write("收到，谢谢".getBytes(StandardCharsets.UTF_8));


                client.close();
                socketServer.close();

            }
        }



// 客户端：=======================================

         package TCPDemo;

        import java.io.IOException;
        import java.io.InputStream;
        import java.io.OutputStream;
        import java.net.Socket;
        import java.nio.charset.StandardCharsets;

        public class Demo {
            public static void main(String[] args)throws IOException {


                Socket socketClient = new Socket("127.0.0.1",8888);
                
                OutputStream o = socketClient.getOutputStream();
                
                o.write("你好，服务器".getBytes(StandardCharsets.UTF_8));


                InputStream is = socketClient.getInputStream();
                
                byte[] buffer = new byte[1024];
                int len=is.read(buffer);
                System.out.println(new String(buffer,0,len));


                socketClient.close();


            }
        }


```





#### 案例1：文件上传、下载

步骤：

-   客户端：获取`本地上传的输入流`，使用`网络Socket输出流`上传文件。接收服务器的“上传成功”
-   服务器：获取`网络Socket输入流`，使用`本地下载输出流`下载文件。给客户端发送“上传成功”。



优化思路：将服务端的代码，放入Thread中，并开启多线程。

```java
// 服务端：=======================================

package TCPDemo;

import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;
import java.nio.charset.StandardCharsets;
import java.util.Random;

public class FileUploadServer {
    public static void main(String[] args)throws Exception{

        ServerSocket server = new ServerSocket(8888);

        new Thread(new Runnable() {
            @Override
            public void run() {
                try {
                    // 死循环，让服务器一直监听客户端
                    while(true){



                        Socket client = server.accept();

                        InputStream is = client.getInputStream();


                        File file = new File("E:\\2-demo");

                        if(!file.exists()){
                            file.mkdirs();

                        }


                        byte[] buffer = new byte[1024];

                        int len = is.read(buffer);

// 自定义文件的命名规则：域名+毫秒值+随机数
                        String filename = "\\txt-"
                                +System.currentTimeMillis()
                                +new Random().nextInt(200)
                                +".txt";


                        FileOutputStream fos = new FileOutputStream(file+filename);

                        fos.write(buffer);


                        OutputStream os = client.getOutputStream();
                        os.write("上传成功".getBytes(StandardCharsets.UTF_8));


                        fos.close();
                        client.close();

                    }

                }catch (Exception e){
                    e.printStackTrace();
                }
            }
        }).start();

    }
}




// 客户端：=======================================
        package TCPDemo;

        import java.io.FileInputStream;
        import java.io.OutputStream;
        import java.net.Socket;
        import java.nio.charset.StandardCharsets;

        public class FileUploadClient {
            public static void main(String[] args) throws Exception{

                FileInputStream fis = new FileInputStream("E:\\0-demo\\1.txt");
                
                Socket client = new Socket("127.0.0.1",8888);
                
                OutputStream os = client.getOutputStream();
                
                byte[] buffer = new byte[1024];
                int len;
                
// 读取数据【可能死循环，因为读到文件结束符后，没有将结束符写入文件，因此read()阻塞，程序不结束】                
                while((len=fis.read(buffer))!=-1){
                     os.write(buffer,0,len);
                }
// 解决死循环问题                
                client.shutdownOutput();
                
// 打印数据【可能死循环，因为读到文件结束符后，没有将结束符写入文件，因此read()阻塞，程序不结束】                      
                InputStream is = client.getInputStream();
                
                while((len=is.read(buffer))!=-1){
                    System.out.println(new String(buffer,0,len));
                }

                fis.close();
                is.close();
                os.close();
                client.close();



            }
        }




```





#### 案例二：模拟 B/S进行通信

```java
// 服务端：Java
// 客户端：浏览器


import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;
import java.nio.charset.StandardCharsets;

public class Demo {
    public static void main(String[] args) throws Exception {

        ServerSocket server = new ServerSocket(8888);

        while (true) {

            Socket clientSocket = server.accept();
            new Thread(new Runnable() {
                @Override
                public void run() {

                    try {


                        InputStream is = clientSocket.getInputStream();

                        BufferedReader br = new BufferedReader(new InputStreamReader(is));

                        // http请求的第一行
                        String line = br.readLine();
                        String[] arr = line.split(" ");
                        String htmlpath = arr[1].substring(1);

                        FileInputStream fis = new FileInputStream(htmlpath);
                        OutputStream os = clientSocket.getOutputStream();

                        //固定写法
                        os.write("HTTP/1.1 200 OK\r\n".getBytes());
                        os.write("Content-Type:text/html\r\n".getBytes());
                        os.write("\r\n".getBytes());


                        // 在浏览器输出页面
                        byte[] buff = new byte[1024];
                        int len = 0;
                        while ((len = fis.read(buff)) != -1) {
                            os.write(buff, 0, len);
                        }


                        fis.close();
                        br.close();
                        clientSocket.close();


                    } catch (Exception e) {
                        e.printStackTrace();
                    }

                }
            }).start();

        }

    }
}


```













## 12. Junit单元测试



![测试-分类](https://z3.ax1x.com/2021/05/27/2CT5h8.png)

Junit：

>   一种白盒测试工具。



#### 12.1 Junit的使用：【@Test】

```java
/*	步骤：
*		1. 定义1个测试类
			 建议：
               - 包名：xxx.xxxx.test;  => 如： cn.itcast.test;
			   - 类名：测试类的类名以被测试类的名字+Test组成：类名Test =》 如：CalcTest
		
		2. 定义1个测试方法：=》如：void testAdd()
        	 建议：
        	 	- 方法名： 以test开头
        	 	- 方法返回值： void
        	 	- 参数：无需参数
		3. 在方法定义位置前，加上注解@Test ,使得方法可以独立运行【无需main方法】
        
        4. 导入Junit工具的依赖包
        	- 【点击@Test旁边的小灯泡，添加进classPath】
        
        5. 在测试方法中：
        	- 创建被测试的对象，调用被测试的方法。
        	
        6. 运行
        
        7. 判断结果：
        	- 红色：测试失败
        	- 绿色：测试通过
        	
        8. 断言，将当前结果与正确结果比较	
        	Assert.assertEquals(正确值，当前值);
*
*
*/


package demo.test;

import org.junit.Test;
import demo.junit.Calculator;// 自定义的被测试类

public class CalcTest {

    // 测试add方法
    @Test
    public void testAdd(){
        Calculator c = new  Calculator();
        int rst = c.add(1,2);
        Assert.assertEquals(3,rst);
    }
    
    // 测试sub方法
    @Test
    public void testSub(){
        Calculator c = new Calculator();
        int rst = c.sub(2,1);
        Assert.assertEquals(1,2);
    }
    
}

```





#### 12.2 Junit的使用：【@Before 和 @After】

```java
package demo.test;

import org.junit.After;
import org.junit.Assert;
import org.junit.Before;
import org.junit.Test;
import demo.junit.Calculator;// 自定义的被测试类

public class CalcTest {

    /*
    *   定义1个 init方法，用于在申请资源,每个测试方法在执行前都会自动调用init( )
    *
    *
    * */
    @Before
    public void init(){
        System.out.println("申请资源-init()");
    }


    /*
     *   定义1个 close（），用于在释放资源,每个测试方法在执行后都会自动调用init( )
     *
     *
     * */
    @After
    public void close(){
        System.out.println("释放资源-after()");

    }



    @Test
    public void testAdd(){
        System.out.println("testAdd方法执行");
        Calculator c = new  Calculator();
        int rst = c.add(1,2);
        //System.out.println(rst);
        Assert.assertEquals(3,rst);
    }

    @Test
    public void testSub(){
        System.out.println("testSub方法执行");
        Calculator c = new Calculator();
        int rst = c.sub(2,1);
        Assert.assertEquals(1,2);
    }
}

```







## 13. 反射

-   [Java--反射CSDN博客](https://blog.csdn.net/Zzh1110/article/details/104101124)

-   [狂神说-反射](https://www.bilibili.com/video/BV1p4411P7V3?p=5)

    



**反射** 【框架设计的灵魂】：

>   定义：将类的组成部分，封装为各个对象。
>
>   可以在`运行`的时候，`获取类`的相关信息。【是Java作为`准动态语言`的标志】



-   正常过程：

>   导包 =》new 对象 =》 得到 实例化对象

-   反射过程：

>   获取实例化对象 =》getClass( )来得到类信息 =》找到所在的包名



### 13.1 Java代码的三个阶段：

-   源代码阶段：编写代码【成员变量+成员方法+构造方法】
-   类对象阶段：通过类加载器，将代码读入 JVM【在这个阶段，源码中的1个类，被拆成3个Class对象】
-   运行时阶段：运行代码



![Java代码的三个阶段](https://z3.ax1x.com/2021/05/29/2kTPf0.png)





反射的好处：

>   -   可以在代码运行的时候，来操作对象。【例如：IDE的代码提示功能，就是利用了反射的原理，在类对象阶段加载Methods数组】
>   -   解耦





代码演示：

```java
/*
*   1. 获取字节码【类名.class】：
			Class personClass = Person.class;
			
	2. 常用方法：
	
	    0-设置访问权限【忽略安全检查】：true
        	declaredField_1.setAccessible(true);
        	
	    1-字段：
			- 获取类中，所有public的字段(属性):
				 Field[] fields = personClass.getFields();
				 
			- 获取类中，指定的public的字段(属性):	 
				 Field field_1 = personClass.getField(字段名);
				 				 
			- 获取类中，所有的字段【包括private,但获取private的前提是先设置权限】:	 
				 Field[] declaredFields = personClass.getDeclaredFields();
				 
				 
			- 获取类中，指定的字段【包括private,但获取private的前提是先设置权限】:	 
				 Field declaredField = personClass.getDeclaredField(“name”);
                 
                 
        2-构造器： 
            - 获取类中，所有的构造器
   			Constructor<?>[] Constructors = personClass.getConstructors();
			
			-获取类中，指定的构造器
        	Constructor<T> Constructor = personClass.getConstructor(类<?>...参数类型);
        
            - 获取类中，所有的已经声明的构造器
             Constructor[] declaredConstructors = personClass.getDeclaredConstructors();
             
		 	-获取类中，指定的已经声明的构造器
    		 Constructor declaredConstructor = personClass.getDeclaredConstructor(类<?>...参数类型);
    		 
    		 
       3-成员方法：
       
        	Method[] methods = personClass.getMethods();
        	
        	Method method = personClass.getMethod(String name,类<?>...参数类型);

        	Method[] declaredMethods = personClass.getDeclaredMethods();

        	Method declaredMethod = personClass.getDeclaredMethod(String name,类<?>...参数类型);
     4-获取类名：
     		String class_name = personClass.getName();

*
*
*/





import java.lang.reflect.Field;

// Person类
    class Person{
        private String name;
        private int age;
        
        // 以下省略了get/set访问器
        。。。
            
        public Person() {
        }
        public Person(String name, int age) {
            this.name = name;
            this.age = age;
        }

        
    }

public class Demo {

    public static void main(String[] args) throws Exception{
// 字段相关：=================================================
        // 利用反射来获取字节码，找到Person类
        Class personClass = Person.class;
        // 获取Person中 所有 public 的字段【属性】
        Field[] fields = personClass.getFields();
        
        // 打印所有public属性
        for (Field field : fields) {
            System.out.println(field);
        }
        
        
        
        
        
        // 创建对象
        Person p = new Person("张三",20,1);
        // 获取Person类中的public字段 a类
        Field a = personClass.getField("a");
        
        // 获取 对象p中的属性a的值的对象
        Object value = a.get(p);
        System.out.println(value); // 1
        
        // 设置 对象p中的属性a的值
        a.set(p,123);
        System.out.println(p); // Person{name='张三', age=20, a=123}
        
        

        
        // 获取声明过的属性【包括：private类型的】
        Field declaredField_name = personClass.getDeclaredField("name");
        // 设置访问权限【忽略安全检查】：true
        declaredField_name.setAccessible(true);
        // 获取 具体字段值的 对象
        Object val2 = declaredField_name.get(p);
        System.out.println(val2);
        
// 构造器相关：=================================================      
		Class<Person> personClass = Person.class;
        
		//获取构造器
Constructor constructor1= personClass.getConstructor(String.class,int.class,int.class);

    	// 打印结果：public Person(java.lang.String,int,int)
    	System.out.println(constructor);

    	// 1.创建对象-有参【构造器-newInstance方法】
        Object person_1 = constructor1.newInstance("张三",23,1);
        // 2.创建对象-无参【构造器-newInstance方法】
        Object person_2 = constructor1.newInstance();
        // 3.创建对象-无参【Class类-newInstance方法，已弃用】
        Object person_3 = personClass.newInstance();
        
        // Person{name='张三', age=23, a=1}
        System.out.println(person_1);
        

        
// 成员方法相关：=================================================             
        
        
        Class<Person> personClass = Person.class;
        // 获取指定的方法【方法无参数时】
        Method eat_method = personClass.getMethod("eat");
        System.out.println(eat_method);
        
        Person p = new Person("张三",20,1);
        //调用eat方法
        eat_method.invoke(p);  //我eat
        
        
        
        // 获取指定的方法【方法有参数时,传入：方法名+参数的反射类】        
        Method eat_2 = personClass.getMethod("eat", String.class);
        Person p2 = new Person("李四",24,3);
        eat_2.invoke(p2,"香蕉"); //我eat:香蕉
        
        
        
        
        // 获取类名
        String class_name = personClass.getName();
        System.out.println(class_name);

    }
    
}




```







### 13.2 案例1：

目标：

>   写一个工具，在不改变任何代码的前提下，可以用来：
>
>   -   创建任意对象
>   -   执行任意方法



实现思路:

>   -   需要：配置文件、反射



实现步骤：

>   -   将需要创建的`对象的全类名` 和 需要调用的`方法` 定义在`配置文件`中。
>   -   加载`配置文件`。
>   -   利用`反射`来加载 `所需的类`到`内存`中。
>   -   创建对象
>   -   执行方法



```java
import java.io.IOException;
import java.io.InputStream;
import java.lang.reflect.Member;
import java.lang.reflect.Method;
import java.util.Properties;

public class ReflectTest {
    public static void main(String[] args) throws Exception{
        
        // 创建properties对象
        Properties prop = new Properties();
        
        //获取类加载器，通过类加载器，加载class目录下的配置文件的输入流
        ClassLoader classLoader_1 = ReflectTest.class.getClassLoader();
        InputStream is = classLoader_1.getResourceAsStream("pro.properties");

        //利用配置文件的输入流，加载配置文件
        prop.load(is);

        //获取具体的配置
        String className_1 = prop.getProperty("className");
        String methodName_1 = prop.getProperty("methodName");

        //获取Class类
        Class<?> cls = Class.forName(className_1);
        // 创建类对象
        Object  c = cls.newInstance();

        //获取方法类
        Class<?> cls2 = Class.forName(methodName_1);
        Method m1 = cls2.getMethod(methodName_1);

        //调用方法
        m1.invoke(c);

    }
}




```





### 13.3 获取Class对象的3种方式：

-   源代码阶段：`Class.forName(全类名);	`// 全类名：`包名.类名`
-   类加载阶段：`类名.class 属性;`
-   运行时阶段：`对象名.getClass( ); `  // getClass( )定义在Object类中

结论：

>    同一个字节码文件，在程序的一次运行中，只会加载一次，不论用哪种方式获取的Class对象，最终只会是同一个。







### 13.4 Class对象的功能：

-   获取` 成员变量`：`getFields();`

-   获取 `成员方法`：`getMethods()`

-   获取 `构造方法`：`getConstructors();`

-   获取 `类名`：`getClassName();`

    



### 13.5 类加载的过程：

![Java内存分析](https://z3.ax1x.com/2021/05/30/2V5Aqe.png)





![Java-类加载的过程](https://z3.ax1x.com/2021/05/30/2V5KRP.png)

![类加载的详细过程](https://z3.ax1x.com/2021/05/30/2V5jQf.png)



## 14. 注解（Annotation）

-   [Java--自定义注解-CSDN博客](https://blog.csdn.net/Zzh1110/article/details/104887317)
-   [狂神说-注解和反射](https://www.bilibili.com/video/BV1p4411P7V3?p=1)



### 14.1 注解-概念:



>   -   `注释`：给`人`看的备注
>   -   `注解`：给`计算机`看的备注，也叫`元数据`，`JDK1.5`开始引入的特性。





### 14.2 注解的作用：



>   -   编写文档：生成doc文档
>   -   代码分析：使用反射
>   -   编译检查：如：Ovverride



```java
/**
*
*	注解doc演示
*	@author cyw
*	@version 1.0
*	@since 1.5
*/

// 在cmd中，进入该文件所在的目录，输入：javadoc AnnoDemo.java ,可以得到该类的文档

public class AnnoDemo{
    
    /**
	*	@param a 整数
	*	@param b整数
	*	@return 两数之和
	*/
        public add(int a,int b){
            return a+b;
        }
    
}



```









### 14.2  JDK中内置的注解：

-   `@Override`: 检查被标注的方法是否是继承自 父类/接口。
-   `@Deprecated`: 标注的内容已过时。
-   `@SuppressWarnings`: 压制警告 =》 `@@SuppressWarnings("all")`



```java

// 压制所有警告：IDE不要弹出警告信息    
	@SuppressWarnings("all")
    public class AnnoDemo{

        // 用于标记：方法已过时
            @Deprecated
            public add_1(int a,int b){
                return a+b;
            }


            public add_2(int a,int b){
                return a+b;
            }

    }
```







### 14.3 自定义注解：

-   格式：【`元注解 + public @interface 注解名{ }`】

```java
元注解
public @interface 注解名{
    
}    


// 例如：
    public @interface MyAnno{

    } 

	@MyAnno
	public void sayHello(){
        System.out.println("你好");
    }
```



-   注解的`本质`：【继承了Annotion类的1个`接口`】

```java
public interface MyAnno extends java.lang.annotion,Annotion{
    
} 
```



-   注解的`属性`：【注解接口中的`抽象方法`】

    >   1.  只要定义了注解的属性，则该注解的属性必须赋完值才能使用。
    >
    >   2.  可以使用default关键字，来给注解的属性赋默认值。
    >   3.  如果注解只有1个属性，且该属性的名字为value，使用时，可以只传值，不写属性名。
    >   4.  数组在数值时，在数组的值的最外面，用 { } 包裹；数组只有1个值时，可省略大括号。
    >
    >   





-   注解中抽象方法（注解的属性）的返回值的类型【5种】：

>   -   基本类型
>   -   字符串
>   -   枚举
>   -   注解
>   -   以上4种的数组

```java
    public @interface MyAnno{
		int show_1();	//基本类型
        String show_2();
        MyAnno show_3();
        String[] show_4();
 
    } 

```





实例：

```java
    public @interface MyAnno{
        String name_method();
		int age_method();
    }


    @MyAnno(name_method="张三",age_method = 13)
    public void Eat(){
        System.out.println("你好");
    }
```



#### 14.3.1 元注解：

概念：

>   元注解：修饰注解的注解。



常见的元注解：

-   `@Target`：描述`作用的位置`。如：作用在方法
    -   ElementType：【一种枚举类型】
        -   TYPE：作用在`类`上
        -   METHOD：作用在`方法`上
        -   FIELD：作用在`成员变量`
-   `@Retention`：描述 注解 `保留的阶段`。如：保留在源码阶段
    -   RetentionPolicy：【一种枚举类型】
        -   SOURCE：源码阶段
        -   CLASS：class类对象阶段
        -   RUNTIME：运行时阶段【最常用】
-   `@Documented`：描述 注解 `是否被抽取到API文档`中。
-   `@Inheritied`：描述 注解 `是否被子类继承`。





#### 14.3.2 注解的使用:

-   获取 注解使用的`位置的对象`
-   利用位置对象 获取 `注解对象`: obj.getAnnotion(注解名.class)
-   调用注解中的`抽象方法`





```java

@Target(ElementType.TYPE)		// 元注解，Target元注解,指定自定义注解的使用位置，TYPE：给类使用
@Retention(RetentionPolicy.RUNTIME)	// 元注解，Retention元注解,指定自定义注解的使用时期
public @interface myAnnoDemo{
    String name() default "ZhangSan";
}

//使用注解
@myAnnoDemo(name="lisi")
class TestClass{
    public static void show(){
        System.out.println();
    }
}

```







