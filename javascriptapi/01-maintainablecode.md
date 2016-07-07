---
title: 高质量JavaScript代码的基本要点
layout: page
category: javascriptapi
date: 2016-06-29
modifiedOn: 2016-06-29
---

软件bug的修复是昂贵的，并且随着时间的推移，这些bug的成本也会增加，尤其当这些bug潜伏并慢慢出现在已经发布的软件中时。当你发现bug 的时候就立即修复它是最好的，此时你代码要解决的问题在你脑中还是很清晰的。否则，你转移到其他任务，忘了那个特定的代码，一段时间后再去查看这些代码就需要花费额外的时间，因此，我们需要编写高质量的JavaScript代码

## 最小全局变量

JavaScript通过函数管理作用域。在函数内部声明的变量只在这个函数内部，函数外面不可用。另一方面，全局变量就是在任何函数外面声明的或是未声明直接简单使用的。  
 
每个JavaScript环境有一个全局对象，当你在任意的函数外面使用this的时候可以访问到。你创建的每一个全部变量都成了这个全局对象的属 性。在浏览器中，方便起见，该全局对象有个附加属性叫做`window`，此`window`(通常)指向该全局对象本身。下面的代码片段显示了如何在浏览器环境 中创建和访问的全局变量：

```javascript
myglobal = "hello"; // 不推荐写法
console.log(myglobal); // "hello"
console.log(window.myglobal); // "hello"
console.log(window["myglobal"]); // "hello"
console.log(this.myglobal); // "hello"
```

## 全局变量的问题

全局变量的问题在于，你的JavaScript应用程序和web页面上的所有代码都共享了这些全局变量，他们住在同一个全局命名空间，所以当程序的两个不同部分定义同名但不同作用的全局变量的时候，命名冲突在所难免。  

web页面包含不是该页面开发者所写的代码也是比较常见的，例如：

- 第三方的JavaScript库 
- 广告方的脚本代码
- 第三方用户跟踪和分析脚本代码
- 不同类型的小组件，标志和按钮

比方说，该第三方脚本定义了一个全局变量，叫做`result`；接着，在你的函数中也定义一个名为`result`的全局变量。其结果就是后面的变量覆盖前面的，第三方脚本就一下子嗝屁啦！  

因此，要想和其他脚本成为好邻居的话，尽可能少的使用全局变量是很重要的。在书中后面提到的一些减少全局变量的策略，例如命名空间模式或是函数立即自动执行，但是要想让全局变量少最重要的还是始终使用`var`来声明变量。  

由于JavaScript的两个特征，不自觉地创建出全局变量是出乎意料的容易。首先，你可以甚至不需要声明就可以使用变量；第二，JavaScript有隐含的全局概念，意味着你不声明的任何变量都会成为一个全局对象属性。参考下面的代码：

```javascript
function sum(x, y) {
    // 不推荐写法: 隐式全局变量 
    result = x + y;
    return result;
}
```

此段代码中的result没有声明。代码照样运作正常，但在调用函数后你最后的结果就多一个全局命名空间，这可以是一个问题的根源。经验法则是始终使用`var`声明变量，正如改进版的`sum()`函数所演示的：

```javascript
function sum(x, y) {
    var result = x + y;
    return result;
}
```

另一个创建隐式全局变量的反例就是使用任务链进行部分`var`声明。下面的片段中，a是本地变量但是b确实全局变量，这可能不是你希望发生的：

```javascript
// 反例，勿使用 
function foo() {
    var a = b = 0;
    // ...
}
```

此现象发生的原因在于这个从右到左的赋值，首先，是赋值表达式b = 0，此情况下b是未声明的。这个表达式的返回值是0，然后这个0就分配给了通过`var`定义的这个局部变量a。换句话说，就好比你输入了：

```javascript
var a = (b = 0);
```

如果你已经准备好声明变量，使用链分配是比较好的做法，不会产生任何意料之外的全局变量，如：

```javascript
function foo() {
    var a, b;
    // ... a = b = 0; // 两个均局部变量
}
```

## 忘记var的副作用

隐式全局变量和明确定义的全局变量间有些小的差异，就是通过`delete`操作符让变量未定义的能力。

- 通过var创建的全局变量（任何函数之外的程序中创建）是不能被删除的。
- 无var创建的隐式全局变量（无视是否在函数中创建）是能被删除的。

这表明，在技术上，隐式全局变量并不是真正的全局变量，但它们是全局对象的属性。属性是可以通过`delete`操作符删除的，而变量是不能的：

```javascript
// 定义三个全局变量
var global_var = 1;
global_novar = 2; // 反面教材
(function () {
    global_fromfunc = 3; // 反面教材
}());

// 试图删除
delete global_var; // false
delete global_novar; // true
delete global_fromfunc; // true

// 测试该删除
typeof global_var; // "number"
typeof global_novar; // "undefined"
typeof global_fromfunc; // "undefined"
```

在ES5严格模式下，未声明的变量（如在前面的代码片段中的两个反面教材）工作时会抛出一个错误。

## 访问全局对象

在浏览器中，全局对象可以通过`window`属性在代码的任何位置访问（除非你做了些比较出格的事情，像是声明了一个名为window的局部变量）。但是在其他环境下，这个方便的属性可能被叫做其他什么东西（甚至在程序中不可用）。如果你需要在没有硬编码的`window`标识符下访问全局对象，你可以在任何层级的函数作用域中做如下操作：

```javascript
var global = (function () {
    return this;
}());
```

这种方法可以随时获得全局对象，因为其在函数中被当做函数调用了（不是通过`new`构造），`this`总 是指向全局对象。实际上这个病不适用于`ECMAScript 5`严格模式，所以，在严格模式下时，你必须采取不同的形式。例如，你正在开发一个JavaScript库，你可以将你的代码包裹在一个即时函数中，然后从 全局作用域中，传递一个引用指向`this`作为你即时函数的参数。

## 单var形式

在函数顶部使用单`var`语句是比较有用的一种形式，其好处在于：

- 提供了一个单一的地方去寻找功能所需要的所有局部变量
- 防止变量在定义之前使用的逻辑错误
- 帮助你记住声明的全局变量，因此较少了全局变量//zxx:此处我自己是有点晕乎的…
- 少代码（类型啊传值啊单线完成）

单var形式长得就像下面这个样子：

```javascript
function func() {
    var a = 1,
        b = 2,
        sum = a + b,
        myobject = {},
        i,
        j;
    // function body...
}
```

您可以使用一个`var`语句声明多个变量，并以逗号分隔。像这种初始化变量同时初始化值的做法是很好的。这样子可以防止逻辑错误（所有未初始化但声明的变量的初始值是`undefined`）和增加代码的可读性。在你看到代码后，你可以根据初始化的值知道这些变量大致的用途，例如是要当作对象呢还是当作整数来使。

你也可以在声明的时候做一些实际的工作，例如前面代码中的`sum = a + b`这个情况，另外一个例子就是当你使用`DOM`（文档对象模型）引用时，你可以使用单一的var把`DOM`引用一起指定为局部变量，就如下面代码所示的：

```javascript
function updateElement() {
    var el = document.getElementById("result"),
        style = el.style;
    // 使用el和style干点其他什么事...
}
```

## 预解析：var散布的问题

JavaScript中，你可以在函数的任何位置声明多个`var`语句，并且它们就好像是在函数顶部声明一样发挥作用，这种行为称为 `hoisting（悬置/置顶解析/预解析）`。当你使用了一个变量，然后不久在函数中又重新声明的话，就可能产生逻辑错误。对于JavaScript，只要你的变量是在同一个作用域中（同一函数），它都被当做是声明的，即使是它在`var`声明前使用的时候。看下面这个例子：

```javascript
// 反例
myname = "global"; // 全局变量
function func() {
    alert(myname); // "undefined"
    var myname = "local";
    alert(myname); // "local"
}
func();
```

在这个例子中，你可能会以为第一个alert弹出的是”global”，第二个弹出”loacl”。这种期许是可以理解的，因为在第一个alert 的时候，myname未声明，此时函数肯定很自然而然地看全局变量myname，但是，实际上并不是这么工作的。第一个alert会弹 出”undefined”是因为myname被当做了函数的局部变量（尽管是之后声明的），所有的变量声明当被悬置到函数的顶部了。因此，为了避免这种混 乱，最好是预先声明你想使用的全部变量。  

上面的代码片段执行的行为可能就像下面这样：

```javascript
myname = "global"; // global variable
function func() {
    var myname; // 等同于 -> var myname = undefined;
    alert(myname); // "undefined"
    myname = "local";
    alert(myname); // "local"
}
func();
```

为了完整，我们再提一提执行层面的稍微复杂点的东西。代码处理分两个阶段，第一阶段是变量，函数声明，以及正常格式的参数创建，这是一个解析和进入上下文 的阶段。第二个阶段是代码执行，函数表达式和不合格的标识符（为声明的变量）被创建。但是，出于实用的目的，我们就采用了`hoisting`这个概念， 这种ECMAScript标准中并未定义，通常用来描述行为。

## for循环

在for循环中，你可以循环取得数组或是数组类似对象的值，譬如`arguments`和`HTMLCollection`对象。通常的循环形式如下：

```javascript
// 次佳的循环
for (var i = 0; i < myarray.length; i++) {
    // 使用myarray[i]做点什么
}
```

这种形式的循环的不足在于每次循环的时候数组的长度都要去获取下。这回降低你的代码，尤其当myarray不是数组，而是一个`HTMLCollection`对象的时候。  

HTMLCollections指的是DOM方法返回的对象，例如：

```javascript
document.getElementsByName()
document.getElementsByClassName()
document.getElementsByTagName()
```

还有其他一些HTMLCollections，这些是在DOM标准之前引进并且现在还在使用的。有：

```javascript
document.images // 页面上所有的图片元素
document.links  // 所有a标签元素
document.forms  // 所有表单
document.forms[0].elements // 页面上第一个表单中的所有域
```

集合的麻烦在于它们实时查询基本文档（HTML页面）。这意味着每次你访问任何集合的长度，你要实时查询`DOM`，而`DOM`操作一般都是比较昂贵的。

这就是为什么当你循环获取值时，缓存数组(或集合)的长度是比较好的形式，正如下面代码显示的：

```javascript
for (var i = 0, max = myarray.length; i < max; i++) {
    // 使用myarray[i]做点什么
}
```

这样，在这个循环过程中，你只检索了一次长度值。在所有浏览器下，循环获取内容时缓存`HTMLCollections`的长度是更快的，2倍(Safari3)到190倍(IE7)之间。//zxx:此数据貌似很老，仅供参考注意到，当你明确想要修改循环中的集合的时候（例如，添加更多的DOM元素），你可能更喜欢长度更新而不是常量。伴随着单var形式，你可以把变量从循环中提出来，就像下面这样：

```javascript
function looper() {
    var i = 0,
         max,
         myarray = [];
    // ...
    for (i = 0, max = myarray.length; i < max; i++) {
        // 使用myarray[i]做点什么
    }
}
```

这种形式具有一致性的好处，因为你坚持了单一var形式。不足在于当重构代码的时候，复制和粘贴整个循环有点困难。例如，你从一个函数复制了一个循环到另一个函数，你不得不去确定你能够把i和max引入新的函数（如果在这里没有用的话，很有可能你要从原函数中把它们删掉）。  

最后一个需要对循环进行调整的是使用下面表达式之一来替换i++。

```javascript
i = i + 1
i += 1
```

JSLint提示您这样做，原因是++和–-促进了“过分棘手(excessive trickiness)”。zxx:这里比较难翻译，我想本意应该是让代码变得更加的棘手如果你直接无视它，JSLint的plusplus选项会是false（默认是default）。    
还有两种变化的形式，其又有了些微改进，因为：

- 少了一个变量(无max)
- 向下数到0，通常更快，因为和0做比较要比和数组长度或是其他不是0的东西作比较更有效率

```javascript
//第一种变化的形式：

var i, myarray = [];
for (i = myarray.length; i–-;) {
    // 使用myarray[i]做点什么
}

//第二种使用while循环：

var myarray = [],
    i = myarray.length;
while (i–-) {
    // 使用myarray[i]做点什么
}
```

## for-in循环

for-in循环应该用在非数组对象的遍历上，使用for-in进行循环也被称为“枚举”。  

从技术上将，你可以使用for-in循环数组（因为JavaScript中数组也是对象），但这是不推荐的。因为如果数组对象已被自定义的功能增强，就可能发生逻辑错误。另外，在for-in中，属性列表的顺序（序列）是不能保证的。所以最好数组使用正常的for循环，对象使用for-in循环。  

有个很重要的`hasOwnProperty()`方法，当遍历对象属性的时候可以过滤掉从原型链上下来的属性。  

思考下面一段代码：

```javascript
// 对象
var man = {
    hands: 2,
    legs: 2,
    heads: 1
};

// 在代码的某个地方
// 一个方法添加给了所有对象
if (typeof Object.prototype.clone === "undefined") {
    Object.prototype.clone = function () {};
}
```

在这个例子中，我们有一个使用对象字面量定义的名叫man的对象。在man定义完成后的某个地方，在对象原型上增加了一个很有用的名叫 `clone()`的方法。此原型链是实时的，这就意味着所有的对象自动可以访问新的方法。为了避免枚举man的时候出现`clone()`方法，你需要应用`hasOwnProperty()`方法过滤原型属性。如果不做过滤，会导致`clone()`函数显示出来，在大多数情况下这是不希望出现的。

```javascript
// 1.
// for-in 循环
for (var i in man) {
    if (man.hasOwnProperty(i)) { // 过滤
        console.log(i, ":", man[i]);
    }
}
/* 控制台显示结果
hands : 2
legs : 2
heads : 1
*/
// 2.
// 反面例子:
// for-in loop without checking hasOwnProperty()
for (var i in man) {
    console.log(i, ":", man[i]);
}
/*
控制台显示结果
hands : 2
legs : 2
heads : 1
clone: function()
*/
```

另外一种使用`hasOwnProperty()`的形式是取消`Object.prototype`上的方法。像是：

```javascript
for (var i in man) {
    if (Object.prototype.hasOwnProperty.call(man, i)) { // 过滤
        console.log(i, ":", man[i]);
    }
}
```

其好处在于在man对象重新定义`hasOwnProperty`情况下避免命名冲突。也避免了长属性查找对象的所有方法，你可以使用局部变量“缓存”它。

```javascript
var i, hasOwn = Object.prototype.hasOwnProperty;
for (i in man) {
    if (hasOwn.call(man, i)) { // 过滤
        console.log(i, ":", man[i]);
    }
}
```

严格来说，不使用`hasOwnProperty()`并不是一个错误。根据任务以及你对代码的自信程度，你可以跳过它以提高些许的循环速度。但是当你对当前对象内容（和其原型链）不确定的时候，添加hasOwnProperty()更加保险些。  

格式化的变化（通不过JSLint）会直接忽略掉花括号，把if语句放到同一行上。其优点在于循环语句读起来就像一个完整的想法（每个元素都有一个自己的属性”X”，使用”X”干点什么）：

```javascript
// 警告： 通不过JSLint检测
var i, hasOwn = Object.prototype.hasOwnProperty;
for (i in man) if (hasOwn.call(man, i)) { // 过滤
    console.log(i, ":", man[i]);
}
```


## （不）扩展内置原型

扩增构造函数的`prototype`属性是个很强大的增加功能的方法，但有时候它太强大了。  
  
增加内置的构造函数原型（如`Object()`, `Array()`, 或`Function()`）挺诱人的，但是这严重降低了可维护性，因为它让你的代码变得难以预测。使用你代码的其他开发人员很可能更期望使用内置的 JavaScript方法来持续不断地工作，而不是你另加的方法。

另外，属性添加到原型中，可能会导致不使用`hasOwnProperty`属性时在循环中显示出来，这会造成混乱。

因此，不增加内置原型是最好的。你可以指定一个规则，仅当下面的条件均满足时例外：

- 可以预期将来的`ECMAScript`版本或是`JavaScript`实现将一直将此功能当作内置方法来实现。例如，你可以添加`ECMAScript 5`中描述的方法，一直到各个浏览器都迎头赶上。这种情况下，你只是提前定义了有用的方法。
- 如果您检查您的自定义属性或方法已不存在——也许已经在代码的其他地方实现或已经是你支持的浏览器JavaScript引擎部分。
- 你清楚地文档记录并和团队交流了变化。

如果这三个条件得到满足，你可以给原型进行自定义的添加，形式如下：

```javascript
if (typeof Object.protoype.myMethod !== "function") {
    Object.protoype.myMethod = function () {
        // 实现...
    };
}
```

## 避免隐式类型转换

JavaScript的变量在比较的时候会隐式类型转换。这就是为什么一些诸如：false == 0 或 “” == 0 返回的结果是true。为避免引起混乱的隐含类型转换，在你比较值和表达式类型的时候始终使用`===`和`!==`操作符。

```javascript
var zero = 0;
if (zero === false) {
    // 不执行，因为zero为0, 而不是false
}

// 反面示例
if (zero == false) {
    // 执行了...
}
```

还有另外一种思想观点认为==就足够了===是多余的。例如，当你使用typeof你就知道它会返回一个字符串，所以没有使用严格相等的理由。然而，JSLint要求严格相等，它使代码看上去更有一致性，可以降低代码阅读时的精力消耗。（“==是故意的还是一个疏漏？”）




































