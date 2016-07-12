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

**block**

div 是一个标准的块级元素。一个块级元素会新开始一行并且尽可能撑满容器。其他常用的块级元素包括 p 、 form 和HTML5中的新元素： header 、 footer 、 section 等等。

**inline**

span 是一个标准的行内元素。一个行内元素可以在段落中 &lt;span&gt 像这样 &lt;/span&gt 包裹一些文字而不会打乱段落的布局。 a 元素是最常用的行内元素，它可以被用作链接。

**none**

另一个常用的display值是 none 。一些特殊元素的默认 display 值是它，例如 script 。 display:none 通常被 JavaScript 用来在不删除元素的情况下隐藏或显示元素。
它和 visibility 属性不一样。把 display 设置成 none 不会保留元素本该显示的空间，但是 visibility: hidden; 还会保留。

**其他 display 值**

还有很多的更有意思的 display 值，例如 list-item 和 table 。[这里有一份详细的列表](https://developer.mozilla.org/en-US/docs/Web/CSS/display)。之后我们会讨论下 inline-block 和 flex 。

**自定义行级块级元素**

就像我之前讨论过的，每个元素都有一个默认的 display 类型。不过你可以随时随地的重写它！虽然“人工制造”一个行内元素可能看起来很难以理解，不过你可以把有特定语义的元素改成行内元素。常见的例子是：把 li 元素修改成 inline，制作成水平菜单。










