---
title: Sass入门
layout: page
category: css
date: 2016-07-15
modifiedOn: 2016-07-15
---

## Sass入门

Sass 是对 CSS 的扩展，让 CSS 语言更强大、优雅。 它允许你使用变量、嵌套规则、 mixins、导入等众多功能， 并且完全兼容 CSS 语法。 Sass 有助于保持大型样式表结构良好， 同时也让你能够快速开始小型项目， 特别是在搭配 Compass 样式库一同使用时。

**特点**

- 完全兼容 CSS3
- 在 CSS 语言基础上添加了扩展功能，比如变量、嵌套 (nesting)、混合 (mixin)
- 对颜色和其它值进行操作的{Sass::Script::Functions 函数}
- 函数库控制指令之类的高级功能
- 良好的格式，可对输出格式进行定制

SASS提供四个编译风格的选项：

- nested：嵌套缩进的css代码，它是默认值。
- expanded：没有缩进的、扩展的css代码。
- compact：简洁格式的css代码。
- compressed：压缩后的css代码。


## 语法

Sass 有两种语法。 第一种被称为 SCSS (Sassy CSS)，是一个 CSS3 语法的扩充版本，这份参考资料使用的就是此语法。 也就是说，所有符合 CSS3 语法的样式表也都是具有相同语法意义的 SCSS 文件。 另外，SCSS 理解大多数 CSS hacks 以及浏览器专属语法，例如[IE 古老的 filter 语法](https://msdn.microsoft.com/en-us/library/ms533754(VS.85).aspx)。 这种语种语法的样式表文件需要以 .scss 扩展名。

第二种比较老的语法成为缩排语法（或者就称为 "Sass"）， 提供了一种更简洁的 CSS 书写方式。 它不使用花括号，而是通过缩排的方式来表达选择符的嵌套层级，I 而且也不使用分号，而是用换行符来分隔属性。 很多人认为这种格式比 SCSS 更容易阅读，书写也更快速。 缩排语法具有 Sass 的所有特色功能， 虽然有些语法上稍有差异； 这些差异在{file:INDENTED_SYNTAX.md 所排语法参考手册}中都有描述。 使用此种语法的样式表文件需要以 .sass 作为扩展名。  

任一语法都可以导入另一种语法撰写的文件中。 只要使用 sass-convert 命令行工具，就可以将一种语法转换为另一种语法：

```shell
# 将 Sass 转换为 SCSS
$ sass-convert style.sass style.scss

# 将 SCSS 转换为 Sass
$ sass-convert style.scss style.sass
```

## 使用 Sass

Sass 有三种使用方式： 命令行工具、独立的 Ruby 模块，以及包含 Ruby on Rails 和 Merb 作为支持 Rack 的框架的插件。 所有这些方式的第一步都是安装 Sass gem：

```shell
gem install sass
```

如果要在命令行中运行 Sass ,只要输入

```shell
sass input.scss output.css
```

你还可以命令 Sass 监视文件的改动并更新 CSS ：

```shell
sass --watch input.scss:output.css
```

如果你的目录里有很多 Sass 文件，你还可以命令 Sass 监视整个目录：

```shell
sass --watch app/sass:public/stylesheets
```

使用 sass --help 可以列出完整的帮助文档。

## 缓存

默认情况下，Sass 会对编译过的模板（template）和partials 进行缓存。 这将明显加快大量 Sass 文件的重新编译速度， 并且在 Sass 模板被切割为多个文件并通过 @import 引入形成一个大文件时效果最好。

如果不使用框架，Sass 将会把缓存的模板放入 .sass-cache 目录。 在 Rails 和 Merb 中，将被放到 tmp/sass-cache 目录。 此目录可以通过 :cache_location 选项进行配置。 如果你不希望 Sass 启用缓存功能， 可以将 :cache 选项设置为 false。

## 用法

### 变量

SASS允许使用变量，所有变量以$开头。

```css
$blue : #1875e7;　
div {
    color : $blue;
}
```

如果变量需要镶嵌在字符串之中，就必须需要写在#{}之中。

```css
$side : left;
.rounded {
    border-#{$side}-radius: 5px;
}
```

### 计算

SASS允许在代码中使用算式：

```css
body {
    margin: (14px/2);
    top: 50px + 100px;
    right: $var * 10%;
}
```



### Nested Rules(嵌入规则)

Sass允许选择器相互嵌套。比如，下面的CSS代码

```css
div h1 {
    color : red;
}
```

可以改成

```css
div {
    hi {
        color:red;
    }
}
```

属性也可以嵌套，比如border-color属性，可以写成：

```css
p {
    border: {
        color: red;
    }
}
```

**注意**：border后面必须加上冒号。

在嵌套的代码块内，可以使用&引用父元素。比如a:hover伪类，可以写成：

```css
a {
    &:hover { color: #ffb3ff; }
}
```

## 代码的重用

### 继承

SASS允许一个选择器，继承另一个选择器。比如，现有class1：

```css
.class1 {
    border: 1px solid #ddd;
}
```

class2要继承class1，就要使用@extend命令：

```css
.class2 {
    @extend .class1;
    font-size:120%;
}
```

### Mixin

Mixin有点像C语言的宏（macro），是可以重用的代码块。  

使用@mixin命令，定义一个代码块。

```css
@mixin left {
    float: left;
    margin-left: 10px;
}
```

使用@include命令，调用这个mixin。

```css
div {
    @include left;
}
```

mixin的强大之处，在于可以指定参数和缺省值。

```css
@mixin left($value: 10px) {
    float: left;
    margin-right: $value;
}
```

使用的时候，根据需要加入参数：

```css
div {
    @include left(20px);
}
```

下面是一个mixin的实例，用来生成浏览器前缀。

```css
@mixin rounded($vert, $horz, $radius: 10px) {
    -webkit-border-#{$vert}-#{$horz}-radius: $radius;
        -moz-border-radius-#{$vert}#{$horz}: $radius;
            border-#{$vert}-#{$horz}-radius: $radius;
　　}
```

使用的时候，可以像下面这样调用：

```css
#navbar li { @include rounded(top, left); }
#footer { @include rounded(top, left, 5px); }
```

### 颜色函数

SASS提供了一些内置的颜色函数，以便生成系列颜色。

```javascript
lighten(#cc3, 10%) // #d6d65c
darken(#cc3, 10%) // #a3a329
grayscale(#cc3) // #808080
complement(#cc3) // #33c
```

### 插入文件

@import命令，用来插入外部文件。

```css
@import "path/filename.scss";
```

如果插入的是.css文件，则等同于css的import命令。

```css
@import "foo.css";
```

## 高级用法

### if 语句

@if可以用来判断：

```css
p {
    @if 1 + 1 == 2 { border: 1px solid; }
    @if 5 < 3 { border: 2px dotted; }
}
```

配套的还有@else命令：

```css
@if lightness($color) > 30% {
    background-color: #000;
} @else {
    background-color: #fff;
}
```

### 循环语句

SASS支持for循环：

```css
@for $i from 1 to 10 {
    .border-#{$i} {
        border: #{$i}px solid blue;
    }
}
```

也支持while循环：

```css
$i: 6;
@while $i > 0 {
    .item-#{$i} { width: 2em * $i; }
    $i: $i - 2;
}
```

each命令，作用与for类似：

```css
@each $member in a, b, c, d {
    .#{$member} {
        background-image: url("/image/#{$member}.jpg");
    }
}
```

### 自定义函数

SASS允许用户编写自己的函数。

```css
@function double($n) {
    @return $n * 2;
}
#sidebar {
    width: double(5px);
}
```







## 参考链接

- 官网,[官网](http://sass.bootcss.com/)
- 在线转换器, [在线转换器](http://www.sassmeister.com/)

