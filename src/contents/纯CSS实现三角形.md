---
author: isolcat
datetime: 2022-11-8T12:13:24Z
title: 纯CSS实现三角形
featured: false
draft: false
tags:
  - 面试题
ogImage: ""
description: 纯CSS实现一个三角形
---

# CSS实现三角形

使用**纯CSS来实现三角形**，这里就先以`等腰三角形`来为例：

主要利用的核心是属性是`border`

> 1.画一个带边框的普通正方形

```html
<div class="div"></div>
    <style>
      .div {
        width: 100px;
        height: 100px;
        border: 1px solid #66ccff;
      }
    </style>
```

> 2.将border的数值`调大`

```css
 .div {
        width: 100px;
        height: 100px;
        border: 30px solid #66ccff;
      }
```

> 3.将border的颜色设置为`不一样的`

![image.png](https://ldbbs.ldmnq.com/bbs/topic/attachment/2023-1/da9e0022-8d98-4735-bf99-9928717eb59b.png)

> 4.将div的宽高都设置为`0`

![image.png](https://ldbbs.ldmnq.com/bbs/topic/attachment/2023-1/c239e7ed-c5e7-48f3-816e-1c56242f84b6.png)

可以很清楚的看见我们这时候得到了4个三角形，现在需要做的就是将三角形抠出来

> 5.想要保留哪个三角形，就`保留三角形的颜色`，将其他都设置为`透明`

![image.png](https://ldbbs.ldmnq.com/bbs/topic/attachment/2023-1/e448d25b-cb54-4644-a60e-c86d9df80173.png)
这样子是不是看上去仿佛实现了三角形了呢？但实际上如果我们给三角形加上一个背景，就会发现div的占位空间并没有发生任何的改变

> 6.所以我们将解决一下div占位的问题，将三角形的对边的`border`给去掉

![image.png](https://ldbbs.ldmnq.com/bbs/topic/attachment/2023-1/8ace70a6-6edd-46ba-a0f7-4f25658cc45b.png)

这样一个三角形便实现了
具体代码：https://stackblitz.com/edit/web-platform-wqhwfy?file=index.html

