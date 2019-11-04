---
title: Java基础知识学习-4
date: 2019-03-14 19:37:22
top: 10
tags:
- Java
categories:
- 编程语言
---

## 封装

在面向对象程式设计方法中，封装是指一种将抽象性函式接口的实现细节部份包装、隐藏起来的方法。

封装可以被认为是一个保护屏障，防止该类的代码和数据被外部类定义的代码随机访问。

封装的优点：
- 良好的封装能够减少耦合
- 类内部的结构可以自由修改
- 可以对成员变量进行更精确的控制
- 隐藏信息，实现细节

## 实现封装

修改属性的可见性来限制对属性的访问（一般限制为private），例如：

```java
public class Person {
  private int age;
  private String name;
}
```

将 name 和 age 属性设置为私有的，只能本类才能访问，其他类都访问不了，如此就对信息进行了隐藏。同时提供对外的公共方法访问，也就是创建一对赋取值方法，用于对私有属性的访问。

```java
public class Person {
  private int age;
  private String name;

  public int getAge() {
    return age;
  }
  public String getName() {
    return name;
  }
  public void setAge(int age) {
    this.age = age;
  }
  public void setName(String name) {
    this.name = name;
  }
}
```

## Java中的成员内部类

当一个类包含另一个类时，内部类如何访问外部类中的成员属性？如何在外部调用内部类中的方法？

```java
// Person.java
public class Person {
  // 外部类的私有属性
  private String name = "Sillywa";
  // 外部类的成员属性
  int age = 20;
  
  // 成员内部类
  public class Inner {
    String name = "wenwen";	
    
    // 内部类中的方法
    public void show() {
      System.out.println("外部类中的name：" + Person.this.name);
      System.out.println("外部类中的age：" + Person.this.age);
      System.out.println("内部类中的name：" + name);
    }
  }
}
```
```java
// Main.java
public class Main {
  
  public static void main(String[] args) {
    // 创建外部类的对象
    Person person = new Person();
    
    // 创建内部类的对象
    Inner inn = person.new Inner();
    inn.show();
  }
}
```
可以看出：

*** 内部类通过 外部类名.this.成员属性 访问外部类中的成员属性。 ***

*** 当需要调用内部类中的方法时，需要先实例化外部类，再实例化内部类。 ***

## Java中的静态内部类

静态内部类如何访问外部类的变量？

```java
//Person.java
public class Person {
  // 外部类的私有静态属性
  private static String name = "Sillywa";
  private static String city = "wuhan";
  // 外部类的成员属性
  int age = 20;
  
  // 静态内部类
  public static class Inner {
    String name = "wenwen";
    // 内部类中的方法
    public void show() {
      // 静态内部类访问外部的非静态成员： `new 外部类().成员`
      System.out.println("外部类中的age：" + new Person().age);

      // 内部类没有与该成员同名的变量： 直接通过 `变量名访问`
      System.out.println("外部类中的age：" + city);

      // 内部类存在与该成员同名的变量： 通过 `类名.静态成员访问`
      System.out.println("外部类中的name：" + Person.name);
    }
  }
}

```
*** 调用时需要实现引入静态内部类 ***

```java
// Main.java
import packageName.Person.Inner;
public class Main {  
  public static void main(String[] args) {   
    // 创建内部类的对象
    Inner inn = new Inner();
    inn.show();
  }
}
```
两种情况：
  1. 静态内部类访问外部的非静态成员： `new 外部类().成员`
  2. 静态内部类访问外部的静态成员:
    - 内部类没有与该成员同名的变量： 直接通过 `变量名访问`
    - 内部类存在与该成员同名的变量： 通过 `类名.静态成员访问`

## Java中的方法内部类

*** 方法内部类就是内部类定义在外部类的方法中，方法内部类只在该方法的内部可见，即只在该方法内可以使用。 ***

```java
//Person.java
public class Person {
  // 外部类中的方法
  public void show() {
    // 方法内部类
    class MInner {
      int score = 83;
      public int getScore() {
        return score + 10;
      }
    }
    // 创建方法内部类的实例
    MInner minner = new MInner();
    minner.getScore();
  }
}
```

一定注意哦：*** 由于方法内部类不能在外部类的方法以外的地方使用(相当于“局部类”)，因此方法内部类不能使用访问控制符和static修饰符。 ***



