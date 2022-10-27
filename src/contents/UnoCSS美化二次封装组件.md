---
title: UnoCSS美化二次封装组件
author: isolcat
datetime: 2022-10-27T04:06:31Z
slug: my-recent-2
featured: true
draft: false
tags:
  - UnoCSS
  - element plus
description:
  UnoCSS作为原子化CSS的新秀，如果和element plus组件库发生碰撞会产生怎样的火花呢
---

# 封装element plus组件并用UnoCSS美化

## 前言

在日常的开发中，为了避免重复造轮子，浪费开发的时间，我们经常会使用到第三方组件库，如`element plus`、`vant-ui`等知名组件库，而在一些开发中往往为了项目的整体美观性，我们都不会直接拿着第三方组件库就直接拿来使用，会对其进行修改，使其更加贴合项目的整体UI风格，这个时候我们就可以将第三方组件库进行抽离，封装成`公共组件库`，而这种对第三方组件库进行封装的操作就被称为`二次封装`。

### 二次封装的好处

- 更遵守代码的简介之道✌️
- 便于项目后期的维护🤪
- 组件复用性更强🪄

## Unocss介绍

既然要对原第三方组件库进行封装，就难免对其`样式`进行修改，修改css往往是最让人头疼的(起码对我来说是这样的🥹)，这时候我看见了`antfu`老师的[重新构想原子化CSS](https://antfu.me/posts/reimagine-atomic-css-zh),强烈建议先将这篇文章阅读后再看下去，会让你对`原子化CSS`有着更深的认识,这里用大神的话来概括一下就是:

> 原子化 CSS 是一种 CSS 的架构方式，它倾向于小巧且用途单一的 class，并且会以视觉效果进行命名。

而Unocss便是antfu老师做出的`具有高性能且极具灵活性的即时原子化 CSS 引擎。`至于为什么是引擎而不是一个CSS框架，是因为Unocss并**没有提供任何的核心工具类**，所有功能都可以通过`预设和内联配置`来提供

### Unocss的优势

- 灵活性(属性化模式、上万个`纯CSS图标`、无需担心样式冲突)🪄
- 样式复用性强🐼
- 不用想类名！(这点对于起名困难的人来说帮助太大了)🤣

既然`二次封装`和`Unocss`都能够大大提高开发效率，让大家心情愉悦，那么接下来我们便试一试让这两件事情合在一起的样子，这里便拿`element plus`的`loading加载组件`进行简单的二次封装，再用`Unocss`来进行美化

## 二次封装的核心

这里是使用的`vue3`的组件封装方法，与`vue2`的封装方法还是有些许区别的，关于`vue2`的封装方法请看[红尘老师的文章](https://juejin.cn/post/6943534547501858824#heading-4)

### $attrs

> 一个包含了组件所有透传 attributes 的对象。

这是vue官方对`$attrs`下的定义，是指由父组件传入，且没有被子组件声明为 props 或是组件自定义事件的 attributes 和事件处理函数。比如在组件当中我们用`<div>`标签嵌套了一个`<button>`的时候，想要让`class`或者`v-on`的监听器这类的透传attribute能直接应用在内部的`<button>`上，这时候我们就可以采用`v-bind="$attrs"`来实现

### v-on监听器继承

`vue3`当中直接删除了2里的`$listeners`事件监听器，现在直接将其功能融合到了`$attrs`中。例如写一个点击事件，在组件封装中，我们原子组件的点击，仍会触发父组件的onclick事件

```vue
  <!-- 子组件 -->
<button>click me</button>
  <!-- 父组件 -->
<MyButton @click="onClick" />
```

## 组件的封装

### 项目初始化

命令行输入:

```shell
pnpm create vite element-plus-unocss --template vue
```

使用`vite+pnpm`快速初始化项目

```shell
cd element-plus-unocss
pnpm i
pnpm run dev
```

成功运行后，项目的初始化便完成了

### 引入组件库

我们要封装的组件是`element plus`，故在此引入：

```shell
pnpm install element-plus
```

这里我们还是按照官方推荐的[自动导入](https://element-plus.org/zh-CN/guide/quickstart.html#%E6%8C%89%E9%9C%80%E5%AF%BC%E5%85%A5)，这里就不多赘述了，直接附上了官网的链接，点击进行配置即可，接下来才是重点

### 二次封装

这里我们选择`loading`加载组件进行演示，在`components`中添加我们要封装的子组件`loading.vue`，直接复制官方的[链接例子](https://element-plus.org/zh-CN/component/loading.html)皆可(可以适当做一点删减)：

```vue
<template>
  <el-button
    v-loading.fullscreen.lock="fullscreenLoading"
    type="primary"
    @click="openFullScreen1"
  >
    Click me
  </el-button>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import { ElLoading } from 'element-plus'

const fullscreenLoading = ref(false)
const openFullScreen1 = () => {
  fullscreenLoading.value = true
  setTimeout(() => {
    fullscreenLoading.value = false
  }, 2000)
}
</script>
```

这时候我们再创建`Myloading.vue`组件，再在其中进行引入，并对其代码进行修改:

```vue
<template>
  <Loading
    v-bind="$attrs"
    element-loading-text="正在努力加载~"
    element-loading-background="rgba(122, 122, 122, 0.8)"
  />
</template>

<script setup>
import Loading from "./loading.vue";
</script>
<style>
.el-loading-mask .el-loading-spinner .el-loading-text {
  font-size: 20px;
}
</style>
```

运行结果如下：

 <img src="https://pic5.58cdn.com.cn/nowater/webim/big/n_v215c64d070b3347c0901f0b239ea1b8f1.png" alt="image.png" style="zoom: 67%;" />

这时候便代表着我们组件的`二次封装`成功了

## UnoCSS美化组件

这个时候我们发现，似乎这个`click me`看上去死气沉沉的，完全没有让人点击的欲望，那么有什么方法可以让这个按钮给人一种呼之欲出，让人很想点击呢？这时候我们就可以请出我们的重量级人物`UnoCSS`了

### 安装并引入UnoCss

```shell
pnpm i -D unocss
```

对`vite.config.js`进行配置：

```js
import Unocss from 'unocss/vite'

export default {
  plugins: [
    Unocss({ /* options */ }),
  ],
}
```

并将`UnoCSS`引入到`main.js`中：`import 'uno.css'`

### 配置预设

配置预设是我认为`UnoCSS`的一个重要优势，只需要几个简单的预设，就能在几分钟搭建出属于自己的`自定义框架`，，`属性化`的特点就是antfu老师的`Windi CSS`的特点之一了，在`UnoCSS`中也将这一特点给保留了下来。这里我们就安装`preset-attributify`和`unocss/preset-uno`:

```shell
pnpm i -D @unocss/preset-attributify
pnpm i -D @unocss/preset-uno
```

修改后的`vite.config.js`：

```js
import { defineConfig } from 'vite'
import AutoImport from 'unplugin-auto-import/vite'
import Components from 'unplugin-vue-components/vite'
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers'
import Unocss from '@unocss/vite'
import presetUno from '@unocss/preset-uno'
import presetAttributify from '@unocss/preset-attributify'

import vue from '@vitejs/plugin-vue'
export default defineConfig({
    plugins: [vue(), AutoImport({
            resolvers: [ElementPlusResolver()],
        }),
        Components({
            resolvers: [ElementPlusResolver()],
        }),
        Unocss({
            presets: [presetUno(), presetAttributify()]
        })
    ]
})
```

此时我们便有了一个`默认预设+属性模式`的自定义框架了，之后写了一长串的css类后，就会直接按照`属性模式`进行分组，代码更加整洁，可读性大大加强：

 ![官方的实例](https://pic8.58cdn.com.cn/nowater/webim/big/n_v2892f4d98847a4e1f90d1307c136f9eff.png)

### 修改组件的样式

我们为了让按钮看上去更有点击的欲望，我们可以尝试给`click me`加上一个跳跃的动画，这时候我们打开`UnoCSS`的`playground`，发现官方的演示里面就有着反复跳跃的样式，我们直接cv一下，修改我们的子组件:

```vue
<div  class="
      text-5xl
      fw300
      animate-bounce-alt
      animate-count-infinite
      animate-duration-1s"
    >
      click me
</div>
```

这时候我们感觉默认的按钮字体颜色似乎有些太深了，这时候我们再在父组件进行修改:

```vue
<Loading
    element-loading-text="正在努力加载~"
    v-bind="$attrs"
    element-loading-background="rgba(122, 122, 122, 0.8)"
    class="text-lg 
           fw300 
           m2 
           op70"
  />
```

> 这里如果我们想要知道cv的到底是什么内容，我们可以下载一个UnoCSS插件，直接在vscode中搜索即可，安装后再放在上面就会显示出这个类源码，便于后续的开发

好了，让我们来看看美化后的按钮的模样：

 <img src="https://pic4.58cdn.com.cn/nowater/webim/big/n_v204070dee2d374ff493745496d195dc8a.gif" alt="GIF 2022-10-22 23-10-45.gif" style="zoom: 150%;" />

不停的跳动，是不是让人更有想要点击的欲望呢😂

## 结语

`UnoCSS`作为原子化CSS的新秀，让人眼前一亮，它吸取了前辈`taiwind CSS`的优势，融合了自己的`windiCSS`的特色，让它出奇的好用，尽管现在的它仍在测试版，但还是很推荐大家去尝试一下的，绝对会让你有种**什么？还可以这样**的感觉，还可以用`UnoCSS`来搭建一个属于你自己的组件库，这里贴一个自己的组件库项目，就是对UnoCSS的一次尝试:https://github.com/isolcat/CatIsol-UI,组件库`预览地址`：`https://cat-isol-ui.vercel.app/`(顺带一提，这个文档是`vitepress`搭建的，也是antfu老师的作品)

结合本文所说的组件二次封装，相信你们也可以做出一个很有意思的项目的，期待有更多让人眼前一亮的项目出现，能给我这个菜狗开开眼😽

