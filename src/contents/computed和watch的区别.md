---
title: 说说computed和watch的区别
datetime: 2023-2-7T10:55:11Z
featured: false
draft: false
tags:
  - 面试题
  - vue
description: 说说computed和watch的区别
---

# 说说computed和watch的区别

Vue.js 中 computed 和 watch 是两种不同的数据绑定机制。

`computed` 是一种计算属性，用于依赖于其他数据的属性，并在数据更改时自动更新。它的值是缓存的，只有在它依赖的数据发生更改时才会重新计算。

`watch` 是一种监听器，用于在数据发生变化时执行回调函数。它可以用来执行一些特定的操作，如在数据变化后发送 HTTP 请求，或更新其他数据。

通常情况下，当我们需要从一个值计算出另一个值时，我们可以使用 computed。如果我们需要在数据变化时执行一些特定的操作，则可以使用 watch。

>Computed 和 Watch 功能的不同：Computed 是用于计算和缓存结果的，而 Watch 是监听数据变化并作出响应的。
>Computed 和 Watch 用法的不同：Computed 需要给出计算规则，并在需要时触发计算；而 Watch 则需要监听数据变化，并在数据变化时触发回调。
>Computed 为**一对一**，Watch**为一对多**：一个 Computed 变量只能有一个计算规则，而一个 Watch 变量可以有多个监听器。

例如，我们可以使用 computed 来计算一个人的年龄，并在出生日期更改时自动更新：

```javascript
computed: {
  age() {
    return new Date().getFullYear() - this.birthYear;
  }
}
```

我们也可以使用 watch 来监听一个人的姓名，并在姓名更改时执行回调函数：

```javascript
watch: {
  name(newName, oldName) {
    console.log(`姓名从 ${oldName} 更改为 ${newName}`);
  }
}
```