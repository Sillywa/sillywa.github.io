---
title: Java基础知识学习-5
date: 2019-03-15 12:39:53
top: 10
tags: 
- Java
categories:
- 编程语言
---

## 继承

在 Java 中，类的继承只能是单一继承，也就是说，一个子类只能拥有一个父类。

Java 中用 extends 关键字来实现继承。

```java
// 父类
public class Father {
  String name;
  int age;

  public void sayName() {
    System.out.println(name);
  }
}

```
```java
// 子类
public class Son extends Father {

}
```

继承的特点：
1. 子类拥有父类非 private 的属性和方法
2. 子类可以拥有自己的属性和方法
3. 子类可以用自己的方式实现父类的方法
4. Java 的继承是单继承，但是可以多重继承，单继承就是一个子类只能继承一个父类，多重继承就是，例如A类继承B类，B类继承C类，所以按照关系就是C类是B类的父类，B类是A类的父类

Java 中用 extends 关键字来实现继承，用 super 关键字来实现对父类成员的访问，用来引用当前对象的父类，用 this 关键字指向自己的引用。

```java
// 父类
public class Father {
  String name = "father";
  int age = 50;

  public void sayName() {
    System.out.println(name);
  }
}

```
```java
// 子类
public class Son extends Father {
  String name = "son";
  int age = 23;
  public void sayName() {
    System.out.println(name);
  }
  void say() {
    super.sayName();   // father
    this.sayName();   // son
    sayName();        // son
  }
}
```

*** 子类是不能继承父类的构造方法的，它只是隐式调用。如果父类的构造方法带有参数，则必须在子类的构造器中显式通过 super 关键字调用父类的构造方法并配有适当的参数。且必须在子类构造方法的第一行 ***

```java
// 父类
public class Father {
  String name;
  int age;

  // 父类带参数构造方法
  public Father(String name,int age) {
    this.name = name;
    this.age = age;
  }

  public void sayName() {
    System.out.println(name);
  }
}

```
```java
// 子类
public class Son extends Father {
  String name = "son";
  int age = 23;

  // 子类构造方法
  public Son(String name,int age) {
    // 显示调用 super
    super(name,age);
  }
  public void sayName() {
    System.out.println(name);
  }
  void say() {
    super.sayName();   // father
    this.sayName();   // son
    sayName();        // son
  }
}
```

如果父类构造方法没有参数，则在子类的构造方法中不需要使用 super 关键字调用父类构造方法，系统会自动调用父类的无参构造方法。

Java 中所有的类都继承 Object 类，如果一个类没有使用 extends 关键字明确标识继承另一个类，那么这个类默认继承 Object 类。

Object 类的 toString() 方法返回对象的哈希 code 码（对象地址字符串）。

Object 类的 equals() 方法比较对象的引用是否指向同一块地址。

