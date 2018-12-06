---
title: 基于 Object.defineProperty() 的原生js双向数据绑定
date: 2018-12-06 13:18:38
tags:
- JS
categories:
- 前端
---
前面我们介绍过存储器属性({% post_link 重新认识JS对象（一） 重新认识JS对象（一）-- 对象及其属性 %})，以及如何用`Object.defineProperty()`定义一个存储器属性，今天我们介绍如何用`Object.defineProperty()`实现双向数据绑定。

我们知道一个存储器属性有四个属性描述符：`get，set，configurable，enumerable`。我们来复习一下如何创建一个存储器属性：
```js
var user = {
    name: ''
}
Object.defineProperty(user, 'nickname', {
    configurable: true,
    enumerable: true,
    get: function() {
        return this.name
    },
    set: function(value) {
        this.name = value
    }
})
```
以上代码我们给`user`创建了一个名为`nickname`的存储器属性。

接下来我们改写`get`和`set`，让它们与`DOM`绑定，并实现双向数据绑定，以下为具体实现的伪代码：
```js
<input type="text" id="foo">

<script>
    var user = {}
    Object.defineProperty(user, 'inputValue', {
        configurable: true,
        get: function() {
            return document.getElementById('foo').value
        },
        set: function(value) {
            document.getElementById('foo').value = value
        }
    })
</script>
```
我们打开控制台，改变`user.inputValue`的值，会发现`input`输入框里的值也发生变化；同样我们在`input`输入框里面输入值，在控制台打印`user.inputValue`，会发现`user.inputValue`也发生了变化。这样我们就实现了双向的数据绑定。

如果多个`DOM`绑定同一个数据，我们可以监听`input`输入框的`keyup`事件，只要触发了`keyup`事件我们就把`user.inputValue`的值赋给另一个`DOM`，具体实现如下：
```js
<input type="text" id="foo">
<p id="test"></p>

<script>
    var user = {}
    Object.defineProperty(user, 'inputValue', {
        configurable: true,
        get: function() {
            return document.getElementById('foo').value
        },
        set: function(value) {
            document.getElementById('foo').value = value
            document.getElementById('test').innerHTML = value
        }
    })
    document.getElementById('foo').addEventListener('keyup',function() {
        document.getElementById('test').innerHTML = user.inputValue
    })
```
最后附上源码图片
![](https://user-gold-cdn.xitu.io/2018/3/2/161e673d16d3544a?w=809&h=889&f=png&s=92772)

思考：其实实现双向数据绑定并不一定要用`Object.defineProperty()`，其主要是运用存储器属性的`get和set`，以下代码也可以实现双向数据绑定：
```js
<input type="text" id="foo">
<p id="test"></p>

<script>
    var user = {
      get inputValue() { 
        return document.getElementById('foo').value
      },
      set inputValue(value) {
        document.getElementById('foo').value = value
        document.getElementById('test').innerHTML = value
      }
    }
    document.getElementById('foo').addEventListener('keyup',function() {
      document.getElementById('test').innerHTML = user.inputValue
    })
</script>
```
