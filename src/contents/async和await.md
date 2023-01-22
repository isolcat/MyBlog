---
title: Async Await
author: isolcat
datetime: 2023-1-12T11:19:11Z
featured: false
draft: false
tags:
  - 面试题
description: Async Await
---

# Async Await

## 同步与异步

将`同步`代入我们的生活，可以这么看：起床刷牙-->加热早餐，吃早餐-->上学

而`异步`代入我们的生活，可以这么看：起床-->加热早餐，但在加热的过程中，去刷牙了-->吃早餐-->上学，在异步当中，我们没有等加热完早餐再做另一件事情，**节省**了时间，也就解决了同步编程的痛点

而Async Await就是用`同步编程`的方式去写`异步代码`

## 代码演示

要注意的一点是，`await`**不能单独使用**，必须要结合`async`,而await和async就相当于是promise的`语法糖`

```js
async function bb() {
    console.log('1')
    let two = await Promise.resolve('2')
    console.log(two);
    console.log('3');
    // async其实就是执行promise.resolve，这里显示写上去
    return Promise.resolve('别bb 专心学习')
}
bb().then(value => {
    console.log(value)
})
```