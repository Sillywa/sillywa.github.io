---
title: Java基础知识学习-3
date: 2019-03-11 19:19:39
top: 10
tags:
- Java
categories:
- 编程语言
---

## 类与对象

类是一个模板，它描述一类对象的行为和状态。对象是类的一个实例。

## 类的定义
```java
public class Person {
  int age;
  String name;
  void sayAge() {
    System.out.println("age:" + age);
  }
}
```

一个类包含以下类型的变量：

1.*** 局部变量 ***：在方法、构造方法或者语句块中定义的变量被称为局部变量。变量声明和初始化都是在方法中，方法结束后，变量就会自动销毁。
2.*** 成员变量 ***：成员变量是定义在类中，方法体之外的变量。这种变量在创建对象的时候实例化。成员变量可以被类中方法、构造方法和特定类的语句块访问。
3.*** 类变量 ***：类变量也声明在类中，方法体之外，但必须声明为static类型。

## 构造方法

每个类都有构造方法。如果没有显式地为类定义构造方法，Java编译器将会为该类提供一个默认构造方法。

在创建一个对象的时候，至少要调用一个构造方法。*** 构造方法的名称必须与类同名，一个类可以有多个构造方法 ***。

```java
public class Person {
  int age;
  String name;
  
  // 构造方法与类同名
  public Person(int myAge, String myName) {
    age = myAge;
    name = myName;
  }
  void sayAge() {
    System.out.println("age:" + age);
  }
}
```

## 创建对象

```java
public class Person {
  int age;
  String name;
  
  // 构造方法与类同名
  public Person(int myAge, String myName) {
    age = myAge;
    name = myName;
  }
  void sayAge() {
    System.out.println("age:" + age);
  }

  // 主函数
  public static void main(String[] args) {
    // 实例化对象
    Person person = new Person(12,"Sillywa");

    person.sayAge()
  }
}
```

## static 用于创建静态变量和静态方法

文件结构，一个 package 下面有以下三个类，一个 package 下的所有类都是相互可见的:
  - `Main.java`     程序入口
  - `Person.java`   Person类
  - `Dog.java`      Dog类

*** 对各种变量而言，成员变量只在本类中可以访问，而用 static 声明的静态变量或方法在同一个 package 下的所有类都可以访问，相当于该 package 下的全局变量或方法。 ***

*** 因此，对于静态变量或静态方法，应使用类名访问。 ***

```java
// Main.java
public class Main {
  public static void main(String[] args) {
    System.out.println(Person.allAge);
    System.out.println(Dog.allAge);
  }
}
```
```java
// Person.java
public class Person {
  int age;
  String name;
  // 声明静态变量
  static int allAge = 70;
  // 构造方法与类同名
  public Person(int myAge, String myName) {
    age = myAge;
    name = myName;
  }
  void sayAge() {
    System.out.println("person age:" + age);
  }
}
```
```java
// Dog.java
public class Dog {
  int age;
  String name;
  // 声明静态变量
  static int allAge = 10;
  // 构造方法与类同名
  public Dog(int myAge, String myName) {
    age = myAge;
    name = myName;
  }
  void sayAge() {
    System.out.println("dog age:" + age);
  }
}
```

*** 静态方法可以直接调用同类中的静态成员，但不能直接调用非静态成员。 ***

*** 普通方法中可以直接使用静态或非静态变量或方法。 ***

```java
// Person.java
public class Person {
  int age;
  String name;
  static int allAge = 70;
  // 构造方法与类同名
  public Person(int myAge, String myName) {
    age = myAge;
    name = myName;
  }
  void sayName() {
    // 普通方法中可以直接使用静态或非静态变量或方法
    System.out.println("person name:" + name);
    System.out.println("person allAge:" + allAge);
  }
  
  static void sayAllAge() {
    System.out.println("person allAge:" + allAge);
  }
  // 静态方法可以直接调用同类中的静态成员，但不能直接调用非静态成员。
  static void sayAge() {
    System.out.println("person age:" + age);    // 调用非静态变量，报错
    sayName();                                  // 调用非静态方法，报错
    sayAllAge();                                // 调用静态方法，成功
  }
}
```

*** 如果在静态方法中想要调用非静态成员，需先实例化对象。 ***

```java
// Person.java
public class Person {
  int age;
  String name;
  static int allAge = 70;
  // 构造方法与类同名
  public Person(int myAge, String myName) {
    age = myAge;
    name = myName;
  }
  void sayName() {
    System.out.println("person name:" + name);
    System.out.println("person allAge:" + allAge);
  }
  
  static void sayAllAge() {
    System.out.println("person allAge:" + allAge);
  }
  static void sayAge() {
    // 在静态方法中想要调用非静态成员，需先实例化对象。
    Person person = new Person(12,"Sillywa");           // 实例化对象
    System.out.println("person age:" + person.age);    // 调用非静态变量，成功
    person.sayName();                                  // 调用非静态方法，成功
    sayAllAge();                                // 调用静态方法，成功
  }
}
```

## 使用 static 静态初始化块

*** 需要特别注意：静态初始化块只在类加载时执行，且只会执行一次，同时静态初始化块只能给静态变量赋值，不能初始化普通的成员变量。 ***

```java
// Person.java
public class Person {
  int age;
  static String name;
  static int allAge;
  {
    age = 10;
    System.out.println("为普通变量赋值");
  }
  static {
    allAge = 90;
    name = "Sillywa";
    System.out.println("为静态变量赋值");
  }
  
}
```

```java
// Main.java
public class Main {
  public static void main(String[] args) {
    Person person1 = new Person();
    Person person2 = new Person();
  }
}
```

输出结果：

```
为静态变量赋值
为普通变量赋值
为普通变量赋值
```

*** 可以看出，静态赋值最先执行，当实例化两个对象时，静态初始化只被执行一次。 ***




