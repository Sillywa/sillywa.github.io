---
title: 浏览器回流(Reflow)与重绘(Repaint)及其思考
date: 2018-12-06 13:30:51
tags:
- JS
categories:
- 前端
---
我们知道浏览器在解析文档时，会经历以下步骤：
1. 将`HTML`解析为`DOM`，将`CSS`解析为`CSSOM`，`DOM`和`CSSOM`合并生成`Render Tree`
2. 然后根据`Render Tree`将节点绘制在页面上

所谓回流是当元素尺寸、结构或者某些属性发生改变时，浏览器重新部分或者全部渲染文档的过程。
所谓重绘是指元素样式发生改变但并未改变其在文档流中的位置。

### 回流(Reflow)
我们理解了回流之后再看看哪些操作会导致浏览器回流：
1. 浏览器窗口大小发生改变
2. 元素尺寸或者位置发生变化
3. 元素内容变化
4. 元素字体变化
5. 添加或删除`DOM`
6. 激活`CSS`伪类
7. 查询某些属性或调用某些方法：
 * `clientWidth、clientHeight、clientTop、clientLeft`
 * `offsetWidth、offsetHeight、offsetTop、offsetLeft`
 * `scrollWidth、scrollHeight、scrollTop、scrollLeft`
 * `scrollIntoView()、scrollIntoViewIfNeeded()`
 * `getComputedStyle()`
 * `getBoundingClientRect()`
 * `scrollTo()`

### 重绘(Repaint)
当页面中元素的样式改变并不影响其在文档流中得到位置时，浏览器紧紧将新样式赋给它并重新绘制，这就是重绘。例如`color、background-color、visibility`

### 思考
既然这样那么我们如何避免重绘与回流呢？

* 避免使用`table`布局
* 将动画效果应用到`position`属性为`absolute`或`fixed`的元素上，脱离文档流的元素不会引起回流
* 可以使用`tranform`属性来设置动画
* 避免频繁操作样式，最好使用`class`来更改样式
* 避免频繁操作`DOM`，创建一个`documentFragment`，在它上面进行`DOM`操作
* 可以先将元素设为`display:none`，操作后再把它显示出来。因为在`display`为`none`的元素上操作不会引起重绘与回流
