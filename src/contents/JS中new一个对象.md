---
title: JS 中 new 一个对象的过程
author: isolcat
datetime: 2022-2-7T15:33:51Z
featured: false
draft: false
tags:
  - 面试题
description: JS 中 new 一个对象的过程
---
## JS 中 new 一个对象的过程

（1） 创建一个空的简单对象
（2） 为步骤 1 新创建的对象添加属性`__proto__`,该属性连接至构造函数的原型对象
（3） 将步骤 1 新创建的对象作为 this 的上下文
（4） 执行该函数，如果该函数没有返回对象，则返回 this

## new 出来的这个对象的指向
当 new 出一个对象时，this 指向那里
## new 出一个对象时，构造函数的返回值类型

就是前面步骤里说的，如果这个对象指向的构造函数有返回值的话，那么它的返回类型就是根
据这个返回值来的，如果没有返回值的话，就返回 this，即这个 new 出来的对象

## 如果new一个箭头函数会发生什么？
箭头函数在es6中被提出，它没有`prototype`，也没有自己的this指向，更不可以使用`argument`参数，所以是不能new一个箭头函数的
new操作符的步骤：
![image.png](https://ldbbs.ldmnq.com/bbs/topic/attachment/2023-2/45938a3f-a4f3-472f-880a-f3ad804deccb.png)
