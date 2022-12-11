---
author: isolcat
datetime: 2022-12-6T04:58:53Z
title: null和undefined的区别
featured: false
draft: false
tags:
  - 面试题
ogImage: ""
description: null和undefined的区别
---

# null和undefined的区别

## undefind

`undefind`是全局对象的一个属性，当一个变量**没有被赋值或者一个函数没有返回值或者某个对象不存在某个属性却去访问或者函数定义了形参但没有传递实参**，这时候都是undefined。undefined通过`typeof判断类型是'undefined'`。undefined == undefined undefined === undefined 。 

## null

`null`代表对象的值**未设置**，相当于一个对象没有设置指针地址就是null。`null通过typeof判断类型是'object'`。null === null null == null null == undefined null !== undefined undefined 表示一个变量初始状态值，而 null 则表示一个变量被人为的设置为空对象，而不是原始状态。在实际使用过程中，不需要对一个变量显式的赋值 undefined，当需要释放一个对象时，直接赋值为 null 即可。 让一个变量为null，直接给该变量赋值为null即可。 加分回答 null 其实属于自己的类型 Null，而不属于Object类型，typeof 之所以会判定为 Object 类型，是因为JavaScript 数据类型在底层都是以二进制的形式表示的，二进制的前三位为 0 会被 typeof 判断为对象类型，而 null 的二进制位恰好都是 0 ，因此，null 被误判断为 Object 类型。 对象被赋值了null 以后，对象对应的堆内存中的值就是游离状态了，GC 会择机回收该值并释放内存。因此，需要释放某个对象，就将变量设置为 null，即表示该对象已经被清空，目前无效状态。