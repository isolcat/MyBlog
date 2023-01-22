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

### 简易版本：

```js
function fn(arg1,arg2){
  console.log('fn执行了',this.name,arg1,arg2);
  return '我是返回值'
}
let thisarg={
  name: 'Yang'
}

//实现myCall
function myCall(fn,thisarg,...args){
  thisarg.fnName = fn  //核心：利用隐式绑定
  const result= thisarg.fnName(...args)  //执行隐式绑定函数并返回结果
  delete thisarg.fn //销毁隐式绑定函数
  return result //返回值
}

let result=myCall(fn,thisarg,'1',"2") //fn执行了 Yang 1 2
console.log(result); //我是返回值
```

#### 两个缺陷：

- **若已经有 thisarg.fn 存在,则起名冲突**
- **与原生fn.call(thisarg , ...args)的形式不同**

### 进阶版本：

**使用Symbol创建独一无二的隐式绑定this函数**

```js
function myCall(fn,thisarg,...args){
  let fnName=Symbol()
  thisarg[fnName]=fn
  const result = thisarg[fnName](...args)
  delete thisarg[fnName]
  return result
}
```

### 究极版本：

**在 Function原型 添加 myCall方法 并完成 fn.myCall( )形式的调用**

```js
Function.prototype.myCall=function(thisarg,...args){
  let fnName=Symbol()
  thisarg[fnName]=this  //this指向被调用的函数fn
  const result = thisarg[fnName](...args)
  delete thisarg[fnName]
  return result
}

let result=qaq.myCall(thisarg,'1',"2") //第一个参数用this代替
//fn执行了 Yang 1 2
console.log(result);
//我是返回值
```

### 最简易版本

```js
    Function.prototype.myCall=function(thisArg,...args){
      thisArg.t=this
      return thisArg.t(...args)
    }
```

## 实现apply方法：

传入的参数必须为数组，然后使用扩展运算符拆分出来

```js
Function.prototype.myApply=function(thisarg,args){ 
  let fnName=Symbol()
  thisarg[fnName]=this  //this指向被调用的函数fn
  const result = thisarg[fnName](...args)
  delete thisarg[fnName]
  return result
}
```

## 实现bind方法：

与call方法不同

1.返回值为改变this指向后的可调用函数

2.改变this指向时传入的参数依然保留

3.函数被new时，this指向回到最初

```js
Function.prototype.myBind=function(thisarg,...args1){
let fn= this
if(new.target == undefined)
{
  return function(...args2){
  let fnName=Symbol()
  thisarg[fnName]=fn
  const result = thisarg[fnName](...args1,...args2)
  delete thisarg[fnName]
  return result
}
}
  else{
    return fn()
  }
}
-------------------------------------------------------------
function say(arg1,arg2){
  console.log(this.age,arg1,arg2)
}
let person={
  age:3
}

let bindSay = say.bind(person,"我叫",'Yang')

bindSay() //3 '我叫' 'Yang'
new bindSay() //undefined '我叫' 'Yang' //调用了new this指向复原
```