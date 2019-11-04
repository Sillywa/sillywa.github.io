---
title: TypeScript 面向对象(一)
date: 2019-09-07 20:32:38
top: 10
tags:
- TypeScript
categories:
- 前端
---


TypeScript 可以采用面向对象的方式来进行编程，以下介绍一些面向对象的基本特性，包含类、继承、super关键字和访问控制修饰符。

## 类

TypeScript 使用如下方式声明一个类：

```ts
class Person {
  name: string;
  constructor(my_name: string) {
    this.name = my_name;
  }
  sayName():string {
    return `my name is ${this.name}`;
  }
}

let p1 = new Person("Sillywa");

p1.sayName();    
p1.name;  
```

我们声明了一个 Person 类，他有一个 name 属性，一个构造函数和一个 sayName 方法。

我们在引用任何一个类成员的时候都使用了 this 关键字，表示我们要访问的类成员。

接着我们用 new 关键字创建了一个 Person 类的实例对象 p1，由于其构造函数接受一个 my_name 参数，因此我们在实例化 p1 时，传递给一个参数。

最后 p1 就可以使用 Person 类的属性和方法。

以上代码通过 TypeScript 的编译会得到如下 JavaScript 代码：

```js
var Person = /** @class */ (function () {
    function Person(my_name) {
        this.name = my_name;
    }
    Person.prototype.sayName = function () {
        return "my name is " + this.name;
    };
    return Person;
}());
var p1 = new Person("Sillywa");
p1.sayName();
p1.name;
```

## 继承

在 TypeScript 里，我们可以使用常用的面向对象模式。 基于类的程序设计中一种最基本的模式是允许使用继承来扩展现有的类。

```ts

class Person {
  sayName():string {
    return "name";
  }
}

class Student extends Person {
  saySchool():string {
    return "school";
  }
}

let stu1 = new Student();
stu1.saySchool();
stu1.sayName();
```

与其他语言类似，在 TypeScript 中也是使用 extends 关键字实现继承。在上述例子中，Student 类继承 Person 类，Student 为子类或派生类，Person 类为父类或基类。

需要注意的是子类除了不能继承父类的私有成员(方法和属性)和构造函数，其他的都可以继承。

TypeScript 一次只能继承一个类，不支持继承多个类，但 TypeScript 支持多重继承（A 继承 B，B 继承 C）。

## super 关键字

*** 如果子类包含了一个构造函数，它必须调用 super()，它会执行父类的构造函数。 而且，在构造函数里访问 this 的属性之前，我们一定要调用 super()。 这个是TypeScript强制执行的一条重要规则。 ***

```ts
class Person {
  name: string;
  constructor(my_name: string) {
    this.name = my_name;
  }
  sayName(): string {
    return this.name;
  }
}

class Teacher extends Person {
  constructor(tea_name: string) {
    // 派生类包含了一个构造函数，它必须调用 super()
    super(tea_name);
  }
  sayTeaName(): void {
    console.log("teacher name");
    console.log(super.sayName());    // 调用父类的函数
  }
}

class Student extends Person {
  school: string;
  constructor(stu_name: string, my_school: string) {
    // 在构造函数里访问 this 的属性之前，我们一定要调用 super()
    super(stu_name);
    this.school = my_school;
  }
  saySchool():string {
    return this.school;
  }
}

let tea1 = new Teacher("math teacher");
let stu1 = new Student("middle student","peaking university");


tea1.sayTeaName();
console.log(stu1.saySchool());
```

以上我们声明了三个类，其中 Teacher 类和 Student 类都继承 Person 类。由于子类包含了一个构造函数，它必须调用 super()，执行父类的构造函数。如果构造函数接受参数，需要显式传递参数给 super 方法。

从 Teacher 类我们可以看出，super 不仅可以调用父类的构造函数，还可以调用父类的其它共有方法。

从 Student 类可以看出，当子类包含自己的属性时，需要在访问子类的属性之前调用 super()。

## 访问控制修饰符

TypeScript 中，可以使用访问控制符来保护对类、变量、方法和构造方法的访问。TypeScript 支持 3 种不同的访问权限。、

* public：类的所有成员如果没有访问控制符的话，默认为 public，共有，可以在任何地方被访问。

* private：私有，只能被其定义所在的类访问。

* protected：受保护，可以被其自身以及其子类或父类访问。

前面我们写的类的成员都没有访问控制修饰符，因此默认为 public，可以在任何地方被访问。

```ts
class Person {
  private name: string;
  constructor(my_name: string) {
    this.name = my_name;
  }
  logName() {
    console.log(this.name);
  }
}

class Student extends Person{
  constructor(stu_name: string) {
    super(stu_name);
  }
  sayName() {
    console.log(this.name);    // 报错，私有成员无法在其它类中被访问
  }
}

let stu1 = new Student("Sillywa");
stu1.sayName()

let p1 = new Person("Sillywa father");
console.log(p1.name);       // 报错，私有成员只能在其所属类中被访问

p1.logName();               // 正确，私有成员只能在其所属类中被访问

```

以上代码我们定义了一个 Person 类，其有一个私有属性 name，我们可以看到，无论我们通过何种方法，私有属性 name 只能在其所属类中被访问，其他地方都访问不了。

而 protected 修饰符与 private 有一点不同，protected 成员可以被其自身以及其子类或父类访问。

同样是以上代码，我们将 name 的修饰符换为 protected

```ts
class Person {
  protected name: string;
  constructor(my_name: string) {
    this.name = my_name;
  }
  logName() {
    console.log(this.name);
  }
}

class Student extends Person{
  constructor(stu_name: string) {
    super(stu_name);
  }
  sayName() {
    console.log(this.name);    // 正确，受保护成员可以在其子类中被访问
  }
}

let stu1 = new Student("Sillywa");
stu1.sayName()

let p1 = new Person("Sillywa father");
console.log(p1.name);       // 报错，受保护成员只能在其所属类或其子类或父类中被访问

p1.logName();               // 正确，受保护成员可以在其所属类中被访问
```
