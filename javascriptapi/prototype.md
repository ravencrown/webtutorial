---
title: 原型和原型链
layout: page
category: javascriptapi
date: 2016-07-05	
modifiedOn: 2016-07-05
---

JavaScript 不包含传统的类继承模型，而是使用 `prototypal` 原型模型。

## 构造函数

典型的面向对象编程语言（比如C++和Java），存在“类”（class）这个概念。所谓“类”就是对象的模板，对象就是“类”的实例。但是，JavaScript语言的对象体系，不是基于“类”的，而是基于构造函数（`constructor`）和原型链（`prototype`）。

JavaScript语言使用构造函数（`constructor`）作为对象的模板。所谓“构造函数”，就是专门用来生成“对象”的函数。它提供模板，描述对象的基本结构。一个构造函数，可以生成多个对象，这些对象都有相同的结构。


构造函数的写法就是一个普通的函数，但是有自己的特征和用法。

```javascript
var Vehicle = function () {
    this.price = 1000;
};
```

上面代码中，Vehicle就是构造函数，它提供模板，用来生成对象实例。为了与普通函数区别，构造函数名字的第一个字母通常大写。

构造函数的特点有两个。

- 函数体内部使用了this关键字，代表了所要生成的对象实例。
- 生成对象的时候，必需用new命令，调用Vehicle函数。




















