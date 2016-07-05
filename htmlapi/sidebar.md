---
title: 侧边栏布局
category: htmlapi
layout: page
date: 2016-06-27
modifiedOn: 2016-06-27
---

## 布局一

**HTML代码**

```html
<div id="left">left sidebar</div>
<div id="content">main content</div>
```

**CSS代码**

```css
* {
    margin: 0;
    padding: 0;
}
#left{
    float: left;
    width: 220px;
    background: green;
}
#content {
    background: orange;
    margin-left: 220px;
}
```

## 布局二

**HTML代码**

```html
<div id="left">Left Sidebar</div>
<div id="content">
    <div id="content-inner">Main Content</div>
</div>
```

**CSS代码**

```css
* {
    margin: 0;
    padding: 0;
}
#left {
    float: left;
    width: 220px;
    background: green;
    margin-right: -100%;
}
#content {
    float: left;
    width: 100%;
}
#content-inner {
    margin-left: 220px;
    background: orange;
}
```


## 最小高度布局一

**HTML代码**

```html
<div id="container">
    <div id="wrapper">
        <div id="sidebar">
            <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
            </p>
            <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
            </p>
            <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
            </p>
        </div>
        <div id="main">
            <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
            <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
            <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
        </div>
    </div>
</div>
```

**CSS代码**

```css
* {
    margin: 0;
    padding: 0;
}

html {
    height: auto;
}

#container {
    background: #ccc;
}

#wrapper {
    display: inline-block;
    border-left: 200px solid #d4c376;
    position: relative;
    vertical-align: bottom;
}

#sidebar {
    float: left;
    width: 200px;
    margin-left: -200px;
    position: relative;

    min-height: 200px;
    height: auto !important;
    height: 200px;
}

#main {
    float: left;
}
```

## 最小高度布局二

**HTML代码**

```html
<div id="container">
    <div id="content">
        <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
        </p>
        <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
        </p>
        <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
        </p>
    </div>
    <div id="sidebar">
        <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
        </p>
        <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
        </p>
        <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
        </p>
    </div>
</div>
```

**CSS代码**

```css
* {
    margin: 0;
    padding: 0;
}

#container {
    background: #ccc;
    padding-left: 220px;
    overflow: hidden;
}
#content {
    width: 100%;
    border-left: 220px solid #333;
    margin-left: -220px;
    float: right;
    background: #ccc;
}
#sidebar {
    background: #333;
    width: 220px;
    float: right;
    margin-left: -220px;
}
#content,
#sidebar {
    min-height: 200px;
    height: auto !important;
    height: 200px;
}
```


## 整体布局

**HTML代码**

```html
<div id="wrapper">
    <div id="header" class="clearfix">Hearder</div>
    <div id="body">
        <div id="sidebar">
            <div class="inner">Sidebar</div>
        </div>
        <div id="main">
            <div class="inner">Main Content</div>
        </div>
    </div>
    <div id="footer">Footer</div>
</div>
```

**CSS代码**

```css
* {
    margin: 0;
    padding: 0;
}
.clearfix {
    overflow: auto;
    zoom: 1;
}
.clearfix:before,
.clearfix:after{
    content: "";
    display: block;
    height: 0;
    visibility: hidden;
    clear: both;
}

#header {
    height: 90px;
    width: 980px;
    margin: 0 auto;
    background: #ccc;
}
#body {
    width: 980px;
    height: 530px;
    margin: 0 auto;
}
#sidebar {
    float: left;
    width: 220px;
    height: 530px;
    background-color: #333;
}
#main {
    float: left;
    width: 760px;
    height: 530px;
    background: #999;
}

#footer {
    width: 980px;
    margin: 0 auto;
    background: #ddd;
}
```

