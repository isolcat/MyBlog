---
title: Promise相关面试题
author: isolcat
datetime: 2022-12-13T04:06:31Z
featured: false
draft: false
tags:
  - 面试题
description: promise相关面试题
---

# Promise

## Promise的作用：

Promise是异步微任务，解决了异步多层嵌套回调的问题，让代码的可读性更高，更容易维护 **解决了地狱回调问题**

## Promise使用：

Promise是ES6提供的一个构造函数，可以使用Promise构造函数new一个实例，Promise构造函数接收一个函数作为参数，这个函数有两个参数，分别是两个函数 `resolve`和`reject`，`resolve`将Promise的状态由等待变为成功，将异步操作的结果作为参数传递过去；`reject`则将状态由等待转变为失败，在异步操作失败时调用，将异步操作报出的错误作为参数传递过去。实例创建完成后，可以使用`then`方法分别指定成功或失败的回调函数，也可以使用catch捕获失败，then和catch最终返回的也是一个Promise，所以可以链式调用。 

## Promise的特点：

1. 对象的状态不受外界影响（Promise对象代表一个异步操作，有三种状态）。 - `pending`（执行中） - `Resolved`（成功，又称Fulfilled） - `rejected`（拒绝） 其中pending为初始状态，fulfilled和rejected为结束状态（结束状态表示promise的生命周期已结束）。 
2. **一旦状态改变，就不会再变**，任何时候都可以得到这个结果。 Promise对象的状态改变，只有两种可能（状态凝固了，就不会再变了，会一直保持这个结果）： - 从Pending变为Resolved - 从Pending变为Rejected 
3. resolve 方法的参数是then中回调函数的参数，reject 方法中的参数是catch中的参数 
4. then 方法和 catch方法 只要不报错，返回的都是一个fullfilled状态的promise 加分回答 Promise的其他方法： Promise.resolve() :返回的Promise对象状态为fulfilled，并且将该value传递给对应的then方法。 Promise.reject()：返回一个状态为失败的Promise对象，并将给定的失败信息传递给对应的处理方法。 Promise.all()：返回一个新的promise对象，该promise对象在参数对象里所有的promise对象都成功的时候才会触发成功，一旦有任何一个iterable里面的promise对象失败则立即触发该promise对象的失败。 Promise.any()：接收一个Promise对象的集合，当其中的一个 promise 成功，就返回那个成功的promise的值。 Promise.race()：当参数里的任意一个子promise被成功或失败后，父promise马上也会用子promise的成功返回值或失败详情作为参数调用父promise绑定的相应句柄，并返回该promise对象。