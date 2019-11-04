---
title: TypeScript 面向对象(三)
date: 2019-09-16 20:34:27
top: 10
tags:
- TypeScript
categories:
- 前端
---
## 接口

### 介绍

TypeScript 的核心原则之一是对值所具有的结构进行类型检查，其被称为“鸭式辨型法”或“结构式子类型化”。

在 TypeScript 里，接口的作用就是为这些类型命名和为你的代码或第三方代码定义契约。

### 接口初探

```ts
function getName(my_object: { name: string }) {
  console.log(my_object.name);
}

let my_object = { age: 22, name: "Sillywa" };

getName(my_object);
```

以上代码我们规定 getName 函数具有一个参数，且该参数必须含有一个 string 类型的 name 属性。需要注意的是，我们传入的参数实际包含很多属性，但是编译器只会检查那些必须的属性是否存在，并且其类型需要匹配。

下面我们重写这个例子，使用接口来进行描述：必须包含一个 string 类型的 name 属性：

```ts
interface ObjectValue {
  name: string;
}

function getName(my_object: ObjectValue) {
  console.log(my_obj.name);
}

let my_object = { age: 22, name: "Sillywa" };

getName(my_object);
```

我们使用 interface 关键字来定义一个接口，以上代码中 objectValue 为接口的名字，花括号里面为接口所具有的约束。

需要注意的是，类型检查器不会去检查属性的顺序，只要相应属性存在即可。

### 可选属性

接口里的属性不全都是必须的。可选属性在应用在 “option bags" 模式时很常用，即给函数传入的参数中只有部分属性赋值了。

```ts
interface SquareConfig {
  color?: string;
  width?: number;
}

function createSquare(config: SquareConfig): { color: string; area: number } {
  let newSquare = { color: "white", area: 100};
  if (config.color) {
    newSquare.color = config.color;
  }
  if (config.width) {
    newSquare.area = config.width * config.width;
  }
  return newSquare;
}

let mySquare = createSquare({ color: "black" });
```

### 只读属性

只读属性只能在对象刚刚创建是为其赋值，无法修改其值。

```ts
interface Point {
  readonly x: number;
  readonly y: number;
}

let p1: Point = { x: 12, y: 90};

p1.x = 80;   // error

```

TypeScript 具有 ReadonlyArray<T> 类型，它与 Array<T> 类似，只是把所有可变方法去掉了，因此可以保证数组被创建后再也无法被修改：

```ts
let a: number[] = [1,2,3];
let ro: ReadonlyArray<number> = a;

ro[1] = 13;   // error
ro.push(0);  // error
ro.length = 90;  // error
a = ro;       // error

```
上面代码的最后一行，可以看到就算把整个ReadonlyArray赋值到一个普通数组也是不可以的。

`readonly` vs `const`

判断该使用 readonly 还是 const 的方法是看要把它当作变量使用还是属性使用。作为变量使用时用 const，作属性使用时用 readonly。

### 额外的属性检查

TypeScript 在检查对象字面量时会特殊对待而且会经过额外的属性检查，当他们赋值给变量或作为参数传递的时候。如果一个对象字面量存在任何”目标类型“不包含的属性时，你会得到一个错误。

```ts
interface SquareConfig {
    color?: string;
    width?: number;
}

function createSquare(config: SquareConfig){
    // ...
}

createSquare({ e_color: "red", width: 100 });   // error
```
绕开这些检查非常简单，最简单的方法是使用类型断言：

```ts

createSquare({ e_color: "red", width: 100 } as SquareConfig);
```

然而，最佳的方式是能够添加一个字符串索引签名，前提是你能够确定这个对象可能具有某些做为特殊用途使用的额外属性。 如果 SquareConfig 带有上面定义的类型的 color 和 width 属性，并且还会带有任意数量的其它属性，那么我们可以这样定义它：

```ts
interface SquareConfig {
  color?: string;
  width?: number;
  [propName: string]: any;
}
```
这里我们要表示的是 SquareConfig 可以有任意数量的属性，并且只要他们不是 color 和 width，那么就无所谓它的类型是什么。

还有最后一种跳过检查的方法，它就是将这个对象赋值给另一个变量：

```ts
let squareOptions = { colour: "red", width: 100 };
let mySquare = createSquare(squareOptions);
```

### 函数类型

接口除了描述带有属性的普通对象外，接口也可以描述函数类型。

为了使接口能描述函数类型，需要给接口定义一个调用签名。它就像是一个只有参数列表和返回值类型的函数定义。参数列表里的每个参数都需要名字和类型。

```ts
interface SearchFunc {
  (source: string, subString: string): boolean;
}

let mySearch: SearchFunc = function(source: string, subString: string) {
  let result = source.search(subString);
  return result > -1;
}

```
对于函数类型的类型检查来说，函数的参数名不需要与接口里定义的名字相匹配。

```ts
interface SearchFunc {
  (source: string, subString: string): boolean;
}

let mySearch: SearchFunc = function(src: string, sub: string) {
  let result = src.search(sub);
  return result > -1;
}

```
函数的参数会逐个进行类型检查，要求对应位置上的参数类型是兼容的。如果不指定类型， TypeScript 的类型系统会推断出参数类型。

### 类类型

#### 实现接口

```ts
interface PersonInterface {
  age: number;
  name: string;
  getAge(): number;
}

class Person implements PersonInterface {
  constructor(public name: string, public age: number) {};
  getAge(): number {
    return this.age
  }
}

```
接口只描述了类的共有部分，而不是公共和私有两部分。它不会帮忙检查类是否具有某些私有成员。

### 继承接口

和类一样，接口也可以相互继承。这样我们可以从一个接口里复制成员到另一个接口里，可以更灵活地将接口分割到可重用的模块里。

```ts
interface Shape {
  color: string;
}

interface Square extends Shape {
  sideLength: number;
}

let square = <Square>{};

square.color = "blue";
square.sideLength = 10;
```
一个接口可以继承多个接口，创建出多个接口的合成接口。

```ts
interface Shape {
    color: string;
}

interface PenStroke {
    penWidth: number;
}

interface Square extends Shape, PenStroke {
    sideLength: number;
}

let square = <Square>{};
square.color = "blue";
square.sideLength = 10;
square.penWidth = 5.0;

```
