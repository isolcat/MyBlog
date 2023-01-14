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

 <img src="https://pic8.58cdn.com.cn/nowater/webim/big/n_v2ca5f1a958a6e444898ca0fe3541f15f5.png" alt="image.png" style="zoom:67%;" />

> 4.将div的宽高都设置为`0`

 <img src="https://pic6.58cdn.com.cn/nowater/webim/big/n_v28c6d2dcb9259481da7303f5d72fd3844.png" alt="image.png" style="zoom:67%;" />

可以很清楚的看见我们这时候得到了4个三角形，现在需要做的就是将三角形抠出来

> 5.想要保留哪个三角形，就`保留三角形的颜色`，将其他都设置为`透明`

 <img src="https://pic8.58cdn.com.cn/nowater/webim/big/n_v29b16c57c87464b2ea85488582215bb94.png" alt="image.png" style="zoom:67%;" />

这样子是不是看上去仿佛实现了三角形了呢？但实际上如果我们给三角形加上一个背景，就会发现div的占位空间并没有发生任何的改变

> 6.所以我们将解决一下div占位的问题，将三角形的对边的`border`给去掉

 <img src="https://pic2.58cdn.com.cn/nowater/webim/big/n_v2b395eb7a6a734e45b3b0be9175c4a832.png" alt="image.png" style="zoom:67%;" />

这样一个三角形便实现了

