---
title: 元素垂直居中
category: htmlapi
layout: page
date: 2016-06-27
modifiedOn: 2016-06-27
---

## 利用table-cell方式

**HTML代码**

```html
<div id="wrapper">
    <div id="cell">
        content 
    </div>
</div>		
```

**CSS代码**

```css
#wrapper {
    width: 200px;
    height: 200px;
    display: table;
    background: #ccc;
}
#cell {
    display: table-cell;
    vertical-align: middle;
    text-align: center;
    background: #333;
}
```

## 利用绝对定位方式

**HTML代码**

```html
<div id="wrapper">
    <div id="content">
        content
    </div>
</div>
```

**CSS代码**

```css
body {
    margin: 0;
    padding: 0;
}
#wrapper {
    position: relative;
    background: #ccc;
    width: 500px;
    height: 500px;
}
#content {
    width: 50%;
    height: 200px;
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    margin: auto;
    text-align: center;
    line-height: 200px;
    background: #333;
}
```

## 利用CSS3的transform方式

**HTML代码**

```html
<div id="wrapper">
    <div id="content">
        居中居中居中居中居中居中
    </div>
</div>
```

```css
#wrapper {
    width: 300px;
    height: 300px;
    position: relative;
    background: #ccc;
}
#content {
    position: absolute;
    left: 50%;
    top: 50%;
    -webkit-transform: translate(-50%, -50%);
        -ms-transform: translate(-50%, -50%);
         -o-transform: translate(-50%, -50%);
            transform: translate(-50%, -50%);
    background: #333;
}
```

**CSS代码**

## 利用font-size方式

**HTML代码**

```html
<div id="wrapper">
    a
</div>
```

```css
#wrapper {
    width: 400px;
    height: 400px;
    margin: 0 auto;
    text-align: center;
    font-size: 349.2px;
    background: #ccc;
}
```

**CSS代码**

## 利用弹性布局flex方式

**HTML代码**

```html
<div id="wrapper">
    <div id="content">
    </div>
</div>
```
**CSS代码**

```css
body {
    margin: 0;
    padding: 0;
}
#wrapper {
    display: flex;
    -webkit-align-items: center;
    align-items: center;
    justify-content: center;
    height: 400px;
    width: 400px;
    background: #ccc;
}
#content {
    width: 100px;
    height: 100px;
    background: #333;
}
```










