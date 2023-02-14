---
title: Vue八股文
datetime: 2023-2-2T18:00:11Z
featured: false
draft: false
tags:
  - 面试题
  - Vue
description: 一些面试中常考的Vue知识点
---
# Vue八股文

## computed和watch的区别

computed和methods的差异是它(`computed`)具备**缓存性**，如果依赖项不变时不会重新计算。

侦听器**可以检测某个响应式数据的变化并执行副作用**，常见用法是传递一个函数，执行副作用，`watch`没有返回值，但可以执行**异步**操作等复杂逻辑。（一般用来请求接口）

### 使用场景

有异步请求的用Wach，其他情况能用计算属性就首选计算属性

## v-if和v-show的区别

v-show 总是会进行编译和渲染的工作 - 它只是简单的在元素上添加了 `display: none;` 的样式。v-show 具有较高的初始化性能成本上的消耗，但是使得转换状态变得很容易。 相比之下，v-if 才是真正「有条件」的：它的加载是惰性的，因此，若它的初始条件是 false，它就不会做任何事情。这对于初始加载时间来说是有益的，当条件为 true 时，v-if 才会编译并渲染其内容。切换 v-if 下的块儿内容实际上时销毁了其内部的所有元素，比如说处于 v-if 下的组件实际上在切换状态时会被销毁并重新生成，因此，切换一个较大 v-if 块儿时会比 v-show 消耗的性能多。

一句话概括：

> `v-show`无论是否为TRUE都会对元素进行渲染，设为false的时候只是在元素上添加了`display:none`，`v-if`是当为TRUE的时候才会进行一个渲染

## 父子组件生成周期的顺序

参考之前掘金上自己写的文章即可：https://juejin.cn/post/7071630284155781133

还有官方文档的生命周期钩子：https://cn.vuejs.org/guide/essentials/lifecycle.html

## vue3 ref 和 reactive 有什么区别？为什么有ref 的存在

参考这篇文章即可：https://juejin.cn/post/7192994086255591480

> `ref` 的存在是为了给开发者更好的控制组件内部的数据，使得更新数据的逻辑更清晰，并且可以方便地使用原生 DOM API 操作 DOM 元素。

## vuex整体数据流程？

看这篇文章就可以了：https://juejin.cn/post/6928468842377117709

## 组件通信？

看这篇文章就可以了：https://juejin.cn/post/7062740057018335245

## VDOM与diff算法？

看自己写的这篇文章：https://juejin.cn/post/7182374239582814266

## template 如何编译，编译过程怎么样？

参考这篇：https://juejin.cn/post/7116296421816418311

## data 为什么是一个函数

直白点应该就是:多个组件复用时，每次调用data函数的时候都会return一个新的对象，它们的内存地址都是不一样的，这样就不会相互影响

## Object.defineProperty 有什么缺陷？为什么proxy 更有优势

参考这篇文章：https://juejin.cn/post/7069397770766909476

## Template 比 jsx 有哪些优势？有哪些优化

知乎这篇文章分析的很不错：https://www.zhihu.com/question/436260027/answer/1647182157

而我选择tsx来实现自己组件库的原因也有这点：

> 是组件库代码比业务代码具有**更强的动态性**，使用 JSX 可以很灵活地控制动态 DOM 片段，使用tsx在保证灵活性的同时也为组件库的后期维护提供了很大的帮助

## nexTick
`nextTick`API常用于在修改数据，需要获取到最新的DOM的时候可以使用，比如：从服务器拉数据渲染后希望拿到dom 的高度 在nextTick 里获取，而nextTick就Vue进行**异步渲染**的关键之所在了
会在DOM渲染完毕之后执行这个异步函数(也就是在箭头函数里面的内容)
![image.png](https://ldbbs.ldmnq.com/bbs/topic/attachment/2023-2/f4aeb129-1f10-44a8-88d3-6ab643851215.png)
vue在做异步渲染的时候，是**批量进行**的渲染，不是每次数据发生了更新就马上进行渲染(同步渲染就会导致这样的结果)，这也就是为什么Vue是异步进行渲染的
掘金这篇文章：https://juejin.cn/post/7177681326861418556

## vue中的设计模式
- 双向绑定（Two-way binding）
- 状态管理模式（State Management Pattern）
- 单向数据流（One-way Data Flow）
- 组件化（Component-based Architecture）
- 访问者模式（Visitor Pattern）
- 观察者模式（Observer Pattern）
这些模式在 Vue 中经常用于组织代码，提高代码复用性和代码维护性，使得 Vue 项目的维护更加容易

## vue2和vue3的区别
- 响应式系统：Vue 3 对响应式系统进行了改进，并且更加快速和可靠。
- 代码量：Vue 3 相对于 Vue 2 更小，加载速度更快。
- Composition API：Vue 3 引入了 Composition API，为组件的状态和行为提供了一种更灵活的编写方式。
- 性能：Vue 3 对性能进行了改进，使用起来更加顺畅。
- 静态检查：Vue 3 引入了静态检查，使得开发者更容易发现代码问题。

## vite和webpack的区别

- 构建方式：Vite 是通过 ESModule 和 native ESM 进行构建，而 Webpack 是通过 CommonJS 进行构建。
- 构建速度：Vite 的构建速度比 Webpack 快很多，因为它更简单且更快。
- 设计思想：Vite 是基于原生 ESM 的，设计思想是简单且快速，而 Webpack 是一个功能强大的打包工具，有很多高级功能。
- 打包内容：Vite 只会打包必要的内容，不会打包无关的内容，以此来节省构建时间。而 Webpack 可以打包所有内容。

总的来说，Vite 更适合于快速构建小型项目，而 Webpack 则更适合于大型项目或者需要使用复杂插件的项目。

