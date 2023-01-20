---
title: 手撕Promise.all()与Promise.race()
datetime: 2023-1-20T17:15:11Z
featured: false
draft: false
tags:
  - 面试题
description: 手撕Promise.all()与Promise.race()
---

# 手撕Promise.all()与Promise.race()

## 实现Promise.all()

- promise.all接受一个数组，返回一个**新的promise**实例：

  ```js
  Promise.Myall=function(promises)=>{
      return new Promise((resolve,reject)=>{
          
      })
  }
  ```

- 必须得数组中的**所有promise实例成功**，promise.all才成功，所以在这里我们就需要定义一个**数组来收集这些promise实例的resolve结果**，万一数组里面有元素不是 `Promise` —— 那就得用 `Promise.resolve()` 把它办了。这里还有一个问题，`Promise` 实例是**不能直接调用** `resolve` 方法的，咱得在 `.then()` 中去收集结果。注意要保持结果的顺序。

  ```js
  Promise.Myall=function(promises)=>{
      let arr=[]//存储promise的resolve结果
      return new Promise((resolve,reject)=>{
          Promise.forEach((item,i)=>{
              Promise.resolve(item).then(res=>{
                  arr[i]=res//收集结果，将结果存入数组中
              })
          })
      })
  }
  ```

- 如果这些 Promise 中最后一个先完成,那 arr 数组就只有最后一项了，前面的所有项都是 empty。所以这里咱们应该创建一个**计数器**，每有一个 Promise 实例成功，计数器加一

  ```js
  Promise.MyAll = function (promises) {
    let arr = [],//存储promise的resolve结果
      count = 0//进行计数
    return new Promise((resolve, reject) => {
      promises.forEach((item, i) => {
        Promise.resolve(item).then(res => {
          arr[i] = res
          count += 1
          if (count === promises.length) resolve(arr)//如果最终计数器结果与接受数组长度相等，则说明全部resolve
        })
      })
    })
  }
  ```

- 处理**失败的结果**，可以采用**catch（）**方法进行**捕获失败**

  ```js
  Promise.MyAll = function (promises) {
    let arr = [],//存储promise的resolve结果
      count = 0//进行计数
    return new Promise((resolve, reject) => {
      promises.forEach((item, i) => {
        Promise.resolve(item).then(res => {
          arr[i] = res
          count += 1
          if (count === promises.length) resolve(arr)//如果最终计数器结果与接受数组长度相等，则说明全部resolve
        }).catch(reject)
      })
    })
  }
  ```

到这里，`Promise.all()`便手撕完成了

## 实现Promise.race()

> `Promise.race` 从字面意思理解就是赛跑，以状态变化最快的那个 `Promise` 实例为准，最快的 `Promise` 成功 `Promise.race` 就成功，最快的 `Promise` 失败 `Promise.race` 就失败。

整体流程与`Promise`差不多，只是对数组中的`Promise`实例处理的逻辑不一样，这里我们需要将最快改变状态的`Promise` 结果作为`Promise.race`的结果，相对来说就比较简单了，代码如下：

```js
Promise.MyRace=function(promises){
    return new Promise((resolve,reject)=>{
        //这里无需索引，只要可以循环出每一项即可
        for(const item of promises){
            Promise.resolve(item).then(resolve,reject)
        }
    })
}
```

