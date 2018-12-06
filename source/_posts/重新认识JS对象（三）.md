---
title: 重新认识JS对象（三）-- 原型及原型链
date: 2018-12-06 13:07:40
tags:
- JS
categories:
- 前端
---
## 一、原型检测
javascript中提供`Object.getPrototypeOf()`方法来获得对象的直接原型。
```js
function Person() {
    this.name = 'sillywa'
}
var person1 = new Person()
Object.getPrototypeOf(person1)  // {constructor: ƒ Person()}
Object.getPrototypeOf(person1.__proto__)  // Object.prototype

var person = {
    name: 'sillywa'
}
var person2 = Object.create(person)
Object.getPrototypeOf(person2)  // {name: "sillywa"}
```
javascript有以下几种方法检测一个对象的原型：
1. `isPrototypeOf()`：检测一个对象是否是另一个对象的原型
2. `obj.constructor.prototype`：检测非`Object.create()`创建的对象的原型
```js
var obj1 = {
    name: 'sillywa'
}
var obj2 = Object.create(obj1)

// isPrototypeOf()方法
Object.prototype.isPrototypeOf(obj1)  // true
obj1.isPrototypeOf(obj2)  // true
Object.prototype.isPrototypeOf(obj2)  // true

// obj.constructor.prototype
obj1.constructor.prototype === Object.prototype  // true
// obj1是obj2的原型，以下等式应为true
obj2.constructor.prototype === obj1  // false
// 而实际上
obj2.constructor.prototype === Object.prototype  // true
```
以上代码中`obj1`是`obj2`的原型，`obj2.constructor.prototype === obj1`应为`true`但是实际上却是`false`，因为`obj2`的`__proto__`里面并没有一个`constructor`属性，`obj2.constructor`实际上是`obj1`的`__proto__`里面的`constructor`，所以`obj2.constructor.prototype === Object.prototype`。
![](https://user-gold-cdn.xitu.io/2018/2/2/161555a08b89f7c8?w=501&h=377&f=png&s=40282)
## 二、`constructor`、`__proto__`与`prototype`之间的关系
在javascript中我们每创建一个对象，该对象都会获得一个`__proto__`属性（该属性是个对象），该属性指向创建该对象的`构造函数的原型`即`prototype`，同时`__proto__`对象有一个`constructor`属性指向该构造函数。这里我们需要注意的是只有函数才有`prototype`，每个对象（函数也是对象）都有`__proto__`，`Object`本身是个构造函数。举例来说：
```js
var obj = new Object()
// 也可以使用对象字面量创建，但使用Object.create()情况会不一样
// Object本身是个构造函数
Object instanceof Function  // true
obj.__proto__ === Object.prototype  // true
obj.__proto__.constructor === Object  // true
// 我们一般习惯这样写
obj.constructor === Object  // true
```
当我们访问`obj.constructor`的时候，`obj`本身是没有`constructor`属性的，但属性访问会沿着`__proto__`向上查找，即在`obj.__proto__`里面寻找`constructor`属性，如果找到了就返回值，如果未找到则继续向上查找直到`obj.__proto__.__proto__...(__proto__) === null `为止，没有找到则返回undefined。这样由`__proto__`构成的一条查找属性的线称为‘原型链’。
## 三、进一步探讨
我们知道JS是单继承的，`Object.prototype`是原型链的顶端，所有对象从它继承了包括`toString`等等方法和属性。

前面我们说到`Object`本身是构造函数，那么它继承了`Function.prototype`;`Function`也是对象，继承了`Object.prototype`。这里就有一个鸡和蛋的问题：
```js
Object instanceof Function  // true
Function instanceof Object  // true
```
以下是ES规范的解释：
>`Function`本身就是函数，`Function.__proto__`是标准的内置对象`Function.prototype`。
>`Function.prototype.__proto__`是标准的内置对象`Object.prototype`。
```js
function Person(name) {
    this.name = name
}
var person1 = new Person('sillywa')
```
![](https://user-gold-cdn.xitu.io/2018/2/2/161558d58482d19c?w=826&h=253&f=png&s=20926)

总的来说：先有`Object.prototype`（原型链顶端），`Function.prototype`继承`Object.prototype`而产生，最后，`Function`和`Object`和其它构造函数继承`Function.prototype`而产生。
## 四、`Object.create()`
我们知道通过`Object.create()`创建的对象实际上等于将该对象的`__proto__`指向`Object.create()`里面的参数对象，那么当涉及到原型时它是怎么工作的呢？
```js
var a = {
    name: 'sillywa'
}
var b = Object.create(a)

b.__proto__ === Object.prototype  // false
b.__proto__ === a  // true
b.__proto__.constructor === Object  // true
b.__proto__.hasOwnProperty('constructor')  // false
```
下面我们来具体看一看当`var b = Object.create(a)`到底发生了什么，以下实在浏览器中的结果：
![](https://user-gold-cdn.xitu.io/2018/2/2/161555a08b89f7c8?w=501&h=377&f=png&s=40282)
我们可以看到当`var b = Object.create(a)`实际上是把`b`的`__proto__`指向了`a`。当访问`b.constructor`时，实际上访问的是`b.__proto__.__proto__.constructor`。
## 五、实例与总结
```js
function Person(name) {
    this.name = name
}
var person1 = new Person('sillywa')

person1.__proto__ === Person.prototype
person1.__proto__.__proto__ === Person.prototype.__proto__
person1.__proto__.__proto__ === Object.prototype
Person.prototype.__proto__ === Object.prototype 
person1.__proto__.__proto__.__proto__ === null

Person.__proto__ === Function.prototype
```
以上均返回`true`，前五个等式和第一部分内容相关，最后一个等式为第二部分内容，**需要注意的是IE浏览器里面并没有实现`__proto__`，为了便于理解我们可以这样解释，但是最好不要在实际中使用**

![](https://user-gold-cdn.xitu.io/2018/2/2/161557f207fff784?w=823&h=242&f=png&s=21059)

