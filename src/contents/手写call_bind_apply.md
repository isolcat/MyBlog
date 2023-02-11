---
title: 手写call、bind、apply
datetime: 2023-1-22T15:25:11Z
featured: false
draft: false
tags:
  - 面试题
description: 手写call、bind、apply
---

# 手写call、bind、apply

## 原理: 利用隐式绑定this实现显示绑定

## 实现call方法：

```js
Function.prototype.myCall = function (context) {
  //判断调用对象
  if (typeof this !== 'function') {
    console.error('type error');
  }
  //获取参数(argument类数组)处理参数，截取第一个参数之后的所有参数
  let args = [...arguments].slice(1);
  result = null;
  //判断context是否传入，如果未传入则设置为window
  context = context || window;
  //将调用函数设置为对象(将函数作为上下文的一个属性)
  context.fn = this;
  //使用上下文对象调用这个方法，并保存返回结果
  result = context.fn(...args);
  //删除刚刚新增的属性
  delete context.fn;
  return result;
};

//测试代码：
function person() {
  console.log(this.name);
}
var cat = {
  name: 'isolcat',
};

person.myCall(cat);//isolcat
```

代码演示：https://stackblitz.com/edit/js-mlt7xo?file=index.js

## 实现apply方法：

传入的参数必须为数组，然后使用扩展运算符拆分出来

```js
Function.prototype.myApply = function (context, args) {
  //判断调用对象是否为函数
  if (typeof this !== 'function') {
    console.log('type error');
  }
  let result = null;
  //判断context是否存在，如果不存在的话则传入window
  context = context || window;
  //将函数作为上下文对象的一个属性
  context.fn = this;
  //使用上下文来调用这个方法，并保存返回结果
  if (args) {
    result = context.fn(...args);
  } else {
    result = context.fn();
  }
  //删除刚刚新增的属性
  delete context.fn;
  return result;
};
let obj = {
  name: 'isolcat',
};

function add(a, b, c) {
  console.log(`${a} + ${b} + ${c} = ${a + b + c}`);
}

add.myApply(obj, [1, 2, 3]);

// 输出：1 + 2 + 3 = 6
```

代码演示：https://stackblitz.com/edit/js-vpihjk?file=index.js

## 实现bind方法：

与call方法不同

1.返回值为改变this指向后的可调用函数

2.改变this指向时传入的参数依然保留

3.函数被new时，this指向回到最初

```js
Function.prototype.myBind = function (context) {
  //判断调用的对象是否为函数
  if (typeof this !== 'function') {
    console.lgo('error');
  }
  //获取参数
  var args = [...arguments].slice(1),
    fn = this;
  //返回一个函数
  return function Fn() {
    //根据调用方式，传入不同的绑定值
    return fn.apply(
      this instanceof Fn ? this : context,
      args.concat(...arguments)
    );
  };
};
//测试代码
const obj = {
  name: 'objName',
};

function func(a, b, c) {
  console.log(this.name);
  console.log(a, b, c);
}

const bindFunc = func.myBind(obj, 1, 2);

bindFunc(3);
// 输出： objName
//       1 2 3
```
代码演示：https://stackblitz.com/edit/js-szmr6f?file=index.js