---
title: AngularJS 入门
layout: page
category: angularjs
date: 2016-07-11
modifiedOn: 2016-07-11
---

## 概述

AngularJS 是一个为动态WEB应用设计的结构框架。它能让你使用HTML作为模板语言，通过扩展HTML的语法，让你能更清楚、简洁地构建你的应用组件。它的创新点在于，利用 数据绑定 和 依赖注入，它使你不用再写大量的代码了。这些全都是通过浏览器端的Javascript实现，这也使得它能够完美地和任何服务器端技术结合。

通常，我们是通过以下技术来解决静态网页技术在构建动态应用上的不足：

- 类库 - 类库是一些函数的集合，它能帮助你写WEB应用。起主导作用的是你的代码，由你来决定何时使用类库。例如`jQuery`
- 框架 - 框架是一种特殊的、已经实现了的WEB应用，你只需要对它填充具体的业务逻辑。这里框架是起主导作用的，由它来根据具体的应用逻辑来调用你的代码。例如 `knockout`、`sproutcore`等。

AngularJS试图成为成为WEB应用中的一种端对端的解决方案。这意味着它不只是你的WEB应用中的一个小部分，而是一个完整的端对端的解决方案。这会让AngularJS在构建一个CRUD（增加Create、查询Retrieve、更新Update、删除Delete）的应用时显得很单一, 使得AngularJS更适合于后台管理系统的开发 CRUD，也就是网站的后台管理系统等  

AngularJS通过为开发者呈现一个更高层次的抽象来简化应用的开发。如同其他的抽象技术一样，这也会损失一部分灵活性。换句话说，并不是所有的应用都适合用AngularJS来做。AngularJS主要考虑的是构建CRUD应用。幸运的是，至少90%的WEB应用都是CRUD应用   


## script标签

这个示例展示了我们推荐的整合AngularJS的方法，它被称之为“自动初始化”。

```html
<!doctype html>
<html xmlns:ng="http://angularjs.org" ng-app>
    <body>
        ...
    <script src="angular.js"></script>
    </body>
</html>
```

- 将上面代码中的script标签放在页面的底部。将script标签放在底部缩短应用加载的时间，因为这样HTML的加载不会被angular.js脚本的加载阻塞。你可以从http://code.angularjs.org获得最新的版本。请不要在你的代码里面引用这个URL，因为它会暴露你的站点的安全隐患。如果只是实验性质的开发，那链接到我们的站点没有什么问题。
- 请将ng-app指令 放到你的应用的标签根节点中, 如果你想要AngularJS自动执行整个<html>程序就把它放在 <html> 标签中。

```html
<html ng-app>
```

- 如果你想使用旧版的指令语法：ng:，那还需要将xml-namespace写在<html>中 才能使AngularJS在IE下正常工作。(这样做是因为一些历史原因, 我们不推荐继续使用ng:的语法。)

```html
<html xmlns:ng="http://angularjs.org">
```

### 自动初始化

AngularJS会在DOMContentLoaded事件触发时执行，并通过ng-app指令 寻找你的应用根作用域。如果 ng-app指令找到了，那么AngularJS将会：

- 载入和 指令 内容相关的模块。
- 创建一个应用的“注入器(injector)”。
- 已拥有ng-app 指令 的标签为根节点来编译其中的DOM。这使得你可以只指定DOM中的一部分作为你的AngularJS应用。

```html
<!doctype html>
<html ng-app="optionalModuleName">
    <body>
        I can add: {{ 1+2 }}.
        <script src="angular.js"></script>
    </body>
</html>
```

### 手动初始化


如果你需要主动控制一下初始化的过程，你可以使用手动执行引导程序的方法。比如当你使用“脚本加载器(script loader)”，或者需要在AngularJS编译页面之前做一些操作，你就会用到它了。

下面的例子演示了手动初始化AngularJS的方法。它的效果等同于使用ng-app指令 。

```html
<!doctype html>
<html xmlns:ng="http://angularjs.org">
    <body>
        Hello {{'World'}}!
        <script src="http://code.angularjs.org/angular.js"></script>
        <script>
            angular.element(document).ready(function() {
            angular.bootstrap(document);
            });
        </script>
    </body>
</html>
```





## 参考链接

- Angular 2.0, [Angular 2.0](https://angular.cn/)
- Angular API 1.XX, [Angular API 1.XX](http://docs.ngnice.com/api)
- AngularJS中文网, [AngularJS中文网](http://www.apjs.net/)


