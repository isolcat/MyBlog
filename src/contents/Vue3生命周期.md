---
author: isolcat
datetime: 2022-03-5T12:13:24Z
title: Vue3生命周期函数
featured: false
draft: false
tags:
  - Vue3
ogImage: ""
description:
  Vue3生命周期函数快速上手
---

# Vue3生命周期

## 什么是生命周期？

无论我们使用什么框架，在使用框架的时候，都会用上一个非常重要的特性：**生命周期**钩子，这些生命周期能够让我们针对数据更新时的每一个阶段，都能够做出相应的相应，极大方便的我们的编译过程。

甚至由于生命周期钩子使用的次数太多了，也诞生了一些相关的调侃图：

<img src="https://ae01.alicdn.com/kf/H1462d761fa984011b521b79046eefb377.png" alt="调侃生命周期钩子使用之多" style="zoom:33%;" />



## Vue生命周期有哪些？

首先附上一份Vue官方的生命周期图（*Vue3的生命周期图和Vue2有着细小的区别*）

![Vue3生命周期图](https://ae01.alicdn.com/kf/Hc3ed1160814b458da5e115c2b709a04b4.png)

再看一看官方对于该图上出现的生命周期钩子的定义

### beforeCreate

在实例初始化之后、进行数据侦听和事件/侦听器的配置之前同步调用。

### created

在实例创建完成后被立即同步调用。在这一步中，实例已完成对选项的处理，意味着以下内容已被配置完毕：数据侦听、计算属性、方法、事件/侦听器的回调函数。然而，挂载阶段还没开始，且 `$el` property 目前尚不可用。

### beforeMount

在挂载开始之前被调用：相关的 `render` 函数首次被调用。

### mount

在实例挂载完成后被调用，这时候传递给 [`app.mount`](https://v3.cn.vuejs.org/api/application-api.html#mount) 的元素已经被新创建的 `vm.$el` 替换了。如果根实例被挂载到了一个文档内的元素上，当 `mounted` 被调用时， `vm.$el` 也会在文档内。 注意 `mounted` **不会**保证所有的子组件也都被挂载完成。如果你希望等待整个视图都渲染完毕，可以在 `mounted` 内部使用 [vm.$nextTick](https://v3.cn.vuejs.org/api/instance-methods.html#nexttick)：

### beforeUpdate

在数据发生改变后，DOM 被更新之前被调用。这里适合在现有 DOM 将要被更新之前访问它，比如移除手动添加的事件监听器。

### update

在数据更改导致的虚拟 DOM 重新渲染和更新完毕之后被调用。

当这个钩子被调用时，组件 DOM 已经更新，所以你现在可以执行依赖于 DOM 的操作。然而在大多数情况下，你应该避免在此期间更改状态。如果要相应状态改变，通常最好使用[计算属性](https://v3.cn.vuejs.org/api/options-data.html#computed)或[侦听器](https://v3.cn.vuejs.org/api/options-data.html#watch)取而代之。

注意，`updated` **不会**保证所有的子组件也都被重新渲染完毕。如果你希望等待整个视图都渲染完毕，可以在 `updated` 内部使用 [vm.$nextTick](https://v3.cn.vuejs.org/api/instance-methods.html#nexttick)：

### beforeUnmount

在卸载组件实例之前调用。在这个阶段，实例仍然是完全正常的。

**该钩子在服务器端渲染期间不被调用。**

###  unmounted

卸载组件实例后调用。调用此钩子时，组件实例的所有指令都被解除绑定，所有事件侦听器都被移除，所有子组件实例被卸载。

**该钩子在服务器端渲染期间不被调用。**



如果看了官方文档对于生命周期钩子的理解仍然感到抽象的话，可以接下来看看实际的例子



## 实际案例

下面的代码为components中的Demo组件：

<img src="https://ae04.alicdn.com/kf/Hd47bed089e454608b003b5d6932a741cp.png" alt="代码" style="zoom:33%;" />

在网页刚刚创建的时候，我们可以打开控制台进行查看：

![](https://ae01.alicdn.com/kf/He7e3581b4a7347b2874b8bc3ce0dee4aK.png)

这时候我们可以清楚的看见，网页在被创建的时候调用了**beforeCreate  Created beforeMount  mounted**这四个生命周期钩子，但要注意：**此时的生命周期钩子并非同时调用的**！

#### beforeCreate

这时候的实例刚刚被创建，而该钩子的便需要在实例被初始化之后即可进行调用，这便是在**初始化界面之前**就需要被调用的

#### created

created在实例创建彻底完成后才进行调用，故该生命周期钩子于**初始界面后调用**

#### beforeMount

该钩子的存在感稍弱，在该处只对render方法（*前提是必须存在的*）进行了绑定，在绑定完成后便执行了beforeMount钩子，故该钩子是在进行**Dom渲染前调用的**

#### mount

该钩子相对的就是在beforeMount之后再被调用了，故为**Dom渲染后调用**



接着，我们点击网页的“点我+1”按钮，再继续观察控制台的变化：

![](https://ae03.alicdn.com/kf/He023320f3f664d61a54c046621413b16P.png)

此时的控制台调用了**beforeUpdate与updated**函数，是因为在此时，一直被监听的数据得到了更新，故可得**beforeUpdated是在更新数据前进行了调用**，而**update**则是在**更新数据后得到了调用**



再接着，我们试着点击当前页面的“切换隐藏”按钮，将数据进行修改，得到了控制台如下的反馈：

![](https://ae02.alicdn.com/kf/H462ed6ce281d4ef3ae3991347644deebG.png)

由此可得，该实例在被进行销毁后，调用的是**beforeUnmount与unmounted**，而beforeUmount则是在**销毁组件前**，相对的，unmounted**则是在销毁组件后被调用**

## 每个生命周期的作用是什么？
- created：实例已经创建完成，因为他是最早触发的，所以可以进行一些数据、资源的请求。
- mounted：实例已经挂载完成，可以进行一些DOM操作。
- beforeUpdate：可以在这个钩子中进一步的更改状态，不会触发重渲染。
- updated：可以执行依赖于DOM的操作，但是要避免更改状态，可能会导致更新无线循环。

## ajax放在哪个生命周期？：
一般放在 `mounted` 中，保证逻辑统一性，因为生命周期是同步执行的， ajax 是异步执行的。单数服务端渲染 ssr 同一放在 created 中，因为服务端渲染不支持 mounted 方法。

## 父子组件的生命周期调用顺序：
-渲染顺序：先父后子，完成顺序：先子后父

-更新顺序：父更新导致子更新，子更新完成后父

-销毁顺序：先父后子，完成顺序：先子后父

## 总结

无论你选择选项API还是组合API，我们都应该去了解生命周期钩子，明白当页面进行了操作后会调用哪个生命周期钩子，这样才能使我们的更多相关的操作的想法能够在不同的生命周期钩子当中得到实现。
