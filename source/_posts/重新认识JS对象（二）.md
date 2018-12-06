---
title: 重新认识JS对象（二）-- 对象及其属性
date: 2018-12-06 13:05:21
tags:
- JS
categories:
- 前端
---
前面介绍了如何创建对象，对象的存储器属性以及对象的特性（属性描述符），今天我们接着前面的来介绍对象及其属性。
## 一、删除属性
`delete` 运算符可以用来删除对象的属性，它的操作数应当是个属性访问表达式（如果是个非法的操作数，早严格模式下会报错）。但它只能删除对象的自有属性，不能删除在原型对象上的属性，如果需要删除原型对象上的属性，需要在原型里面进行操作。

当`delete` 删除属性成功或者没有任何副作用时，返回`true`。
```js
function Person () {
    this.name = 'sillywa'
    this.age = 12
}
Person.prototype.sex = 'boy'
var person1 = new Person()

delete person1.name  // 删除name属性，返回true
person1.name  // undefined

// 试图删除原型上的属性
delete person1.sex  // 无法删除，没有任何副作用，返回true
person1.sex  // 'boy'

// 删除sex属性
delete Person.prototype.sex
person1.sex  // undefined

Person.prototype.sex = 'boy'
// 或者可以这样，和上面的效果一样
delete person1.__proto__.sex
```
`delete` 不能删除那些`configurable`为`false`的属性，也不能删除通过变量声明或者函数声明创建的全局对象的属性。在严格模式下删除不可配置的属性会报错，非严格模式下返回`false`。
```js
delete Object.prototype  // 不能删除

var x = 1
delete this.x  // 不能删除

function f() {}
delete this.f  // 不能删除
```
## 二、检测属性
有三种方法可以检测对象中是否含有某个属性：
1. `in 运算符`：  检测属性在自身及原型链上是否存在
2. `hasOwnProperty()`：  检测属性是否为自有属性（不包括继承属性）
3. `propertyIsEnumerable()`：  检测属性为自有属性切可枚举
```js
function Person () {
    this.name = 'sillywa'
    this.age = 12
}
Person.prototype.sex = 'boy'
var person1 = new Person()
// in 运算符
'age' in person1  // true
'sex' in person1  // true

// hasOwnProperty()
person1.hasOwnProperty('age')  // true
person1.hasOwnProperty('sex')  // false

// propertyIsEnumerable
person1.propertyIsEnumerable('age')  // true
person1.propertyIsEnumerable('sex')  // false
// 定义一个不可枚举的属性college
Object.defineProperty(person1,'college',{
    value: 'wuhan',
    writable: true,
    enumerable: false
})
person1.propertyIsEnumerable('college')  // false
```
判断一个对象中是否存在某个属性不能用`!=undefined`，因为即使这个属性不存在也会返回`undefined`。
## 三、枚举属性
枚举属性即遍历对象中所有的属性包括可枚举属性与不可枚举属性。

遍历可枚举属性有以下两种方法：
1. `for in循环`：  遍历对象中所有可枚举的属性，包括**自有属性和继承属性**。可以配合`hasOwnProperty()`方法得到自有属性。
2. `Object.keys()`：  遍历对象中所有可枚举的**自有属性**，返回由这些属性组成的数组。

遍历不可枚举的属性有一种方法：
1. `Object.getOwnPropertyNames()` 它和`Object.keys()`类似，返回对象所有自有属性的名称，**包括不可枚举的属性**
```js
function Person () {
    this.name = 'sillywa'
}
Person.prototype.sex = 'boy'
var person1 = new Person()
Object.defineProperty(person1,'college',{
    value: 'wuhan',
    writable: true,
    enumerable: false
})
// person1有三个属性，一个可枚举自有属性name，一个可枚举继承属性sex，一个不可枚举自有属性college

// 返回可枚举的自有属性和继承属性
for (var prop in person1) {
    console.log(prop)   // 'name','sex'
}
// 仅仅返回可枚举的自有属性
for (var prop in person1) {
    if (person1.hasOwnProperty(prop)) {
        console.log(prop)   // 'name'
    }
}

// 返回可枚举的自有属性组成的数组
Object.keys(person1)  // ['name']

// 返回所有自有属性组成的数组，包括不可枚举的属性
Object.getOwnPropertyNames(person1)  // ['name','college']

```

至此对象及其属性介绍完毕，下一章将讨论对象的{% post_link 重新认识JS对象（三） 原型及原型链 %}
