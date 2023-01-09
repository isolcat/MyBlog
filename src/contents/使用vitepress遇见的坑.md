---
title: 使用vitepress踩的坑
author: isolcat
datetime: 2023-1-9T20:27:11Z
featured: true
draft: false
tags:
  - vue
description: 踩坑的小记录...
---

# 使用vitepress部署组件库文档时所的踩坑

在自定义组件的时候，之前的`button` `input`组件都没有任何的问题，但是在`link`组件的时候出现了问题，我自定义的组件样式被覆盖：

 ![image.png](https://camo.githubusercontent.com/0fa419dc2b4f3080e48428653bb14db00fa789ad98a3ec3482c5d08a99523f53/68747470733a2f2f6c646262732e6c646d6e712e636f6d2f6262732f746f7069632f6174746163686d656e742f323032332d312f64646435396363382d373066302d343335372d613332352d3138373430636232353337332e706e67)

在翻阅文档的时候可以看见，`vp-doc`类为vitepress官方封装的类，他的作用便是去修饰所有的文档外观，由于vp-doc的优先级过高，所以将我的自定义组件的样式给覆盖了，这个时候我最开始是打算在link组件的源码上直接进行`!import`来提升优先级的，但仔细一想便知道这不行，组件是是拿来用的，本身的作用是更偏向于**毛坯房**的，我们应该将样式的选择权交给用户，所以这个方案就直接pass了

我又想到或许我可以直接查找到这个类，然后直接将其进行删除，就如同该代码所示：

 ![image.png](https://ldbbs.ldmnq.com/bbs/topic/attachment/2023-1/7cc4068f-5d1f-4997-b768-046b9e12f7f3.png)

结果就犯了第二个错误，让我的文档变成了这幅鬼样子：

 <img src="https://ldbbs.ldmnq.com/bbs/topic/attachment/2023-1/acaa705f-f75b-4697-a3d3-8a3609bbfe68.png" alt="image.png" style="zoom: 67%;" />

文档默认样式都被删除了😭

这个时候我只能去官方文档库查阅issues，看看有没有其他的人遇见了这个情况，果然看见了类似的提问：https://github.com/vuejs/vitepress/issues/199，当时的团队成员和这位大佬便一起探讨出来了一个方案，便是进行样式隔离：<img src="https://ldbbs.ldmnq.com/bbs/topic/attachment/2023-1/a47d904c-fe00-452b-9814-53726deac129.png" alt="image.png" style="zoom:67%;" />

于是我连忙翻阅vitepress文档，发现不知道什么时候多了个新的语法`raw`，有了这个语法便可以对自己写的组件进行样式隔离了！不用担心被其他的样式给覆盖，我连忙对自己的文档进行了修改(*当时发生了一点小插曲，vp-raw没有立刻生效，而是在我完全重启vscode才生效😂详细为：https://github.com/vuejs/vitepress/issues/1777*)

最终的呈现为：

 <img src="https://ldbbs.ldmnq.com/bbs/topic/attachment/2023-1/0d76acfb-f37f-4368-9355-2fd17c9565e6.png" alt="image.png" style="zoom:67%;" />