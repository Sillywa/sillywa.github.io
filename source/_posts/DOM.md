---
title: DOM
date: 2018-12-06 13:21:43
tags:
- JS
categories:
- 前端
---
所谓DOM就是文档对象模型，我们可以把一个文档的各种元素想象成一个节点树，每一段标记都可以通过树中的一个节点（Node）来表示：HTML元素通过元素节点表示、属性通过属性节点表示、文本通过文本节点表示。总共有12种节点类型，我们需要重点掌握元素节点，属性节点和文本节点。
### Node类型
每一个节点都具有一系列公共属性，下面我们列举一下常见的属性：

* `nodeType`: 显示节点的类型，1代表元素节点、2代表属性节点、3代表文本节点
* `nodeName`: 元素节点的标签名，以大写表示
* `tagName`: 与`nodeName`一样
* `nodeValue`: 元素节点的值，文本节点和属性节点才有值，元素节点的`nodeValue`为`null`
* `attributes`: 返回属性节点的集合
```js
<div class="wrapper" id="totop">
</div>

document.getElementById('totop').attributes["id"]  // 此处是id属性节点，并不是id的值
// 要得到id的值，只需获取该属性节点的值
document.getElementById('totop').attributes["id"].nodeValue
// 当然一般情况下我们这样获取id的值
document.getElementById('totop').id
```
* `firstChild`: 表示某个节点的第一个节点
* `lastChild`: 表示某个节点的最后一个节点
* `childNodes`: 表示某个节点的所有子节点的数组
* `parentNode`: 表示某个节点的第一个父节点
* `nextSibling`: 下一个兄弟节点
* `previousSibling`: 上一个兄弟节点

### 节点操作
1. 创建节点
 * 创建一个元素节点
 ```js
 // 创建一个div元素
 var div = document.createElement('div')
 ```
 * 创建一个文本节点
 ```
 var text = document.createTextNode('Hello Word')
 ```
2. 添加节点

当我们创建了一个个节点之后我们需要把他们组合在一起，即将一个节点添加到另一个节点中。这是我们可以用`appendChild()`方法，将该节点添加到另一个节点的`childNodes`末尾。添加节点之后，`appendChild()`返回新增的节点。
```js
 // 创建一个div元素
 var div = document.createElement('div')
 var text = document.createTextNode('Hello Word')
 div.appendChild(text)
 // 最后还需要将节点加入到body中
 document.body.appendChild(div)
 
 div.lastChild == div.appendChild(text)  // true
```
3. 插入节点

我们可以用`insertBefore()`方法来在一个节点之前插入另一个节点，调用该方法的应当是目标节点的父节点，这个方法接受两个参数：要插入的节点和目标节点。插入节点后，被插入的节点会变成目标节点的前一个兄弟节点(`previousSibling`)，同时返回插入的节点。如果第二个参数是`null`，则和`appendChild()`方法一样。

下面是该方法的语法：

```js
parentElement.insertBefore(newElement, targetElement)
```
我们不必搞清楚目标元素的父元素是谁，因为`targetElement`元素的`parentNode`就是目标元素的父元素
```js
targetElement.parentNode.insertBefore(newElement, targetElement)
```

4. 移除节点
* `replaceChild()`：该方法替换一个节点的子节点，接受两个参数：第一个为新的节点，第二个为目标节点，返回目标节点并从文档中被移除
```js
// 替换第一个子节点
someNode.replaceChild(newNode, someNode.firstChild)
```

* `removeChild()`：该方法接受一个参数，即要移除的节点，返回移除的节点
```js
// 移除第一个子节点
someNode.removeChild(someNode.firstChild)
```
### 属性操作
1. 获取属性

通常情况下我们可以使用`getAttribute()`方法来获取一个属性节点的值，该方法只有一个参数：要获取的属性的名字
```js
someNode.getAttribute(attribute)
```
2. 设置属性

当我们需要修改或设置属性时，可以使用`setAttribute()`方法，该方法接受两个参数：要设置或修改的属性，属性值
```js
someNode.setAttribute(attribute, value)
```
3. 移除属性

当我们需要移除属性时，可以使用`removeAttribute()`方法，该方法接受一个参数：要移除的属性
```js
someNode.removeAttribute(attribute)
```
### 理解元素的子节点
我们需要注意的是不同浏览器处理子节点的方式是不一样的，以下面代码为例：
```html
<div>
  <p></p>
  <p></p>
  <p></p>
</div>
```
很显然`<div>`元素有3个子节点，分别是3个`<p>`元素，但是这种说法只有在IE浏览器中才是正确的。因为在其它浏览器中`<div>`有7个子节点，增加了4个文本节点（表示`<p>`元素之间的空隙）。如果像下面这样删除空格，那么所有浏览器都会返回相同数量的子节点。
```html
<div><p></p><p></p><p></p></div>
```
因此如果要通过`childNodes`属性遍历子节点，一定要判断节点类型即`nodeType`的值，只有当`nodeType`值为1时，才能进行操作
```js
for (var i = 0, length = element.childNodes.length; i < length; i++) {
    if (element.childNodes[i].nodeType == 1) {
        // do something
    }
}
```

