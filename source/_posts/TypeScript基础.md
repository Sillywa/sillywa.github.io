---
title: TypeScript基础
date: 2019-09-05 14:01:50
top: 10
tags:
- TypeScript
categories:
- 前端
---
## TypeScript 介绍

TypeScript 是微软开发的自由和开源的编程语言，它是 JavaScript 的超集。TypeScript 在 JavaScript 的基础上添加了可选的静态类型和基于类的面向对象编程。

TypeScript 是基于 JavaScript 的，在运行时需要先编译成 JavaScript 代码，其设计目的是开发大型应用，便于多人协作。

与 JavaScript 的对比：

* TypeScript 更适合开发大型应用。
* TypeScript 是 JavaScript 的超集，可以编译成纯 JavaScript 代码。
* 任何可以运行 JavaScript 的地方都可以运行 TypeScript 代码。
* 提供类、模块和接口等，能更好的构建和维护组件。

其给 JavaScript 添加了一些语言扩展，包括：

* 类型批注和编译时类型检查
* 类型推断
* 类型擦除
* 接口
* 枚举
* Mixin
* 泛型编程
* 名字空间
* 元组
* Await

同时其从 ECMAScript5 移植了以下语法：

* 类
* 模块
* lambda 函数的箭头语法
* 可选参数及默认参数

## TypeScript 安装

安装 TypeScript 前，需先安装 node，然后使用 npm 进行 TypeScript 的安装：

```
npm install -g typescript
```

安装完成之后使用 tsc 命令查看版本号以及检查是否安装成功：

```
tsc -v
```

然后编写第一个 TypeScript 程序 hello.ts，以 .ts 结尾：

```ts
let str:string = "hello TypeScript";
console.log(str);
```

然后将 hello.ts 文件编译成 js 文件，再运行该 js 文件：

```
tsc hello.ts
node hello.js
```

## TypeScript 变量类型

TypeScript 包含如下数据类型：

* any：任意类型
* number：数字类型
* string：字符串类型
* boolean：布尔类型
* 数组类型
* 元组
* enum：枚举
* void：用于标识方法的返回值，void 标识该方法没有返回值
* null：表示一个空对象引用
* undefined：初始化变量为一个未定义的值
* never：never是其他类型（包括 null 和 undefined ）的子类型，代表从不会出现的值

### any 类型

任意值是 TypeScript 针对编程时类型不明确的变量使用的一种数据类型，它常用于以下三种情况。

1. 变量的值会动态改变时，比如来自用户的输入，任意值类型可以让这些变量跳过编译阶段的类型检查

```ts
let x:any = 1;      // 数字类型
x = "hello";        // 字符串
x = false;          // 布尔类型
```

2. 定义存储各种类型数据的数组时

```ts
let arrayList:any[] = [1,"hello",false];
```

3. 改写现有代码时，任意值允许在编译时可选择地包含或移除类型检查

```ts
let x: any = 4;
x.ifItExists();    // 正确，ifItExists方法在运行时可能存在，但这里并不会检查
x.toFixed();      // 正确
```

### number 类型

双精度 64 位浮点值。它可以用来表示整数和分数。

```ts
let num1:number = 1;
let num2:number = 1.2;
```

### string 类型

一个字符系列，使用单引号（'）或双引号（"）来表示字符串类型。反引号（`）来定义多行文本和内嵌表达式。

```ts
let str1 = "hello";
let str2 = `str1 is ${str1}`;
```

### 数组类型

```ts
// 声明存储number类型的数组
let arr1:number[] = [1,2];

// 或者使用泛型数组
let arr2:Array<number> = [1,2]
```

### 元组

元组类型用来表示已知元素数量和类型的数组，各元素的类型不必相同，对应位置的类型需要相同。

```ts
let x:[string, number];

x = ["hello", 1];      // right

x1 = [1, "hello"];    // wrong

console.log(x[0]);
```

### enum

枚举类型用于定义数值集合。

```ts
enum Color = {Red, Blue, Pink};

let c:Color = Color.Red;

console.log(c);   // 0
```

### void

用于标识方法返回值的类型，表示该方法没有返回值。

```ts
function say():void {
  console.log("hello ts");
}
```

### null 和 undefined

null 表示一个空对象的引用，typeof null 返回 "object"。

undefined 是一个为初始化值的变量，typeof undefined 返回 "undefined"。

null 和 undefined 是其他任何类型（包括 void）的子类型，可以赋值给其它类型，如数字类型，此时，赋值后的类型会变成 null 或 undefined。

*** 而在TypeScript中启用严格的空校验（--strictNullChecks）特性，就可以使得 null 和 undefined 只能被赋值给 void 或本身对应的类型。 ***

```ts
let x:number;

x = 2;            // right

x = null;         // wrong

x = undefined;    // wrong
```

上面的例子中变量 x 只能是数字类型。如果一个类型可能出行 null 或 undefined， 可以用 | 来支持多种类型

```ts
let x:number | null | undefined;

x = 2;            // right

x = null;         // right

x = undefined;    // right
```

### never 类型

never 是其它类型（包括 null 和 undefined）的子类型，代表从不会出现的值。这意味着声明为 never 类型的变量只能被 never 类型所赋值，在函数中它通常表现为抛出异常或无法执行到终止点（例如无限循环）。

```ts
let x: never;
let y: number;

// 运行错误，数字类型不能转为 never 类型
x = 123;

// 运行正确，never 类型可以赋值给 never类型
x = (()=>{ throw new Error('exception')})();

// 运行正确，never 类型可以赋值给 数字类型
y = (()=>{ throw new Error('exception')})();

// 返回值为 never 的函数可以是抛出异常的情况
function error(message: string): never {
    throw new Error(message);
}

// 返回值为 never 的函数可以是无法被执行到的终止点的情况
function loop(): never {
    while (true) {}
}
```
