---
title: this指向
author: isolcat
datetime: 2022-12-4T16:55:12.000+00:00
featured: false
draft: false
tags:
  - 面试题
description:
  this指向相关的面试题
---

# this指向

## this关键字由来：

**在对象内部的方法中使用对象内部的属性**是一个非常普遍的需求。但是 JavaScript 的作用域机制并不支持这一点，基于这个需求，JavaScript 又搞出来另外一套 this 机制。 

## this存在的场景:

有**三种**:`全局执行上下文`和`函数执行上下文`和`eval执行上下文`，eval这种不讨论。

- 在全局执行环境中无论是否在严格模式下，（在任何函数体外部）`this` 都指向全局对象。

- 在函数执行上下文中访问this，函数的调用方式决定了 `this` 的值。

  > 在`全局环境`中调用一个函数，函数内部的 this 指向的是全局变量 window，通过一个对象来调用其内部的一个方法，该方法的执行上下文中的 this 指向对象本身。 

  > 普通`函数this指向`：当函数被正常调用时，在严格模式下，this 值是 undefined，非严格模式下 this 指向的是全局对象 window；通过一个对象来调用其内部的一个方法，该方法的执行上下文中的 this 指向对象本身。new 关键字构建好了一个新对象，并且构造函数中的 this 其实就是新对象本身。嵌套函数中的 this 不会继承外层函数的 this 值。 箭头函数this指向：箭头函数并不会创建其自身的执行上下文，所以箭头函数中的 this 取决于它的外部函数。 

## 加分回答

箭头函数因为没有this，所以也不能作为构造函数，但是需要继承函数外部this的时候，使用箭头函数比较方便 

```js
var myObj = { name : "闷倒驴", showThis:function(){ console.log(this); // myObj var bar = ()=>{ this.name = "王美丽"; console.log(this) // myObj } bar(); } }; myObj.showThis(); console.log(myObj.name); // "王美丽" console.log(window.name); // ''
```

