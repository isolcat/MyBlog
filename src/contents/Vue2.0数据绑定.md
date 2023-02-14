---
author: isolcat
datetime: 2022-02-26T04:58:53Z
title: Vue2数据绑定
featured: false
draft: false
tags:
  - Vue
  - 面试题
ogImage: ""
description: vue2数据绑定相关面试题
---

# Vue2.0双向绑定的原理与缺陷

Vue响应式指的是：组件的data发生变化，立刻触发试图的更新 

## 原理

Vue 采用数据劫持结合发布者-订阅者模式的方式来实现数据的响应式，通过`Object.defineProperty`（在vue3里改为了`proxy`）来劫持数据的setter，getter，在数据变动时发布消息给订阅者，订阅者收到消息后进行相应的处理。 通过原生js提供的监听数据的API，当数据发生变化的时候，在回调函数中修改dom 

## 核心API：

`Object.defineProperty    Object.defineProperty` API的使用 作用: 用来定义对象属性 特点： 默认情况下定义的数据的属性不能修改 描述属性和存取属性不能同时使用，使用会报错 响应式原理： 获取属性值会触发getter方法 设置属性值会触发setter方法 在setter方法中调用修改dom的方法 加分回答 

## Object.defineProperty的缺点

1. 一次性递归到底开销很大，如果数据很大，大量的递归导致调用栈溢出 
2. 不能监听对象的新增属性和删除属性
3. 无法正确的监听数组的方法，当监听的下标对应的数据发生改变时