---
title: 轻松了解VDOM与diff算法那些不得不说的事
author: isolcat
datetime: 2022-12-25T20:58:31Z
featured: true
draft: false
tags:
  - 面试题
  - vue
description: 轻松掌握VDOM和diff算法那些事
---


# 轻松了解VDOM与diff算法那些不得不说的事

## 前言

事实上，在很多的面试时，经常会遇见面试官询问虚拟DOM和diff算法之间的事情，而这也几乎成为了vue面试者必需掌握的`八股文`之一，接下来我将使用尽量通俗的语言去描述`VDOM`和`diff算法`之间的关系，希望能对你有所帮助

## 什么是VDOM？

在没有`vue` `react`等前端框架的时代，我们如果想操作一个元素，往往是**直接进行DOM**元素的操作，以此来达到一个视图的更新的效果。这时候想要达到视图更新的效果便是**操作DOM->视图更新**。而现在有各个框架的存在时，我们想要让数据进行更新，改变视图的过程也就发生了改变，变成了**数据改变->虚拟DOM计算变更->操作真实DOM->视图更新**，而多出来的虚拟DOM计算变更便是vue框架所需要做的事情

> 虚拟 DOM (Virtual DOM，简称 VDOM) 是一种编程概念，意为将目标所需的 UI 通过数据结构“虚拟”地表示出来，保存在内存中，然后将真实的 DOM 与之保持同步。这个概念是由 [React](https://reactjs.org/) 率先开拓，随后在许多不同的框架中都有不同的实现，当然也包括 Vue。

根据vue官方对虚拟DOM的描述我们不难知道：虚拟DOM的出现就是为了**助开发人员更有效地构建和更新用户界面**。在之前的无框架时代，我们如果想要修改一个真实DOM，在数据非常多，或者需要修改的地方很多的时候，我们需要反复的进行对DOM的操作，这一过程无疑是大大增加了开发的难度，降低了开发的效率，而引入了虚拟DOM后，带来了以下`好处`：

- 使用虚拟 DOM 可以让开发者专注于组件的逻辑，而不用操心 DOM 操作的细节。虚拟 DOM 可以自动管理 DOM 元素的创建、更新和销毁，让开发者更专注于业务逻辑。
- 极大地减少对真实DOM的修改操作，从而**提高渲染性能**
- 使用虚拟 DOM 可以使代码**结构更加清晰**，更易维护。虚拟 DOM 可以把 DOM 操作的逻辑和组件的逻辑分离开来，使代码更易于阅读和维护。

但事实上，我们不应该直接说**虚拟DOM比真实DOM快**(*比如无虚拟DOM的Svelte性能反而更加出众，尤大也在今年的vueconf里说可能会推出`无虚拟DOM`版本的vue*)，我们应该说**虚拟DOM算法操作真实DOM，性能高于直接操作真实DOM**,而`虚拟DOM算法=VDOM+diff`

所以，什么又是diff算法？

## diff算法？

既然我们已经知道了什么是`VDOM`，那我们在实际的开发中，vue是怎么发现我们对DOM进行了修改的呢？总不会我们生成虚拟DOM后它就自己进行比对了吧，而这就是vue采用的一个内部封装的**算法**————**diff算法**，而它的最大作用，便是**检测虚拟DOM和真实DOM的区别**

当我们修改了原DOM的元素，便会生成一个新的VDOM，这时候，diff算法便开始将**新旧VDOM**进行对比，当发现后开始有**针对性**的去更新真实DOM：

<img src="https://ldbbs.ldmnq.com/bbs/topic/attachment/2022-12/56507e8a-8309-4cd9-9f0e-e856b639829c.png" alt="image.png" style="zoom: 67%;" />

## diff算法原理？

diff算法在进行比对的时候不会像dfs那样(*虽然diff算法严格上说也算是dfs*)进行搜索，而是进行了大量的优化的算法，它并不会一上来就疯狂的进行搜索然后比对，而是会进行**分层比较**，或者说是它**只会在同层级进行比较，不会跨层级进行比对**

```html
<div>
    <button>
        我是按钮
    </button>
</div>


<div>
    <p>
        我不是按钮哦
    </p>
</div>
```

按照上述的代码生成的VDOM进行比对，它只会将同一层的div标签进行比对，或者是将同为第二层的button和p标签进行比对，而不会将p标签和div标签进行比对，用图片描述的话大概就是这样子：<img src="https://ldbbs.ldmnq.com/bbs/topic/attachment/2022-12/2e60ebe0-9b18-4e6b-8267-8a185d558aa7.png" alt="image.png" style="zoom:50%;" />

### diff对比流程图

为了方便理解还是用一张大图来表示diff算法的大致流程吧：

 <img src="https://ldbbs.ldmnq.com/bbs/topic/attachment/2022-12/c3223bc6-5b5f-4bc0-81b1-1ccfac99b77b.png" alt="diff.png" style="zoom: 80%;" />

带着这幅超丑的(😭)流程图我们接下来对核心原理代码进行分析：

### patch

patch函数将两个VDOM(分别为oldVnode与newVnode)进行比对，由此来判断是否进行接下来的判断

```js
//代码部分参考了前辈们的文章

function patch(oldVnode, newVnode){
  // 先对类型进行比较，若为同类型才会进行后续的比对
  if(sameVnode(oldVnode, newVnode)){
    patchVnode(oldVnode, newVnode)
  } else {
    // 旧虚拟节点的真实DOM节点
    const oldEl = oldVnode.el 
     // 获取父节点
    const parentEle = api.parentNode(oldEl)
    // 创建新虚拟节点对应的真实DOM节点
    createEle(newVnode) 
    if (parentEle !== null) {
        // 将新元素添加进父元素
      api.insertBefore(parentEle, vnode.el, api.nextSibling(oEl)) 
        // 移除以前的旧元素节点
      api.removeChild(parentEle, oldVnode.el)  
      // 将旧节点设置null，释放内存
      oldVnode = null
    }
  }
  return newVnode
}
```

### sameVnode

这时候我们可以注意到，在进行类型判断的时候，我们用了一个`sameVnode`来实现新旧VDOM类型的比较，接下来我们来查看其源代码，让我们更深入的了解该函数的作用：

```js
function sameVnode(oldVnode, newVnode) {
  return (
    oldVnode.key === newVnode.key && // 判断新旧vnode的key值是否一致
    oldVnode.tagName === newVnode.tagName && // 判断标签名是否一致
    oldVnode.isComment === newVnode.isComment && // 判断是否为注释节点
    isDef(oldVnode.data) === isDef(newVnode.data) && // 判断是否都定义了data(包含一些具体信息，例如onClick , style)
    sameInputType(oldVnode, newVnode) //如果标签是<input>的时候，type必须相同
  )
}
```

### patchVnode

从之前的流程图我们不难判断，`pathcVnode`方法是**最重要**的函数，它将能够进行比对的新旧节点进行了**分类处理**，接下来是核心代码：

```js
function patchVnode(oldVnode, newVnode) {
  // 获取真实DOM对象
  const el = newVnode.el = oldVnode.el
  // 获取新旧VDOM的子节点数组
  const oldCh = oldVnode.children, newCh = newVnode.children
  // 1.如果新旧VDOM是同一个对象，则终止
  if (oldVnode === newVnode) return
  // 2.如果新旧VDOM是文本节点，且文本不一样
  if (oldVnode.text !== null && newVnode.text !== null && oldVnode.text !== newVnode.text) {
    // 则直接将真实DOM中文本更新为新VDOM的文本
    api.setTextContent(el, newVnode.text)
  } else {
    // 否则

    if (oldCh && newCh && oldCh !== newCh) {
      // 3.新旧VDOM都有子节点，且子节点不一样

      // 对比子节点，并更新
      updateChildren(el, oldCh, newCh)
    } else if (newCh) {
      // 4.新VDOM有子节点，旧VDOM没有

      // 创建新虚拟节点的子节点，并更新到真实DOM上去
      createEle(newVnode)
    } else if (oldCh) {
      // 5.旧虚拟节点有子节点，新虚拟节点没有

      //直接删除真实DOM里对应的子节点
      api.removeChild(el)
    }
  }
}
```

而在这里又引入了一个新的函数————`updateChildren`，让我们来分析分析这个函数吧

### updateChildren

先附上核心代码,再慢慢来解释：

```js
function updateChildren(parentElm, oldCh, newCh) {
  let oldStartIdx = 0, newStartIdx = 0
  let oldEndIdx = oldCh.length - 1
  let oldStartVnode = oldCh[0]
  let oldEndVnode = oldCh[oldEndIdx]
  let newEndIdx = newCh.length - 1
  let newStartVnode = newCh[0]
  let newEndVnode = newCh[newEndIdx]
  let oldKeyToIdx
  let idxInOld
  let elmToMove
  let before
  while (oldStartIdx <= oldEndIdx && newStartIdx <= newEndIdx) {
    if (oldStartVnode == null) {
      oldStartVnode = oldCh[++oldStartIdx]
    } else if (oldEndVnode == null) {
      oldEndVnode = oldCh[--oldEndIdx]
    } else if (newStartVnode == null) {
      newStartVnode = newCh[++newStartIdx]
    } else if (newEndVnode == null) {
      newEndVnode = newCh[--newEndIdx]
    } else if (sameVnode(oldStartVnode, newStartVnode)) {
      patchVnode(oldStartVnode, newStartVnode)
      oldStartVnode = oldCh[++oldStartIdx]
      newStartVnode = newCh[++newStartIdx]
    } else if (sameVnode(oldEndVnode, newEndVnode)) {
      patchVnode(oldEndVnode, newEndVnode)
      oldEndVnode = oldCh[--oldEndIdx]
      newEndVnode = newCh[--newEndIdx]
    } else if (sameVnode(oldStartVnode, newEndVnode)) {
      patchVnode(oldStartVnode, newEndVnode)
      api.insertBefore(parentElm, oldStartVnode.el, api.nextSibling(oldEndVnode.el))
      oldStartVnode = oldCh[++oldStartIdx]
      newEndVnode = newCh[--newEndIdx]
    } else if (sameVnode(oldEndVnode, newStartVnode)) {
      patchVnode(oldEndVnode, newStartVnode)
      api.insertBefore(parentElm, oldEndVnode.el, oldStartVnode.el)
      oldEndVnode = oldCh[--oldEndIdx]
      newStartVnode = newCh[++newStartIdx]
    } else {
      // 使用key时的比较
      if (oldKeyToIdx === undefined) {
        oldKeyToIdx = createKeyToOldIdx(oldCh, oldStartIdx, oldEndIdx) // 有key生成index表
      }
      idxInOld = oldKeyToIdx[newStartVnode.key]
      if (!idxInOld) {
        api.insertBefore(parentElm, createEle(newStartVnode).el, oldStartVnode.el)
        newStartVnode = newCh[++newStartIdx]
      }
      else {
        elmToMove = oldCh[idxInOld]
        if (elmToMove.sel !== newStartVnode.sel) {
          api.insertBefore(parentElm, createEle(newStartVnode).el, oldStartVnode.el)
        } else {
          patchVnode(elmToMove, newStartVnode)
          oldCh[idxInOld] = null
          api.insertBefore(parentElm, elmToMove.el, oldStartVnode.el)
        }
        newStartVnode = newCh[++newStartIdx]
      }
    }
  }
  if (oldStartIdx > oldEndIdx) {
    before = newCh[newEndIdx + 1] == null ? null : newCh[newEndIdx + 1].el
    addVnodes(parentElm, before, newCh, newStartIdx, newEndIdx)
  } else if (newStartIdx > newEndIdx) {
    removeVnodes(parentElm, oldCh, oldStartIdx, oldEndIdx)
  }
}
```

是不是看上去头都大了？感觉代码量超多的诶？但其实我们拆开来仔细分析的话，也没有那么难

新旧子节点们均有首尾两个指针，首尾两个指针开始进行比较：

> 1、`oldS 和 newS `使用`sameVnode方法`进行比较，`sameVnode(oldS, newS)`
>
> 2、`oldS 和 newE `使用`sameVnode方法`进行比较，`sameVnode(oldS, newE)`
>
> 3、`oldE 和 newS `使用`sameVnode方法`进行比较，`sameVnode(oldE, newS)`
>
> 4、`oldE 和 newE `使用`sameVnode方法`进行比较，`sameVnode(oldE, newE)`
>
> 5、如果以上逻辑都匹配不到，再把所有旧子节点的 `key` 做一个映射到旧节点下标的 `key -> index` 表，然后用新 `vnode` 的 `key` 去找出在旧节点中可以复用的位置。

而这些的所有操作只有一个核心便是：**判断子节点的差异**

一旦执行了上述情况的任意一种便会结束循环

这里可能我讲的不是很详细，大家可以看一下林老师的文章里关于[updateChildren的部分](https://juejin.cn/post/6994959998283907102)（实在无法做到林老师画的那么生动😢）



## 结语

总而言之，Vue中的VDOM是一个虚拟的、用于描述真实DOM的数据结构，而diff算法则是用于比较两个VDOM树之间的差异的算法，从而可以确定最少的DOM操作来更新真实的DOM树。因此，VDOM和diff算法是协同工作的，VDOM用于描述真实DOM，diff算法用于比较VDOM树之间的差异并确定如何更新真实DOM。希望这篇文章能够对你有所帮助

> 参考文章：
>
> [15张图，20分钟吃透Diff算法核心原理，我说的！！！](https://juejin.cn/post/6994959998283907102)
>
> [详解vue的diff算法](https://juejin.cn/post/6844903607913938951)

