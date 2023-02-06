---
title: defer和async
datetime: 2023-2-6T16:16:11Z
featured: false
draft: false
tags:
  - 面试题
description: defer和async的区别
---
# defer和async

## defer标签和async标签有什么区别？

`defer` 和 `async` 是两个用于**加载脚本**的不同属性。

`defer`标签告诉浏览器该脚本不会影响页面中其他元素的构造，并且按照它在 HTML 文档中出现的顺序加载。在页面渲染完成之前，`defer`脚本都将执行。

`async`标签告诉浏览器该脚本可以异步加载，不必等待它完成即可继续渲染页面。换句话说，就是浏览器会在后台下载这个脚本，但不保证它在其他脚本执行完成之前就完成。

因此，如果需要在页面完全渲染之前执行的脚本，应该使用 defer；如果需要异步加载脚本，不会影响页面渲染，可以使用 async

代码实例：
```js
<script src="script1.js" defer></script>
<script src="script2.js" defer></script>

<!--脚本将按照它们在HTML中出现的顺序执行，但只有在页面完成解析后才会执行 -->

<script src="script1.js" async></script>
<script src="script2.js" async></script>

<!-- 脚本一经下载就会被执行，其执行顺序可能不被保证 -->
```