---
title: Java-新特性
tag: Java
categories:
  - 后端
  - Java
  - Java SE
---

<h1>Java-新特性</h1>

[toc]



# 一、函数式接口



<u>**函数式接口**</u>：只有一个抽象方法的接口，可以使用`@FunctionalInterface`注解修饰。



## 1.1、Predicate 判断



```java
public static void main(String[] args) {
	// 数据源
    List<Integer> list1 = Arrays.asList(3,5,1,8,4,4);

    // 判断的规则
    Predicate<Integer> con = new Predicate<Integer>() {
        @Override
        public boolean test(Integer integer) {
            // 元素值大于5时，符合规则
            return integer > 5;
        }
    };

    // 判断每个元素是否符合大于5的规则
    List<Integer> resList = list1.stream()
        .filter(con::test)
        .collect(Collectors.toList());

    // [8]
    System.out.println(resList);

}
```





## 1.2、Supplier 获取



```java
public static void main(String[] args){
    
		Supplier<Random> s1 = Random::new;
        Random random = s1.get();
        System.out.println(random.nextInt(10));
    
}
```









# 二、Lambda 表达式



<u>**Lambda 表达式**</u> 一般用于<u>**函数式接口**</u>，可以简化<u>**匿名内部类**</u>的使用。



（1）定义一个函数式接口：

```java
@FunctionalInterface
public interface Op(){
    int op(int a,int b);
}
```



（2）使用 <u>**Lambda 表达式**</u> 重写 函数式接口中的<u>**抽象方法**</u>：

```java
public static void main(String[] args){
    
        Op addition = (int a,int b) -> a+b;
        int c = addition.op(1,2); 
    
}
```





# 三、方法引用



方法引用通过<u>**方法的名字**</u>来指向<u>**一个方法**</u>。

方法引用使用一对冒号` ::`。



调用：

> - 调用静态方法：`类名::方法名`
>
> - 调用成员方法：`对象名::方法名`
> - 调用构造函数：`类型::new`



```java
public static void main(String args[]){
    
    List<String> names = new ArrayList();

    names.add("Google");
    names.add("Runoob");
    names.add("Taobao");
    names.add("Baidu");
    names.add("Sina");

    // 使用方法的引用
    names.forEach(System.out::println);
}
```











# 四、流式编程



Collection集合类可以通过调用`stream()`得到数据流。



## 3.1、打印

```java
public static void main(String[] args){ 
    
    // 注意：Arrays.asList() 得到的是一个不可变的列表
	List<Integer> list1 = Arrays.asList(3,5,1,8,4,4);
    
    
    // 打印集合内的每个元素
    list1.stream()
         .forEach(item->System.out.println(item));
    
    // 打印集合内的每个元素
    list1.stream()
         .forEach(System.out::println);   
    
}
```



## 3.2、过滤



```java
public static void main(String[] args){ 
    
    // 注意：Arrays.asList() 得到的是一个不可变的列表
	List<Integer> list1 = Arrays.asList(3,5,1,8,4,4);
    
    // 过滤出大于等于4的数，组成集合
    List<Integer> resList = list1.stream()
        .filter(item -> item >= 4)
        .collect(Collectors.toList());
    
    // [5,8,4,4]
    System.out.println(resList);
    
}
```





## 3.3、处理



```java
public static void main(String[] args){ 
    
    // 注意：Arrays.asList() 得到的是一个不可变的列表
	List<Integer> list1 = Arrays.asList(3,5,1,8,4,4);
    
    // 将每个元素扩大1倍
    List<Integer> resList = list1.stream()
        .map(item -> item * 2)
        .collect(Collectors.toList());
    
    // [6, 10, 2, 16, 8, 8]
    System.out.println(resList);
}
```





## 3.4、判断



判断是否<u>**全部元素**</u>符合指定的条件：

```java
public static void main(String[] args){ 
    
    // 注意：Arrays.asList() 得到的是一个不可变的列表
	List<Integer> list1 = Arrays.asList(3,5,1,8,4,4);
    
    // 判断集合中的元素是否全部大于等于1
    boolean b = list1.stream()
        .allMatch(item -> item >= 1);

    // true
    System.out.println(b);
    
}    
```





判断是否有<u>**一个元素**</u>符合指定的条件：

```java
public static void main(String[] args){ 
    
    // 注意：Arrays.asList() 得到的是一个不可变的列表
	List<Integer> list1 = Arrays.asList(3,5,1,8,4,4);
    
    // 判断集合中的元素是否有一个元素大于等于1
    boolean b = list1.stream()
        .anyMatch(item -> item >= 1);

    // true
    System.out.println(b);
    
}    
```





判断是否<u>**没有元素**</u>符合指定的条件：

```java
public static void main(String[] args){ 
    
    // 注意：Arrays.asList() 得到的是一个不可变的列表
	List<Integer> list1 = Arrays.asList(3,5,1,8,4,4);
    
    // 判断集合中的元素是否没有元素大于20
    boolean b = list1.stream()
        .noneMatch(item -> item > 20);

    // true
    System.out.println(b);
    
}   
```





## 3.5、去重



```java
public static void main(String[] args){ 
    
    // 注意：Arrays.asList() 得到的是一个不可变的列表
	List<Integer> list1 = Arrays.asList(3,5,1,8,4,4);
    
    // 去重
    List<Integer> resList = list1.stream()
        .distinct()
        .collect(Collectors.toList());

    // [3, 5, 1, 8, 4]
    System.out.println(resList);
    
}   
```



## 3.6、查找



查找第一个元素：

```java
public static void main(String[] args){ 
    
    // 注意：Arrays.asList() 得到的是一个不可变的列表
	List<Integer> list1 = Arrays.asList(3,5,1,8,4,4);
    
	// 查找第一个元素
    List<Integer> resList = list1.stream()
        .findFirst()
        .stream()
        .toList();
    
	// [3]
    System.out.println(resList);
    
}   
```





## 3.7、获取指定数量的元素



```java
public static void main(String[] args){ 
    
    // 注意：Arrays.asList() 得到的是一个不可变的列表
	List<Integer> list1 = Arrays.asList(3,5,1,8,4,4);
    
    // 获取3个元素
    List<Integer> resList = list1.stream()
        .limit(3)
        .collect(Collectors.toList());

    // [3, 5, 1]
    System.out.println(resList);
    
}   
```





## 3.8、排序



`sorted()`方法的参数是一个`compator`接口，可以用 lambda 表达式简化`compator`接口中的比较规则的实现。



升序（默认）：

```java
public static void main(String[] args){ 
    
    // 注意：Arrays.asList() 得到的是一个不可变的列表
	List<Integer> list1 = Arrays.asList(3,5,1,8,4,4);
    
    // 升序，
    List<Integer> resList = list1.stream()
        .sorted()
        .collect(Collectors.toList());

    // [1, 3, 4, 4, 5, 8]
    System.out.println(resList);
    
}   
```





降序：

```java
public static void main(String[] args){ 
    
    // 注意：Arrays.asList() 得到的是一个不可变的列表
	List<Integer> list1 = Arrays.asList(3,5,1,8,4,4);
    
    // 降序，sorted()的参数是对`compator`接口compare方法的重写（比较规则）
    // a-b：升序，b-a:降序
    List<Integer> resList = list1.stream()
        .sorted((a,b)-> b-a)
        .collect(Collectors.toList());

    System.out.println(resList);
    
}   
```

