---
title: 手写new
datetime: 2023-1-22T14:00:11Z
featured: false
draft: false
tags:
  - 面试题
description: 手写new
---
# 手写new

> `new`的作用是将**构造函数生成它的实例**

`fn`为一个构造函数，`...args`由于剩余参数不固定，所以用剩余参数的方法，将其整合成一个数组

```js
function myNew(fn,...args){
    //1.定义一个空的对象
    const obj={}
    //2.隐式原型指向构造函数的显示原型
    obj._proto_=fn.prototype
    //3.执行构造函数，this指向空对象
    fn.apply(obj,args)
    //4.返回对象
    return obj
}
```

