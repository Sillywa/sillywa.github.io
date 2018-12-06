---
title: JS事件
date: 2018-12-06 13:27:14
tags:
- JS
categories:
- 前端
---
事件用来处理js与HTML之间的交互，我们可以使用事件处理程序来监听事件，以便在事件发生时执行相应代码。
### 一、事件流
* 事件冒泡：IE事件流叫做事件冒泡，即事件由最具体的元素向外传播，是由内向外的
* 事件捕获：事件捕获即事件由不具体的元素到具体元素，是由外向内传播
* DOM事件流：DOM2级事件规定的事件流包括三个阶段：事件捕获阶段、处于目标阶段和事件冒泡阶段

### 二、事件处理程序
* #### HTML事件处理程序

直接在html代码中绑定事件
```js
<div onclick="doSomething()"></div>
```
我们一般不采用这种方法绑定事件，因为html代码与js代码耦合度太高，不便于维护。但是现在的vue框架使用的却是这种事件处理程序。
* #### DOM0级事件处理程序

这种方法就是将一个函数赋值给一个事件处理程序属性。
```js
var btn = document.getElementById('myBtn')
btn.onclick = function() {
    // do something
}
```
**其中我们需要注意的是DOM0级事件处理程序被认为是元素的方法，因此事件处理程序是在元素的作用域中运行的，this指向当前元素。**
```js
var btn = document.getElementById('myBtn')
btn.onclick = function() {
    // 输出元素的id属性
    console.log(this.id)
}
```
* #### DOM2级事件处理程序

DOM2级事件处理程序定义了两个方法，用于处理指定和删除事件处理程序的操作：`addEventListener()`和`removeEventListener()`。所有DOM节点都包含这两个方法，并且他们都接受三个参数：要处理的事件名、作为事件处理程序的函数和一个布尔值。最后一个布尔值如果为true表示在捕获阶段调用事件处理程序；如果为false表示在冒泡阶段调用事件处理程序。
```js
var btn = document.getElementById('myBtn')
btn.addEventListener('click', function() {
    // do something
}, false)
```
使用DOM2级事件处理程序的好处是可以为一个节点添加多个事件处理程序。
```js
var btn = document.getElementById('myBtn')
btn.addEventListener('click', function() {
    // do something
    alert(0)
}, false)
btn.addEventListener('click', function() {
    // do something
    alert(1)
}, false)
```
这两个事件处理程序会按照顺序依次执行，所以首先弹出0，再弹出1。

通过`addEventListener()`添加的事件处理程序只能使用`removeEventListener()`来移除；移除时使用的参数应当与添加时相同。这也就是说直接添加的匿名函数处理程序是无法移除的，举例来说：
```js
var btn = document.getElementById('myBtn')
btn.addEventListener('click', function() {
    // do something
    alert(0)
}, false)
btn.removeEventListener('click', function() {  // 没有用
    // do something
    alert(0)
}, false)
```
在这个例子中我们虽然调用`removeEventListener()`使用的看似相同的参数，其实两个匿名函数根本不同，所以我们的给函数起个名字：
```js
var btn = document.getElementById('myBtn')
var handler = function() {
    // do something
    alert(0)
}
btn.addEventListener('click', handler, false)
btn.removeEventListener('click', handler, false)  // 有效
```
大多数情况下我们都应该在事件冒泡阶段调用事件处理程序以兼容各大浏览器。最好在只需要在事件到达目标前截获它的时候将事件处理程序添加到事件捕获阶段。

**需要注意的是DOM2级事件处理程序和DOM0级事件处理程序一样，事件处理程序中的this也是指向当前元素。**
* ####  IE事件处理程序

IE实现了与DOM2级处理程序中类似的两个方法：`attachEvent()和detachEvent()`，这两个方法接受相同的两个参数：事件名称和事件处理程序函数。事件处理程序默认被添加到事件冒泡阶段。
```js
var btn = document.getElementById('myBtn')
btn.attachEvent('onclick', function() {
    // do something
})
```
**需要注意的是`attachEvent()`第一个参数为`'onclick'`而非`addEventListener()`中的`'click'`**

**在IE中使用`attachEvent()`与DOM0级DOM2级方法的主要区别在于事件处理程序函数的作用域不一样。在使用DOM0级DOM2级方法时，事件处理函数的作用域为当前元素所在作用域，this指向当前元素，而在使用`attachEvent()`方法时，事件处理程序函数的作用域为全局作用域，this指向window。**

同样我们亦可以使用`attachEvent()`方法为一个元素添加多个事件。
```js
var btn = document.getElementById('myBtn')
btn.attachEvent('onclick', function() {
    // do something
    alert(0)
})
btn.attachEvent('onclick', function() {
    // do something
    alert(1)
})
```
**不过需要注意的是，与DOM事件处理不同，这些事件处理程序并不是按照他们添加的顺序依次执行，而是以相反的顺序执行**。所以先弹出1，再弹出0。

与DOM事件处理程序一样，通过`attachEvent()`添加的事件也只能使用`detachEvent()`方法移除，参数必须一模一样。
```js
var btn = document.getElementById('myBtn')
var handler = function() {
    // do something
    alert(0)
}
btn.attachEvent('onclick', handler)
btn.detachEvent('onclick', handler)
```

在所有事件处理程序中HTML事件处理程序和DOM0级事件处理程序兼容所有浏览器，DOM2级事件处理程序兼容IE9+、Firefox、Chrome和OPera，IE事件处理程序兼容IE8及以下，不过现在大部分公司都不会兼容IE8及以下，所以广泛使用的还是DOM0级和DOM2级事件处理程序。
### 三、事件对象
在触发DOM上的某个事件时，会产生一个事件对象event，这个对象中包含所有与事件对象有关的信息。包括导致事件的元素、事件的类型等等。所有浏览器都支持event，但支持方式不同。
* #### DOM中的事件对象
兼容DOM的浏览器都会将一个event对象传入到事件处理程序中。无论是DOM0级还是DOM2级事件处理程序。
```js
var btn = document.getElementById('myBtn')
btn.onclick = function(event) {
    console.log(event)
}
btn.addEventListener('click', function(event) {
    console.log(event)
}, false)
```
在通过HTML特性指定的事件处理程序时，同样存在event对象。
```js
<div onclick="console.log(event)"></div>
```
event对象包含一下常用属性与方法：

* `type`：事件类型如click
* `target`：事件实际发生的目标
* `preventDefault()`：取消事件的默认行为
* `stopPropagation()`：取消事件捕获或冒泡

需要注意的是只有在事件处理程序执行期间，event对象才会存在；一旦事件处理程序执行完毕，event对象就会被销毁。
* #### IE中的事件对象
与访问DOM中的event对象不同，访问IE中的event对象取决于事件处理程序。

以下是DOM0级事件处理程序时的event：
```js
var btn = document.getElementById('myBtn')
btn.onclick = function() {
    var event = window.event
    console.log(event)
}
```
event对象被看作为window的一个属性。

但是如果事件处理程序使用`attachEvent()`添加的，那么就会有一个event对象作为参数被传入事件处理函数中。
```js
var btn = document.getElementById('myBtn')
btn.attachEvent('onclick', function(event) {
    console.log(event)
})
```
如果直接使用HTML事件处理程序，也可以通过event变量访问到event对象。
```js
<div onclick="console.log(event)"></div>
```
IE中的event对象同时也有一下几个常见的属性及方法：
* `type`
* `srcElement`：事件目标，相当于target
* `returnValue`：默认为true，设置为false可以取消事件的默认行为，相当于preventDefault()
* `cancelBubble`：默认为false，将其设置为true可以取消事件冒泡，相当于stopPropagation()

### 四、事件委托
当事件处理程序过多时，为了避免我们一个个添加事件处理程序，我们可以考虑使用事件委托。考虑以下代码：
```js
<ul id="list">
  <li id="sayOne"></li>
  <li id="sayTwo"></li>
  <li id="sayThree"></li>
</ul>
```
现在我们要点击li元素然后执行不同的函数，一般做法是给每一个li元素都添加一个事件处理程序，但是这种做法或导致页面性能下降，因此我们利用事件冒泡，只指定一个事件处理程序，用它来管理所有同类事件，具体实现方法如下：
```js
var list = document.getElementById("list")
list.addEventListener('click', function(event) {
    // target为当前点击的元素
    var target = event.target
    switch(target.id) {
        case 'sayOne':
            alert(1)
            break;
        case 'sayTwo':
            alert(2)
            break;
        case 'sayThree':
            alert(3)
            break;
    }
}, false)
```
