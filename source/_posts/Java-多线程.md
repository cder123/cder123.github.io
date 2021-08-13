---
title: Java-多线程
tag: Java
categories:
  - 后端
  - Java
  - Java SE
---

# Java-多线程

[toc]



## 0、资源

-   [JUC-入门视频](https://www.bilibili.com/video/BV1vE411D7KE?p=39)

-   [JUC学习笔记-csdn](https://blog.csdn.net/lizongxiao/article/details/106668806)







## 1、案例-1：Synchronized关键字

先导知识：

>   多线程涉及的3个包
>
>   -   java.util.concurrent：并发包
>   -   java.util.concurrent.locks：并发锁包
>   -   java.util.concurrent.atomic：并发原子包









30张票，由3个售票员卖。



通用口诀：线程、操作、资源类





步骤1：【定义1个资源类（有1个提供资源的操作方法）】

```java
class Ticket{
    private int count = 30;

    // 定义操作
    public synchronized void SaleTicket(){
        if(count>0){
            try {
                String curName = Thread.currentThread().getName();
            	System.out.println(curName +" 卖了第"+(count--)+"张票");
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```



步骤2：【main方法中，创建资源类和线程的对象】

```java
// 创建资源类的对象
Ticket ticket = new Ticket();

// 在线程的run方法中调用资源类的操作
new Thread(new Runnable() {
    @Override
    public void run() {
        for (int i = 0; i <30 ; i++) {
            ticket.SaleTicket();
        }
    }
},"aa").start();

// 第2个资源类
new Thread(new Runnable() {
    @Override
    public void run() {
        for (int i = 0; i <30 ; i++) {
            ticket.SaleTicket();
        }
    }
},"bb").start();


new Thread(new Runnable() {
    @Override
    public void run() {
        for (int i = 0; i <30 ; i++) {
            ticket.SaleTicket();
        }
    }
},"cc").start();
```





## 2、可重入锁：ReentrantLock

使用口诀：

>   1.   线程、操作、资源类
>   2.   判断、干活、再通知（判断只能使用while来判断）
>   3.   标志位





>   为什么不用 Synchronized 关键字 来加锁？
>
>   -   Synchronized 关键字的锁粒度太大，类似于你要封闭1个房间，却把整个大楼关了。





案例-1：使用Reentrantlock可重入锁的改进

资源类：

```java
class Ticket{
    private int count = 30;
    // 在资源类中，创建可重入锁
    private Lock lock = new ReentrantLock();

    public synchronized void SaleTicket(){
        
        // 加锁（一定要写在try-catch的外面）
        lock.lock();

        try {
            if(count>0){
                String curName = Thread.currentThread().getName();
                System.out.println(curName+" 卖了第"+(count--)+"张票");
                Thread.sleep(200);
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            // 释放锁
            lock.unlock();
        }
    }
}
```

线程调用：main方法中（将原先匿名内部类run方法中的内容直接写在lambda表达式中）

**能用lambda表达式简化的原因：利用了函数式接口的思想（runnable 接口只有1个抽象方法: run方法）**

函数式接口默认被@FunctionalInterface注解修饰（类似所有类都继承自Object类，无需手动写就能自动补）

函数式接口可以有多个default关键字修饰的方法，因为default关键字修饰的方法在定义时必须写实现代码。

```java
    	Ticket ticket = new Ticket();
// 使用lanbda表达式简化代码
        new Thread(()->{
            for (int i = 0; i <40 ; i++)ticket.sell();
        },"aa").start();

        new Thread(()->{
            for (int i = 0; i <40 ; i++)ticket.sell();
        },"bb").start();

        new Thread(()->{
            for (int i = 0; i <40 ; i++)ticket.sell();
        },"cc").start();
```







回顾线程的6种状态：

-   new
-   runnable
-   waiting：一直等
-   time_waiting：时间到了就不等了
-   terminated
-   blocked





## 3、生产者-消费者模型



口诀：**while判断、干活、通知**

注意：多线时，不要用if来判断，为防止线程的虚假唤醒，应该使用while来判断。



为什么需要使用while来判断?

-   因为使用if来判断时，假设进入if后还没等到wait就被中断，那么当再次被唤醒时，不会进行判断直接wait，造成了虚假唤醒。





案例目标：多线程操作1个数，使其增加或减少



资源类：

```java
public class myResource{
    private int num=0;
    // 增加
    public synchronized void add()throws InterruptedException{
        
        // 判断
        while(num!=0){
            this.wait();
        }
        
        // 干活
        num++;
        String curName = Thread.currentThread().getName();
        System.out.println(curName+" "+num);
        
        // 通知
        this.notify();
    }
    
    // 减少
    public synchronized void sub()throws InterruptedException{
        
        while(num == 0){
            this.wait();
        }
        
        num--;
        String curName = Thread.currentThread().getName();
        System.out.println(curName+" "+num);
        
        this.notify();
        
        
    }
    
}
```



线程：

```java
// main方法

myResource mr = new myResource();

// 第1个线程
new Thread(
	()->{
        for(int i=0;i<10;i++){
            mr.add();
        }
    }
,"AA").start();


// 第2个线程
new Thread(
	()->{
        for(int i=0;i<10;i++){
            mr.sub();
        }
    }
，"BB").start();
```







## 4、新版多线程的写法（Lock锁）

原先使用 Synchronized - wait( ) - notifyAll( )

现在使用 lock锁 - await( ) - signalAll( )



注意： lock锁 - await( ) - signal( ) 需要先创建Condition对象（condition对象就是钥匙）。





4个线程抢占加减：

资源类：

```java
class Num2{
    private int num=0;
    Lock lock = new ReentrantLock();
    Condition condition = lock.newCondition();

    public void add()throws InterruptedException{
        lock.lock();
        try {


            while(num!=0){
                condition.await();
            }
            num++;
            System.out.println(Thread.currentThread().getName()+"\t"+num);

            condition.signalAll();

        } finally {
            lock.unlock();
        }
    }

    public void sub()throws InterruptedException{
        lock.lock();
        try {

            while(num==0){
                condition.await();
            }
            num--;
            System.out.println(Thread.currentThread().getName()+"\t"+num);
            condition.signalAll();

        } finally {
            lock.unlock();
        }

    }

}

```





main函数：线程抢占

```java
package demo;

import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class Demo3 {
    public static void main(String[] args) {

        Num2 num2 = new Num2();

        new Thread(()->{
            for (int i = 0; i <10 ; i++) {
                try {
                    num2.add();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"AA").start();


        new Thread(()->{
            for (int i = 0; i <10 ; i++) {
                try {
                    num2.sub();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"BB").start();


        new Thread(()->{
            for (int i = 0; i <10 ; i++) {
                try {
                    num2.add();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"CC").start();

        new Thread(()->{
            for (int i = 0; i <10 ; i++) {
                try {
                    num2.sub();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"DD").start();

    }
}



```





## 5、案例：修改标志位



题目描述：A打印 5次 -> B打印 10次  -> C打印 15次 ，按顺序轮流打印。



资源类：

```java
class Print{
    Lock lock = new ReentrantLock();
    int flag = 1; // A:1  , B:2 , C:3
    
    // 定义3把锁 给A、B、C
    Condition condition1 = lock.newCondition();
    Condition condition2 = lock.newCondition();
    Condition condition3 = lock.newCondition();

    public void printA5()throws InterruptedException{
        lock.lock();
        try {

            while(flag!=1){
                condition1.await();
            }

            for (int i = 0; i < 5; i++) {
                System.out.println("A");
            }
            
            // 修改标志位
            flag=2;
            // 1打印完，去唤醒2
            condition2.signal();

        } finally {
            lock.unlock();
        }
    }

    public void printB10()throws InterruptedException{
        lock.lock();
        try {

            while(flag!=2){
                condition2.await();
            }

            for (int i = 0; i < 10; i++) {
                System.out.println("B");
            }
            flag=3;
            condition3.signal();

        } finally {
            lock.unlock();
        }
    }


    public void printC15()throws InterruptedException{
        lock.lock();
        try {

            while(flag!=3){
                condition3.await();
            }

            for (int i = 0; i < 15; i++) {
                System.out.println("C");
            }
            flag=1;
            condition1.signal();

        } finally {
            lock.unlock();
        }
    }
}

```



main方法

```java
package demo;

import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class Demo4 {
    public static void main(String[] args) {

        Print print = new Print();

        new Thread(()->{
            for (int i = 0; i < 30; i++) {
                try {
                    print.printA5();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"A").start();


        new Thread(()->{
            for (int i = 0; i < 30; i++) {
                try {
                    print.printB10();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"B").start();


        new Thread(()->{
            for (int i = 0; i < 30; i++) {
                try {
                    print.printC15();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"C").start();


    }
}

```





## 6、锁对象的小结



普通的同步方法锁的是 `this `对象

static 同步方法锁的是` .class字节码` 对象

主要要判断是否锁住了同一把锁。



-   [多线程-8锁](https://blog.csdn.net/qq_35080796/article/details/112613751)

题目：多线程8锁
1、标准访问，请问先打印邮件还是短信？
2、邮件新增暂停4秒钟的方法，请问先打印邮件还是短信？
3、新增普通的hello方法，请问先打印邮件还是hello？`先 hello方法，因为普通法方法不加锁，无需判断锁的情况，可直接调用`
4、有两部手机，请问先打印邮件还是短信？
5、两个静态同步方法，同一部手机，请问先打印邮件还是短信？
6、两个静态同步方法，2部手机，请问先打印邮件还是短信？`5~6两题都是先打印邮件，因为锁住的是模板类`
7、1个静态同步方法,1个普通同步方法，1部手机，请问先打印邮件还是短信？`打印邮件，不同的2把锁，分别锁住了模板类和实例对象`
8、1个静态同步方法,1个普通同步方法，2部手机，请问先打印邮件还是短信？





## 7、如何解决集合类的线程安全问题



常见的线程安全异常：（并发修改异常）`java.util.concurrentModificationException`







>   List - 解决方案（常用的几种）：

-   加` 锁`
-   `Vecter` 来代替：性能差
-   调用`Collections工具类`的同步方法，如：Collections.synchronizedList(new List())
-   JUC包下的` CopyOnWriteArrayList`类：`写入时复制1份Object数组，写完后,将原容器的引用(指向自己的指针)指向新容器（利用Arrays.copyOf方法来复制）`



>   Set- 解决方案（常用的几种）：

-   `Collections.synchronizedSet(new Set());`
-   `CopyOnWriteArraySet`类



**hashset** 的底层是 **hashmap**，在**添加** hashset 的元素时，**传入的是 key**，而 value 是 写死的Object常量。

hashmap的初始容量 16，负载因子 0.75（容量到12就扩容1倍）

注意：arraylist 扩容1半



>   Map- 解决方案（常用的几种）：

-   JUC包下的 `concurrentHashMap`类





## 8、Callable接口

-   创建1个实现了Callable接口的普通类
-   new 1个 FutureTask对象（runnable接口的子类），传入Callable接口的普通类的对象
-   new 1个线程，传入 FutureTask对象







## 9、Volatile关键字

读不加锁，写加锁。



volatile：修饰变量时，保证**可见性、有序性**，让多个线程都能看到变量的真实情况（cpu直接与内存交互）

