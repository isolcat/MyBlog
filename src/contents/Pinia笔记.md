---
author: isolcat
datetime: 2022-09-3T15:22:00Z
title: 十分钟快速入门Pinia
slug: 学习Pinia
featured: true
draft: false
tags:
  - pinia
  - docs
ogImage: ""
description:
  都什么年代了，还在用传统状态管理库？快来学习Pinia吧！
---

## 前言

<img src="https://dd-static.jd.com/ddimg/jfs/t1/120105/4/20625/22967/630ad494E5bab0409/e8309bf2aa9be1e4.png" alt="image.png" style="zoom:50%;" />

都什么年代了，还在用传统状态管理库？快来学习`Pinia`吧！

> Pinia这个名字的来源也很有意思，在西班牙语中，Pinia是菠萝这个词最相似的英语发音，而一个菠萝是由一组组单独的花，结合在一起的，创造了多个水果，与stores类似，每一个都是独立的，但最终都会有着联系

当我们点开`vuex`的github库的时候，会看见官方的提示**Pinia is now the new default**，而作为Vue的新一代的官方状态管理库，Pinia有着更多优势，解决了很多Vuex所留下的问题，在编写的时候，会更有逻辑性，接下来就让我们去试着了解一下吧！

## Pinia优点

- vue3 vue2均支持
- 抛弃了Mutation的操作，只有`state`、`getter`和`action`
- `Actions`支持同步和异步
- 支持使用插件扩展Pinia功能
- 不需要嵌套模块，更加符合Vue3的`Composition api`
- 支持typescript
- 代码更加的简洁

## 用Pinia方式创建一个store

我们先快速创建一个空项目，再安装`Pinia`:

```sh
npm install pinia
```

虽然Pinia支持vue2，但如果你使用的vue版本低于`Vue2.7`,还需独立安装composition api: `@vue/composition-api`*（这里建议直接升到Vue2.7，相较于Vue3来说，跨度不会太大，但对Vue的生态支持的更好）*

在`main.ts`中对Pinia进行引入：

```typescript
import { createApp } from 'vue'
import { createPinia } from 'pinia'

import App from './App.vue'


const app = createApp(App)

app.use(createPinia())

app.mount('#app')
```

接着，我们在`src/store`中创建`counter.ts`，并写下基础模板：

```typescript
import { defineStore } from "pinia";

export const mainStore = defineStore('main', {
  state: () => {
    return {
      helloWord: 'HelloWorld'
    }
  },
  getters: {

  },
  actions: {

  }
})
```

创建好仓库`mainStore(仓库名)`后，我们将在组件中进行使用

```vue
<template>
  <div class="">{{ store.helloWord }}</div>
</template>

<script lang="ts" setup>
import { mainStore } from "../store/counter";
const store = mainStore();
</script>

<style scoped></style>
```

当页面显示出`helloWorld`后，则说明`store`创建成功

## Pinia改变数据状态

我们在`counter.ts`中的`state`添加数据`count`：

```typescript
import { defineStore } from "pinia";

export const mainStore = defineStore('main', {
  state: () => {
    return {
      count: 0,
      helloWord: 'HelloWorld'
    }
  },
  getters: {

  },
  actions: {

  }
})
```

做一个拥有`点击事件`按钮：

```vue
<template>
  <div>
    <button @click="handleClick">修改数据状态</button>
  </div>
</template>

<script setup lang="ts">
import { mainStore } from "@/stores/counter";
const store = mainStore();
const handleClick = () => {
  store.count++;
};
</script>
```

将其引入`App.vue`后，对数据状态进行修改：

```vue
<template>
  <Click />
  <CountButton />
</template>

<script lang="ts" setup>
import Click from "./components/Click.vue";
import CountButton from "./components/CountButton.vue";
</script>
```

这时候便发现点击按钮后会改变`count`的值了

在实际开发的过程中，我们往往需要多次调用`store中的数据`，如果每次改变其值都需要`{{store.****}}`未免太麻烦了，我们对其进行解构：

```vue
<template>
  <div class="">{{ store.helloWord }}</div>
  <div class="">{{ store.count }}</div>
  <hr />
  <!-- 解构后可以直接省略掉store，减少了代码量 -->
  <div>{{ helloWord }}</div>
  <div>{{ count }}</div>
</template>

<script lang="ts" setup>
import { mainStore } from "../store/counter";
import { storeToRefs } from "pinia";
const store = mainStore();

// 进行解构
const { helloWord, count } = storeToRefs(store);
</script>

<style scoped></style>
```

注意，解构必须要用到`storeToRefs()`函数！



## PInia修改数据的四种方法

- 第一种方法：

```js
const handleClick = () => {
  store.count++;
};
```

- 第二种方式`$patch`

```js
const handleClickPatch=()=>{
	store.$patch({
		count:store.count+2
	})
}
```

第二种方法写起来虽然没有第一种简单，但更加适合于`多个数据`的改变

- 第三种方式 `$patch 传递函数`

```js
const handleClickMethod = () => {
  // 这里的state就是指代的仓库中的state
  store.$patch((state) => {
    state.count++;
    state.helloWord = state.helloWord === "jspang" ? "Hello World" : "jspang";
  });
};
```

- 第四种方式 `action`

当业务逻辑很复杂的时候，就将方法写在`store`中的`action`里

```typescript
actions: {
    changeState() {
      this.count++
      this.helloWord = 'jspang'
    }
  }
```



## Getters

与vuex中的getters是差不多的，相当于vue中的计算方法。但当我们查看vuex文档，会发现这个提示：

> 注意
>
> 从 Vue 3.0 开始，getter 的结果不再像计算属性一样会被缓存起来。这是一个已知的问题，将会在 3.1 版本中修复。详情请看 [PR #1878](https://github.com/vuejs/vuex/pull/1883)。

直到现在，这个提示仍然存在，这也是我更推荐使用`Pinia`的原因之一。

Pinia中的`Getters`本身内部是可以进行缓存的，用代码来验证：

```typescript
getters: {
    phoneHidden(state) {
      // 正则表达式
      console.log('getters被调用');
      return state.phone.toString().replace(/^(\d{3})\d{4}(\d{4})$/, '$1****$2')
    }
  },
```

我们在组件中两次调用phone(数据)，点开控制台进行查看

<img src="https://pic.rmb.bdstatic.com/bjh/e0d7a7c0025b7d4ea68c989bf0428a96.png" alt="image.png" style="zoom:50%;" />

是只会出现一次的，证实了其具有缓存功能，这对于`性能优化`是有好处的



## Pinia中的stores互相调用

在实际开发中，往往不会只用到一个仓库的，仓库和仓库之间通常会有一个调度，这里我们再创建一个仓库：

```typescript
import { defineStore } from "pinia";

export const nameStore = defineStore('name', {
    state: () => {
        return {
            list: ['小红', '小美', '胖丫']
        }
    }
})
```

接着我们在`counter.ts`中导入,`import {jspangStore} from './jspang'`，在`action`中进行调用：

```typescript
 actions: {
    getList() {
      console.log(nameStore().list);
    }
  }
```

查看控制台，发现已经成功调用了另一个仓库的`state`，成功做到了`store`之间的相互调用



## 支持VueDevtools

Pinia虽然作为一个新人，但已经全面支持VueDevtools了，这对于我们在实际项目开发中的调试有着很大的帮助，值得一提的是，在打开VueDevtools时，我们可以看见一个很俏皮的菠萝的logo![image.png](https://dd-static.jd.com/ddimg/jfs/t1/136537/39/25083/4395/6312ca98Eca0adada/3813a9c349091b2d.png)

在开发时候的心情都更好了哈哈哈

> 注意：如果你使用的是Pinia v2，请升级你的`Vue Devtools`到v6版本



## Pinia实战：修改头像

说了这么多，不如来实战一下，在实际的项目中运用一下`pinia`。开发背景：在我们制作网页的时候，往往会涉及到`注册功能`，用户头像在`登录前`和`登录后`是不一样的。如果只写一个点击事件来修改头像的话，一旦刷新或是进行了页面的跳转便会恢复原样，状态无法得到保存，这时候我们就可以请出我们的pinia了

> 注意，为了pinia中的数据持久化存储（存到localstorage或sessionstorage中），我们还需要安装一个插件：[pinia-plugin-persist](https://github.com/Seb-L/pinia-plugin-persist)，他能够让我们的操作更加方便，这里便不展开赘述了，详细可以参考[官方文档](https://seb-l.github.io/pinia-plugin-persist/)

### 1.创建仓库

第一步还是先`创建仓库`(*这里默认你已经配置好了相关环境*)，创建`store/user.ts`，具体代码如下：

```typescript
import { defineStore } from 'pinia';

export const mainStore = defineStore('main', {
    state: () => {
        return {
            login: require('../assets/images/login.png')
        }
    },
    // 开启持久化
    persist: {
        enabled: true,
        strategies: [
            { storage: localStorage, paths: ['login'] }
        ],
    },
    getters: {

    },
    actions: {
       
    }
})
```



成功创建后，我们便将`用户头像`存储在了`store`中，接下来便是将其在组件中使用

### 2.组件中调用

```vue
<!-- 头像 -->
<a class="face" href="#/login">
   <img :src="store.login" alt="" />
</a>
```

打开浏览器查看:![image.png](https://dd-static.jd.com/ddimg/jfs/t1/140119/39/29764/2936/6311fe87E39cedb42/a5903ae2a475000b.png)

成功调用！接下来便是最重要的一步，实现登录后头像能够顺利存储在本地。我们这时候选择直接在`actions`中添加修改数据的操作：

```typescript
actions: {
        changeHeadShot() {
            console.log('数据存储成功');
            this.login = require('../assets/images/head.png')
        }
    }
```

将我们写好的`action`在组件中运用：

```vue
<template>
  <!-- 省略不必要的代码 （这里用的是vant的组件）-->
      <van-col span="8" @click="headerC">
        <van-button class="btn2" plain hairlin type="primary" to="/">
          <p class="text">登录</p>
        </van-button>
      </van-col>
</template>

<script setup>
//引入store
import { mainStore } from '@/store/user'
const login = mainStore()

//实现点击修改头像的函数
function headerC() {
  login.changeHeadShot()
}
</script>

<style scoped>
/* 省略不必要的代码 */
</style>
```



接下来便是"见证奇迹"的时刻：我们点击登录按钮后，头像将会发生改变：![image.png](https://dd-static.jd.com/ddimg/jfs/t1/203149/23/26768/3031/63120208E9187c1f0/13daed00483d6234.png)

这时候无论我们怎么刷新，头像都不会出现变化了，我们打开控制台点开`应用中的存储`，可以看见我们的登录头像已经存储在本地的浏览器中:

![image.png](https://dd-static.jd.com/ddimg/jfs/t1/140337/24/29151/8767/631202e4Efa337aba/c99bc2eacc30764b.png)



## 结语

总而言之，无论你是否之前接触过Vuex，我都更推荐你使用`Pinia`，他相比较于Vuex，有着更好的兼容性，在Vuex的基础上去掉了`Mutation`，让语法更加简练，更符合Vue3的`Composition api`，Vuex也不会再进行更新，现在已经处于维护状态了，而Pinia作为下一代的Vuex，又有什么理由不去学习和使用呢？
