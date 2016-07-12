---
title: CSS基本属性
layout: page
category: css
date: 2016-07-04
modifiedOn: 2016-07-04
---

本章主要讲解现在广泛使用雨网站布局领域的CSS基础。

假设你已经掌握了CSS的选择器、属性和值。并且你可能已经对布局有一知半解，但是亲自写可能问题不断，那么你可以看下**[这篇教程](http://learn.shayhowe.com/html-css/)**

## display属性

display 是CSS中最重要的用于控制布局的属性。每个元素都有一个默认的 display 值，这与元素的类型有关。对于大多数元素它们的默认值通常是 block 或 inline 。一个 block 元素通常被叫做块级元素。一个 inline 元素通常被叫做行内元素。

### block

div 是一个标准的块级元素。一个块级元素会新开始一行并且尽可能撑满容器。其他常用的块级元素包括 p 、 form 和HTML5中的新元素： header 、 footer 、 section 等等。

### inline

span 是一个标准的行内元素。一个行内元素可以在段落中 &lt;span&gt 像这样 &lt;/span&gt 包裹一些文字而不会打乱段落的布局。 a 元素是最常用的行内元素，它可以被用作链接。

### none

另一个常用的display值是 none 。一些特殊元素的默认 display 值是它，例如 script 。 display:none 通常被 JavaScript 用来在不删除元素的情况下隐藏或显示元素。
它和 visibility 属性不一样。把 display 设置成 none 不会保留元素本该显示的空间，但是 visibility: hidden; 还会保留。

### 其他 display 值

还有很多的更有意思的 display 值，例如 list-item 和 table 。[这里有一份详细的列表](https://developer.mozilla.org/en-US/docs/Web/CSS/display)。之后我们会讨论下 inline-block 和 flex 。

### 自定义行级块级元素

就像我之前讨论过的，每个元素都有一个默认的 display 类型。不过你可以随时随地的重写它！虽然“人工制造”一个行内元素可能看起来很难以理解，不过你可以把有特定语义的元素改成行内元素。常见的例子是：把 li 元素修改成 inline，制作成水平菜单。

## margin: auto

```css
#main {
    width: 600px;
    margin: 0 auto; 
}
```

设置块级元素的 width 可以阻止它从左到右撑满容器。然后你就可以设置左右外边距为 auto 来使其水平居中。元素会占据你所指定的宽度，然后剩余的宽度会一分为二成为左右外边距。  

唯一的问题是，当浏览器窗口比元素的宽度还要窄时，浏览器会显示一个水平滚动条来容纳页面。这时候可以使用`max-width`

```css
#main {
    max-width: 600px;
    margin: 0 auto; 
}
```

在这种情况下使用 max-width 替代 width 可以使浏览器更好地处理小窗口的情况。这点在移动设备上显得尤为重要。  

顺便提下， [所有的主流浏览器](http://caniuse.com/#search=max-width)包括IE7+在内都支持 max-width ，所以放心大胆的用吧。


## 盒模型

在我们讨论宽度的时候，我们应该讲下与它相关的一个重点知识：盒模型。当你设置了元素的宽度，实际展现的元素却能够超出你的设置：因为元素的边框和内边距会撑开元素。看下面的例子，两个相同宽度的元素显示的实际宽度却不一样。

```css
.simple {
    width: 500px;
    margin: 20px auto;
}

.fancy {
    width: 500px;
    margin: 20px auto;
    padding: 50px;
    border-width: 10px;
}
```

以前有一个代代相传的解决方案是数学。CSS开发者需要用比他们实际想要的宽度小一点的宽度，需要减去内边距和边框的宽度。值得庆幸地是你不需要再这么做了...  

经过了一代又一代人们意识到数学不好玩，所以他们新增了一个叫做 box-sizing 的CSS属性。当你设置一个元素为 box-sizing: border-box; 时，此元素的内边距和边框不再会增加它的宽度。这里有一个与前一页相同的例子，唯一的区别是两个元素都设置了 box-sizing: border-box; ：  

```css
.simple {
    width: 500px;
    margin: 20px auto;
    -webkit-box-sizing: border-box;
       -moz-box-sizing: border-box;
            box-sizing: border-box;
}

.fancy {
    width: 500px;
    margin: 20px auto;
    padding: 50px;
    border: solid blue 10px;
    -webkit-box-sizing: border-box;
       -moz-box-sizing: border-box;
            box-sizing: border-box;
}
```

box-sizing的兼容性写法

```css
* {
    -webkit-box-sizing: border-box;
       -moz-box-sizing: border-box;
            box-sizing: border-box;
}
```

既然 box-sizing 是个很新的属性，目前你还应该像我之前在例子中那样使用 -webkit- 和 -moz- 前缀。这可以启用特定浏览器实验中的特性。同时记住它是支持[IE8+](http://caniuse.com/#search=box-sizing)。

### box-sizing的值

- border-box: 为元素设定的宽度和高度决定了元素的边框盒。就是说，为元素指定的任何内边距和边框都将在已设定的宽度和高度内进行绘制。通过从已设定的宽度和高度分别减去边框和内边距才能得到内容的宽度和高度。`(border和padding不计算入width之内)`
- content-box: 这是由 CSS2.1 规定的宽度高度行为。宽度和高度分别应用到元素的内容框。在宽度和高度之外绘制元素的内边距和边框。`(border和padding计算入width之内)`

在这里顺便介绍下`Box Model`的两种计算方法


### W3C的标准Box Model

```javascript
/*外盒尺寸计算（元素空间尺寸）*/
Element空间高度 = content height + padding + border + margin
Element 空间宽度 = content width + padding + border + margin
/*内盒尺寸计算（元素大小）*/
Element Height = content height + padding + border （Height为内容高度）
Element Width = content width + padding + border （Width为内容宽度）
```

### IE)传统下Box Model（IE6以下

```javascript
/*外盒尺寸计算（元素空间尺寸）*/
Element空间高度 = content Height + margin (Height包含了元素内容宽度，边框宽度，内距宽度)
Element空间宽度 = content Width + margin (Width包含了元素内容宽度、边框宽度、内距宽度)
/*内盒尺寸计算（元素大小）*/
Element Height = content Height(Height包含了元素内容宽度，边框宽度，内距宽度)
Element Width = content Width(Width包含了元素内容宽度、边框宽度、内距宽度)
```














