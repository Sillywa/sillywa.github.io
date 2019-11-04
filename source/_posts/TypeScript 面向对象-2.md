---
title: TypeScript 面向对象(二)
date: 2019-09-08 15:42:57
top: 10
tags:
- TypeScript
categories:
- 前端
---

前面介绍了 TypeScript 面向对象的基本特性，现在我们接着前面继续学习 TypeScript 面向对象的其他特性，包含 readonly 修饰符、参数属性、存储器、静态属性、抽象类。

## readonly 修饰符

readonly 关键字将属性设置为只读的，只读属性必须在声明时或构造函数里被初始化。

```ts
class Person {
  readonly name: string;
  constructor(my_name: string, readonly age: number) {
    this.name = my_name;
  }
  getName(): string {
    return this.name;
  }
  getAge(): number {
    return this.age;
  }
}

let p1 = new Person("sillywa", 23);

p1.getName();
p1.getAge();
p1.name;
p1.age;

p1.name = "hahah";      // 错误，name是只读属性
p1.age = 45;            // 错误，age是只读属性

```

在上述例子中，name属性为只读，在构造函数外声明，age 属性也是只读，在构造函数里初始化，需要注意的是，在构造函数里初始化的属性不用再使用 this 关键字为其赋值。

除 readonly 修饰符外，其它带有修饰符的属性也可以在构造函数里初始化，这种初始化属性的方式称为 参数属性：

## 参数属性

```ts
class Person {
  constructor(public name:string, private age: number, protected sex: string) {

  }
  getInfo(): string {
    return `my name is ${this.name},and I am ${this.age} years old, I am ${this.sex}`;
  }
}

let p1 = new Person("Sillywa",23,"男")

```

参数属性通过给构造函数参数前面添加一个访问限定符来声明。 使用 private 限定一个参数属性会声明并初始化一个私有成员；对于 public 和 protected 来说也是一样。

## 存取器

TypeScript 支持通过 getters/setters 来截取对对象成员的访问。 它能帮助你有效的控制对对象成员的访问。

```ts 
const fullNameMaxLength = 10;

class Employee {
    private _fullName: string;

    get fullName(): string {
        return this._fullName;
    }

    set fullName(newName: string) {
        if (newName && newName.length > fullNameMaxLength) {
            throw new Error("fullName has a max length of " + fullNameMaxLength);
        }
        
        this._fullName = newName;
    }
}

let employee = new Employee();
employee.fullName = "Bob Smith";
if (employee.fullName) {
    console.log(employee.fullName);
}}

```

首先，存取器要求你将编译器设置为输出ECMAScript 5或更高。 不支持降级到ECMAScript 3。 其次，只带有 get不带有 set的存取器自动被推断为 readonly。 这在从代码生成 .d.ts文件时是有帮助的，因为利用这个属性的用户会看到不允许够改变它的值。

## 静态属性

前面我们讨论的都是类的实例成员，即那些仅当实例被初始化的时候才会被初始化的属性。

我们也可以使用 static 关键字创建类的静态成员，*** 这些属性存在于类本身上面而不是类的实例上 ***，我们使用 类名.静态成员 来访问这些静态成员。

```ts
class Person {
  static fullName:string = "sillywa"
  constructor(public age:number) {

  }
  getInfo(): string {
    return `my name is ${Person.fullName},age is ${this.age}`;
  }
}

let p1 = new Person(23);

p1.getInfo();
```

编译之后生成如下 JavaScript 代码：

```js
var Person = /** @class */ (function () {
    function Person(age) {
        this.age = age;
    }
    Person.prototype.getInfo = function () {
        return "my name is " + Person.fullName + ",age is " + this.age;
    };
    Person.fullName = "sillywa";    // 静态属性
    return Person;
}());
var p1 = new Person(23);
p1.getInfo();
```

## 抽象类

抽象类做为其它子类的父类使用。 它们一般不会直接被实例化。 不同于接口，抽象类可以包含成员的实现细节。 

abstract 关键字是用于定义抽象类和在抽象类内部定义抽象方法。

*** 需要注意的是如果一个子类继承了一个抽象类，则该子类必须实现该抽象类中的抽象方法。 ***

```ts
abstract class Person {
  constructor(public name: string){ };

  getName(): string {
    return this.name;
  }

  abstract getAge(): number;  // 必须在子类中实现
}

class Student extends Person {
  constructor(my_name: string, public age: number) {
    super(my_name);
  }
  getAge(): number {
    return this.age;
  }
}

let stu1 = new Student("Sillywa",23);

stu1.getAge();
stu1.getName()
```