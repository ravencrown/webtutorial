---
title: 移动端触摸事件
category: mobileweb
layout: page
date: 2016-06-28
modifiedOn: 2016-06-28
---

智能手机和平板电脑的移动设备通常会有一个触摸屏，用于捕捉用户的手指所做的交互操作。移动网络在不断的发展之中，能够支持越来越复杂的应用，移动端开发者需要找到一种方法来处理这些事件，最近，一个 W3C 工作组正在合力制定这一触摸事件规范

## 触摸事件

**四种在规范中列出并跨移动设备广泛实现的基本触摸事件：**  

- touchstart：手指放在一个 DOM 元素上。
- touchmove：手指拖动一个 DOM 元素。
- touchend：手指从一个 DOM 元素上移开。
- touchcancel：系统取消touch事件的时候触发。（例如你在浏览器里玩HTML5的游戏，这个时候你女朋友打电话进来了，就会触发touchcancel事件，游戏的作者就可以在这里做暂停游戏、自动存档等操作）

**每个触摸事件都包括了三个触摸列表：**  

- touches：当前位于屏幕上的所有手指动作的列表。
- targetTouches：位于当前 DOM 元素上的手指动作的列表。
- changedTouches：涉及当前事件的手指动作的列表。例如，在一个 touchend 事件中，这将是移开手指。

**这些列表由包含触摸信息的对象组成：**  

- identifier : 一个数值，touch对象的unique ID ，唯一标识触摸会话（touch session）中的当前手指
- target : DOM元素，是动作所针对的目标
- clientX / clientY : 触摸点相对于浏览器窗口viewport的位置，不包含滚动距离
- pageX / pageY : 触摸点相对于页面的位置，包含滚动距离
- screenX / screenY : 触摸点相对于屏幕的位置
- radiusX/radiusY/rotationAngle : 画出大约相当于手指形状的椭圆形，分别为椭圆形的两个半径和旋转角度。(目前还没有得到的广泛的支持)


## 可触控的应用程序

touchstart、touchmove 和 touchend 事件提供了一组足够丰富的功能，可支持几乎任何类型的基于触摸的交互，其中包括所有常见的多点触控手势（如开合缩放、旋转等）。  

下面这段代码可让您使用单指触摸随意拖动一个 DOM 元素：  

```javascript
var obj = document.getElementById('id');
obj.addEventListener('touchmove', function(event) {
    // If there's exactly one finger inside this element
    if (event.targetTouches.length == 1) {
        var touch = event.targetTouches[0];
        // Place element where the finger is
        obj.style.left = touch.pageX + 'px';
        obj.style.top = touch.pageY + 'px';
    }
}, false);
```

下面的示例显示了屏幕上当前所有的触点，其作用就是用来感受一下设备的响应性。

```javascript
// Setup canvas and expose context via ctx variable
canvas.addEventListener('touchmove', function(event) {
    for (var i = 0; i < event.touches.length; i++) {
        var touch = event.touches[i];
        ctx.beginPath();
        ctx.arc(touch.pageX, touch.pageY, 20, 0, 2*Math.PI, true);
        ctx.fill();
        ctx.stroke();
    }
}, false);
```

## 触摸事件获取坐标

**移动端获取点击坐标的方式说明**  

- 移动端和PC端在处理事件上有些不同之处，首先事件上不同，移动端这边是touchstart、touchmove、touchend这3个事件。
- 移动端由于是手指操作而非鼠标，所以存在多点触控，即多根手指在屏幕上触发事件。不能通过e.clientX来获取单个点坐标。
- 移动端 事件event中存在一个触控集合touches数组，通过取数组的第一个元素来获取坐标位置，即第一个触碰屏幕手指的坐标(e.touches[0].pageX , e.touches[0].pageY)。
- 有时需要获取全部触碰点的位置，那就要循环数组了，逐个处理。
- 有时要防止多点触碰，以及手指对屏幕进行缩放的影响，可以加入以上判断if(e.touches.length > 1 \|\| e.scale && e.scale != 1)。
- touchend事件，代表手指离开屏幕，此时不存在触控，所以e.touches这个数组的长度为0，也就不能在touchend的处理函数中获取pageX属性了。

## 禁止缩放

多点触控的默认设置不是很好用，因为您的滑动和手势往往与浏览器的行为有关联，例如滚动和缩放。  

要停用缩放，应使用下面的元标记设置视口，这样用户就无法扩展视口了：  

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
```

## 禁止滚动

某些移动设备对于 touchmove 有默认的行为，例如经典的 iOS overscroll 特效，会在滚动超出内容的界限时引发视图反弹。这种做法在很多的多点触控应用程序中会带来混乱，不过很容易就能停用：

```javascript
document.body.addEventListener('touchmove', function(event) {
   event.preventDefault();
}, false); 
```

## 仔细渲染

如果您要编写的多点触控应用程序涉及到复杂的多指手势，应仔细考虑如何响应触摸事件，因为需要一次处理很多的事件。请考虑前一章节中在屏幕上绘制所有触点的例子，您可以在有触摸输入的时候就立刻进行绘制：

```javascript
canvas.addEventListener('touchmove', function(event) {
    renderTouches(event.touches);
}, false);
```
不过这一技术并不是要随着屏幕上的手指数增多而扩展。您可以改为跟踪所有手指，然后在一个循环中进行渲染，这样获得的性能要好得多：

```javascript
var touches = []
canvas.addEventListener('touchmove', function(event) {
    touches = event.touches;
}, false);

// Setup a 60fps timer
timer = setInterval(function() {
    renderTouches(touches);
}, 15);
```

setInterval 不太适于动画，因为它没有考虑到浏览器自身的渲染循环。现代的桌面浏览器提供了 requestAnimationFrame，从性能和电池寿命方面考虑，这是一个更好的选择。因此只要移动浏览器支持这一方式，就应优先选择  

要记住的一点是，event.touches 是与屏幕接触的所有手指的数组，而不仅仅是位于目标 DOM 元素上的那些。您可能会发现，改用 event.targetTouches 或 event.changedTouches 要更实用一些。


## 参考链接

- 触摸事件规范, [触摸事件规范](https://dvcs.w3.org/hg/webevents/raw-file/tip/touchevents.html)