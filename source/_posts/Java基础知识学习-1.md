---
title: Java基础知识学习-1
date: 2019-03-04 16:21:11
top:  10
tags:
- Java
categories:
- 编程语言
---

## Java 语言的特点

1. 开源
2. 一次编写，到处运行——跨平台性
3. 与C/C++相似的语法结构
4. 强类型
5. 面向对象
6. 丰富的库
7. 使用垃圾回收机制进行内存管理
8. 异常处理
9. 并发处理
10. 使用包对类进行分类

## 基本概念

1. JDK
  Java development kit： Java 开发工具包

2. JRE
  Java runtime environment：  Java 运行环境

3. JVM
  Java virtual machine：  Java 虚拟机

三者的关系：
{% asset_img 1.png [三者关系] %}

## 编写 Java 程序

### 使用记事本编写 Java 程序

1. 编写源程序 MyProgram.java
2. 使用 javac 命令编译源程序，生成 MyProgram.calss 字节码文件
```
javac MyProgram.java
```
3. 使用 java 命令运行程序
```
java MyProgram
```
{% asset_img 2.png [基本流程] %}

### 使用 Eclipse 编写 Java 程序

集成开发环境(IDE)是一类软件，将程序开发环境和调试环境集合在一起，提高开发效率。

1. 创建 Java 项目
2. 创建程序包
3. 编写 Java 源程序
4. 运行 Java 程序

{% asset_img 3.png [基本流程] %}

## 输入与输出
```java
import java.util.Scanner;
public class Hello{
  public static void main(String[] args) {
    Scanner stdIn = new Scanner(System.in);
    
    // 读取整数
    int a = stdIn.nextInt();
    // 读取小数
    double b = stdIn.nextDouble();
    // 读取字符串
    // 使用此方法读取字符串时，空白符和制表符被视为字符串的分隔符，因此如果输入中包含空格或者制表符，需要使用nextLine()
    String s = stdIn.next();
    // 读取一整行
    String sl = stdIn.nextLine();
    
    System.out.println(a);
    System.out.println(b);
    System.out.println(s);
    System.out.println(sl);
  }
}
```

程序输出的方法有很多，比如：
- `System.out.print()`    直接输出
- `System.out.println()`  输出一行
- `System.out.printf()`   与C语言的printf()函数类似

读取输入可以使用 Java 的 Scanner 类，使用方法如上。

