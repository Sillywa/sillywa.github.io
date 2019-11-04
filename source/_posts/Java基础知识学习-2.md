---
title: Java基础知识学习-2
date: 2019-03-10 16:36:49
top: 10
tags:
- Java
categories:
- 编程语言
---

## 数组

### 声明数组

```java
int[] arr1;     // 建议使用
String arr2[];

// 指定数组长度
arr1 = new int[5];
arr2 = new String[5];

// 声明的同时指定数组长度
int[] arr3 = new int[5];

// 声明并赋值时不能指定长度
int[] arr4 = new int[] {1,2,3,4,5}
```

### For-Each 循环

JDK 1.5 引进了一种新的循环类型，被称为 For-Each 循环或者加强型循环，它能在不使用下标的情况下遍历数组。

语法格式如下：

```
for(type element : array) {
  System.out.println(element);
}
```

实例

```java
int[] arrs = {0,1,2};
for(int item : arrs) {
  System.out.println(item);
}
```

### Arrays 类提供的方法

首先引入 Arrays 类

```java
import java.util.Arrays;
```

1.`public static void sort(Object[] a)`

对指定对象数组根据其元素的自然顺序进行升序排列。同样的方法适用于所有的其他基本数据类型（Byte，short，Int等）。

```java
int[] arrs = {1,4,63,2,35,7};
Arrays.sort(arrs);
```
 
2.`public static void fill(int[] a, int val)`

将指定的 int 值分配给指定 int 型数组指定范围中的每个元素。同样的方法适用于所有的其他基本数据类型（Byte，short，Int等）。

```java
int[] arrs = new int[5];
int a = 8;
Arrays.fill(arrs,a);
for (int item : arrs) {
  System.out.println(item);
}
// 8
// 8
// 8
// 8
// 8
```

3.`public static boolean equals(long[] a, long[] a2)`

如果两个指定的 long 型数组彼此相等，则返回 true。如果两个数组包含相同数量的元素，并且两个数组中的所有相应元素对都是相等的，则认为这两个数组是相等的。换句话说，如果两个数组以相同顺序包含相同的元素，则两个数组是相等的。同样的方法适用于所有的其他基本数据类型（Byte，short，Int等）。

```java
int[] arrs1 = {1,2,3,4,5};
int[] arrs2 = {1,2,3,4,5};
int[] arrs3 = {1,2,3,4};
boolean a = Arrays.equals(arrs1, arrs2);
boolean b = Arrays.equals(arrs1, arrs3);
System.out.println(a);    // true
System.out.println(b);    // false
```

4.`public static int binarySearch(Object[] a, Object key)`

用二分查找算法在给定数组中搜索给定值的对象(Byte,Int,double等)。数组在调用前必须排序好的。如果查找值包含在数组中，则返回搜索键的索引；否则返回 (-(数组长度) - 1)。

```java
int[] arrs = {1,2,3,4};
int a = 3;
int b = 10;
int index1 = Arrays.binarySearch(arrs, a);
int index2 = Arrays.binarySearch(arrs, b);
System.out.println(index1);
System.out.println(index2);
```

## 方法

### 方法声明

一般情况下，定义一个方法包含以下语法

```
修饰符 返回值类型 方法名(参数类型 参数名) {
  ···
  方法体
  ···
  return 返回值;
}
```

### 方法重载

如果一个类中有两个或两个以上方法名相同、方法参数的个数、顺序或者类型不同的方法，则称为方法的重载。

```java
// 无参数方法
public void show() {
  System.out.println("hello");
}
// 重载show方法，一个参数方法
public void show(String name) {
  System.out.println("hello" + name);
}
// 重载show方法，两个参数
public void show(String name,int age) {
  System.out.println("hello" + name);
  System.out.println(age);
}
// 重载show方法，两个参数顺序不同
public void show(int age,S tring name) {
  System.out.println("hello" + name);
  System.out.println(age);
}
```

*** 当重载方法被调用时，Java 会根据参数的个数和类型来判断应该调用哪个重载方法，参数完全匹配的方法将会被执行。***

*** 判断方法重载的依据 ***

1. 必须在同一个类中
2. 方法名相同
3. 方法参数个数、顺序或类型不同
4. 与方法的修饰符或返回值没有关系

### 可变参数

JDK 1.5 开始，Java支持传递同类型的可变参数给一个方法。

如果一个方法的参数不确定，则可使用可变参数，一个方法中只能指定一个可变参数，它必须是方法的最后一个参数。任何普通的参数必须在它之前声明。

方法的可变参数的声明如下所示：

```
typeName... parameterName
```

在方法声明中，在指定参数类型后加一个省略号(...) 。

```java
public static void main(String[] args) {
  int[] arr = {1,9,5,8,20,6};
  printMax(1,9,5,8,20,6);
  printMax(arr);
}
public static void printMax(int... numbers) {
  if (numbers.length == 0) {
    System.out.println("No data");
    return;
  }
  int max = numbers[0];
  for (int item : numbers) {
    max = item > max ? item : max;
  }
  System.out.println(max);
}
```