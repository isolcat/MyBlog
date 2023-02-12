---
title: 深入理解Vue响应式
author: isolcat
datetime: 2022-2-12T15:33:51Z
featured: true
draft: false
tags:
  - vue
description: 深入分析Vue响应式
---
# Vue响应式

## 前言

响应式在日常开发中的应用，举个简单的例子：

```js
let a=3
let b=a*10
console.log(b)//30
a=4
console.log(b)//40
```

这时候我们想让b=4*10，这样显然是不行的，即使我们在前面加上个`var`也只会发生`变量提升`，我们所给的值并不会提升。

而这个时候，响应式的作用就体现出来了：

```js
import { reactive } from 'vue'

let state = reactive({ a: 3 })
let b = computed(() => state.a * 10)
console.log(b.value) // 30
state.a = 4
console.log(b.value) // 40
```

只需要一个简单的`响应式API`便可实现跟踪变化的效果

## 分析一下reactive

事实上，Vue3的`reactive`的本质上就是一个**发布订阅模式**

通过创建依赖图来跟踪数据的依赖关系。依赖图是一个图形，它描述了哪些数据是依赖于哪些数据的。当数据发生变化时，Vue 3 的 `reactive` 系统会自动触发视图的更新。这是因为它在依赖图中跟踪了数据变化，并通过将其与视图的更新关联起来来实现的

这里我列出尤大在Vue Master里演示的代码做个简单的示例：

```js
class Dep{
    constructor(value){
        this.subscribers=new Set()
        this._value=value
    }
    get value(){
        this.depend()
        return this._value
    }
    set value(newValue){
        this._value=newValue
        this.notify()
    }
    depend(){
        if(activeEffect){
            this.subscribers.add(activeEffect)
        }
    }
    notify(){
        this.subscribers.forEach(effect=>{
            effect()
        })
    }
}
```

我们来分析一下这段代码：

> - 定义一个`subscribe`属性，作为一个订阅者列表，用来存储所有的订阅者信息
> - `depend`函数用来管理依赖关系，即该订阅者所依赖的该变量
> - `notify`函数便是作为通知所有订阅者该变量的值已经发生变化

当变量的值发生变化的时候，它便可以自动的通知所有的订阅者进行更新了

## Vue2的Object.defineProperty

事实上，在Vue2时期，响应式的都是交给`Object.defineProperty `来实现的，但在Vue3当中切换成了`Proxy`，我们等下来结合实际代码来看原因；我们先来看看Vue2是如何实现的：

```js
function reactive(raw){
    Object.keys(raw).forEach(ket=>{
        const dep=new Dep()
        let value=raw[key]
        
        Object.definProperty(raw,key,{
            get(){
                dep.depend()
                return value
            },
            //当属性发生
            set(newValue){
                value=newValue
                dep.notify()
            }
        })
    })
    //这时候返回的原始对象已经具有响应性
    return raw
}
```

这样一个简单的响应式API就实现了

但这里缺点也就很明显了：**在 Vue 2.x 中，被传入的对象会直接被 `Vue.observable` 变更** 而在Vue3当中，则是会**返回一个可响应的代理，而对源对象直接进行变更仍然是不可响应的**

这就导致了：

> - 当我们`添加或者删除`对象的属性时，Vue2的响应式是无法检测的，由于 Vue 会在初始化实例时对 property 执行 getter/setter 转化，所以 property 必须在 `data` 对象上存在才能让 Vue 将它转换为响应式的
> - 无法检测**数组**的`下标和长度`的变化

当然，这个属于历史局限了，当时ES5也就只能选择`Object.definProperty`，但在了ES6版本，便多了`Proxy`，这时候Vue的响应式便得到了升级

## Vue3的Proxy

Vue3采用`Proxy`来监控数据的变化，相较于Vue2来说，不仅解决了上述的问题，还有这些优势：

> - 无需再使用`vue.$set`来触发响应式，这让代码看上去能过更加**简介**
> - 全方位的数组变化检测，消除Vue2中无效边界情况
> - 减少了Vue3中书写响应式的代码量，这让我们的开发更加方便

让我们来看看实际代码是什么样子的：

```js
const reactiveHandles={
    get(target,key,receiver){
        const dep=getDep(target,key)
        dep.depend()
        return Reflect.get(target,key,receiver)
    },
    set(target,key,value,receiver){
        const dep=getDep(target,key)
        const result=Reflect.set(target,key,value,receiver)
        dep.notify()
        return result
    }
}
```

通过对对象进行**收集依赖**来实现响应式的方式也便是Vue3响应式的精髓

## ref

在官方文档有句话：`reactive()` 的种种限制归根结底是因为 JavaScript 没有可以作用于所有值类型的 “引用” 机制，而reactive的限制便是：

> - 只能处理可被观测的数据结构，如数组和对象；而不可观测的数据结构，如原始数据类型就无法被其监测
> - 只能处理定义在它所在组件的数据，不能处理全局变量

而这个时候就需要`ref`来登场了，ref便是针对**基本数据类型**而诞生的，弥补了reactive的缺陷，简单的来说，ref更加适合简单的单个可变值(不过实际开发大多数时候都是ref一把梭哈哈哈😂

这里顺便提一下，响应式语法糖的提案被取消了还是蛮可惜的😢