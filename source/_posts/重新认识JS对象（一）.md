---
title: 重新认识JS对象（一）-- 对象及其属性
date: 2018-12-06 13:02:20
tags:
- JS
categories:
- 前端
---

## 一、创建对象
javascript中有三种方法可以创建一个对象：
1. 对象字面量
```js
var obj = {
    name: 'jack',
    age: 12
}
```

2. new 构造函数
```js
var obj = new Object()
var obj1 = new Object({
    name: 'jack',
    age: 12
})
```

3. `Object.create()`
```js
var obj = Object.create({
    name: 'jack',
    age: 12
})
```
**需要注意的是通过`Object.create()`创建的对象实际上等于将该对象的`__proto__`指向`Object.create()`里面的参数对象，而`obj`本身是个空对象。**
```js
var obj = Object.create({
    name: 'jack',
    age: 12
})
// 等价于 obj.__proto__ = { name: 'jack', age: 12 }
console.log(obj)  // {}
console.log(obj.__proto__)  // { name: 'jack', age: 12 }
obj.toString()   // '[object Object]'
```
如果往`Object.create()`里面传入的是`null`，则创建的对象不继承`Object`的任何方法及属性。
```js
var obj = Object.create(null)
console.log(obj)  // {}
console.log(obj.__proto__)  // undefined
obj.toString()  // 报错
```
如果想创建一个空对象，需要传入`Object.prototype`
```js
var obj = Object.create(Object.prototype)
// 和 {} 、new Object()一样
```
## 二、对象的属性
我们知道，对象的属性是由名字、值和一组特性组成（属性的特性待会介绍）。在ES5中属性值可以用一个或两个方法代替，这两个方法就是`getter`和`setter`。由`getter`和`setter`定义的属性称为“存储器属性”，它不同于数据类型的属性，数据属性只有一个简单的值。我们重点讲解存储器属性。

当我们查询存储器属性时会调用`getter`方法（无参数）。这个方法返回值就是属性存取表达式返回的值。

当我们设置存储器属性时会调用`setter`方法（有参数）。这个方法修改存储器属性的值。

```js
var obj = {
    num: 12,
    age: 13,
    get num1 () {
        return this.num
    },
    set num1 (value) {
        this.num = value
    },
    get age1 () {
        return this.age
    }
}
obj.num1   // 12
obj.num1 = 120
obj.num1  // 120

obj.age1  // 13
obj.age1 = 130
obj.age1  // 13
```
存储器属性定义为一个或者两个和属性同名的函数，这个函数定义没有使用`function`关键字而是使用`get`和`set`。

可以看出如果该属性只有`getter`方法则只能读取该属性不能设置该属性，同样如果只有`setter`方法就只能设置该属性，不能读取该属性，只有当两者都有时才能正常读取和设置属性。
## 三、对象属性的特性
每个对象的数据属性都有四个特性（也可以说是属性描述符），分别为：
1. `value`  属性的值
2. `writable`  可写性，如果为`false`该值将不能被修改
3. `enumerable`  可枚举性，如果为`false`将不能被枚举
4. `configurable`  可配置性，如果为`false`将不能被配置，即不能被`delete`操作符删除，不能更改这四个特性，一旦设为`false`则无法再设为`true`，也就是一个不可逆过程

前面我们讲过存储器属性，每隔对象的存储器属性同样也有四个特性，分别为：
1. `get`
2. `set`
3. `enumerable`
4. `configurable`

如果想要获得一个对象某个属性的这四个特性，可以调用 Object.getOwnPropertyDescriptor() 方法，该方法接受两个参数，第一个为对象，第二个为对象的属性
```js
var obj = {
    name: 'jack',
    age: 12,
    get age1 () {
        return this.age1
    },
    set age1(value) {
        this.age1 = value
    }
}
// 获取数值属性的特性
Object.getOwnPropertyDescriptor(obj,'name')
// {value: "jack", writable: true, enumerable: true, configurable: true}

// 获取存储器属性的特性
Object.getOwnPropertyDescriptor(obj,'age1')
// {enumerable: true, configurable: true, get: ƒ, set: ƒ}

// 试图获取不存在的属性，返回undefined
Object.getOwnPropertyDescriptor(obj, 'sex')  // undefined

// 试图获取原型上的属性，返回undefined 
Object.getOwnPropertyDescriptor(obj, 'toString')  // undefined
```
从上面可以看出，`Object.getOwnPropertyDescriptor()` 只能得到自有属性的描述符，要想获得继承属性的特性，我们可以把该对象的原型传进去。
```js
function Person () {
    this.name = 'sillywa'
}
Person.prototype.sex = 'boy'
Person.prototype.age = 13
var person1 = new Person()
Object.getOwnPropertyDescriptor(person1.__proto__, 'sex')
// {value: "boy", writable: true, enumerable: true, configurable: true}
```
以上可以看出，我们通过对象字面量和`new`运算符创建的对象的属性它们的`writable`,`enumerable`,`configurable`都有`true`，默认都是可写、可枚举、可配置。如果要修改属性的特性可以调用`Object.defineProperty()`。
```js
var obj = {
    name: 'sillywa'
}
// 将name属性设为不可枚举并将其值设为jack
Object.defineProperty(obj, 'name', {
    value: 'jack',
    enumerable: false
})
Object.getOwnPropertyDescriptor(obj, 'name')
// {value: "jack", writable: true, enumerable: false, configurable: true}

// 新增age属性
Object.defineProperty(obj, 'age', {
    value: 12
})
Object.getOwnPropertyDescriptor(obj, 'age')
// {value: 12, writable: false, enumerable: false, configurable: false}

// 将name变为存储器属性
Object.defineProperty(obj, 'name', {
    get: function () {
        return 0
    }
})
Object.getOwnPropertyDescriptor(obj, 'name')
// {set: undefined, enumerable: false, configurable: true, get: ƒ}

obj.age = 78
obj.age  // 12
```
需要注意的是通过`Object.defineProperty()` 创建的属性其`writable`, `enumerable`, `configurable` 都为`false`。尝试修改不写的属性不会报错，但也不会修改，只有在严格模式下才会报错。

如果需要同时修改和创建多个属性，可以使用`Object.defineProperties()`。
```js
var obj = Object.defineProperties({},{
    name: {
        value: 'sillywa',
        writable: true,
        enumerable: true,
        configurable: true
    },
    age: {
        get: function () {
            return 'hello' + this.name
        },
        set: function (value) {
            this.name = 'jack'
        },
        enumerable: true,
        configurable: true
    }
})
```
## 四、属性的设置和屏蔽
我们知道当我们书写以下代码时

`obj.foo = 'bar'`

如果`obj`存在一个名为`foo`的普通数据访问属性，这条赋值语句只会修改已有的属性值。

如果`foo`不是直接存在于obj中，`[[prototype]]`链就会被遍历，如果原型链上找不到`foo`，`foo`就直接被添加到`obj`上。

然而，如果原型链上找到了`foo`属性，情况就有些不一样了。

如果属性`foo`既出现在`obj`中也在其原型链中，那么`obj`中包含的`foo`属性就会屏蔽原型链里面的`foo`属性，这就是属性屏蔽，原理就是属性的查找规则。

下面我们看一下如果`foo`不直接存在于`obj`中，而是在其原型链中时，`obj.foo = 'bar'`会出现的三种情况：
1. 如果原型链中存在名为`foo`的普通数据访问属性并且其`writable`为`true`，那么就会直接在`obj`中添加`foo`属性，它是属性屏蔽。
2. 如果原型链中存在`foo`，但其`writabla`为`false`，那么无法修改已有属性或者在`obj`中创建屏蔽属性。如果运行在严格模式下，会抛出一个错误。否则这条赋值语句会被忽略，不会发生属性屏蔽。
3. 如果原型链上存在`foo`并且它是一个`setter`，那就一定会调用这个`setter`。`foo`不会被添加到`obj`中，也不会重新定义这个`setter`。

大多数人认为，如果向原型链中已存在的属性赋值，就一定会发生属性屏蔽，但以上三种情况只有一种是如此。

如果希望在任何情况下都屏蔽`foo`，那就不能使用`=`操作符来赋值，而是使用`Object.defineProperty()`来向`obj`中添加`foo`。

情况一：
```js
function Person() { }
Person.prototype.foo = 'foo'
var obj = new Person()
obj.foo = 'bar'
obj.foo   // 'bar'
```
情况二：
```js
function Person() {}
Object.defineProperty(Person.prototype,'foo',{
    writable: false,
    enumerable: true,
    configurable: true,
    value: 'foo'
})
var obj = new Person()
obj.foo = 'bar'
obj.foo  // 'foo'
```
情况三：
```js
function Person() {}
Person.prototype = {
    constructor: Person,
    name: 'foo',
    set foo (value) {
        this.name = value
    },
    get foo () {
        return this.name
    }
}
var obj = new Person()
obj.foo = 'bar'
obj.foo  // 'bar'
// obj中并没有foo这个属性，只是调用了setter
obj.hasOwnProperty('foo') // false
```
有些情况下会隐式产生屏蔽，一定要注意，思考一下代码：
```js
var obj = {
    a: 2
}
var myObj = Object.create(obj)
obj.a  // 2
myObj.a  // 2

obj.hasOwnProperty('a')  // true
myObj.hasOwnProperty('a')  // false

myObj.a ++  // 隐式屏蔽

obj.a  // 2
myObj.a  // 3

myObj.hasOwnProperty('a')  // true
```
尽管`myObj.a ++ `看起来是查找并增加`obj.a`的属性，但是别忘了`++`操作符相当于`myObj.a = myObj.a + 1`；因此`++`操作首先会通过原型链查找到`obj.a`，并读取其值为2，然后加1赋值给`myObj.a`。

