---
title: axios二次封装
datetime: 2023-3-9T22:32:11Z
featured: false
draft: false
tags:
  - 面试题
description: axios二次封装
---
# axios二次封装

当被问到如何对axios进行二次封装时，可以回答以下几点：

1. 目的：二次封装axios的主要目的是为了提高代码复用性，减少代码重复，统一接口规范，方便项目维护和升级。
2. 封装思路：二次封装axios的思路主要是在axios的基础上进行封装，对**请求和响应进行统一处理**，例如对请求头进行设置，对响应结果进行统一处理、错误处理等
3. 功能扩展：除了对axios进行基本的封装外，还可以根据项目需求对其功能进行**扩展**，例如对请求进行缓存、加入Loading状态、请求取消等。
4. 统一管理：二次封装axios后，可以将其封装成一个模块，统一管理，方便其他模块进行调用。
5. 标准化接口：封装axios时，可以根据项目需求制定一套统一的接口规范，以便开发人员可以快速上手使用，提高团队协作效率。

最后，需要注意的是，二次封装axios不是必须的，如果项目规模较小或者不需要特殊处理，可以直接使用原生的axios

代码演示：

```js
import axios from 'axios';

const instance = axios.create({
  baseURL: 'http://example.com/api',
  timeout: 5000,
  headers: {
    'Content-Type': 'application/json'
  }
});

// 请求拦截器
instance.interceptors.request.use(
  config => {
    // 可以在此处进行请求头的设置
    return config;
  },
  error => {
    return Promise.reject(error);
  }
);

// 响应拦截器
instance.interceptors.response.use(
  response => {
    // 对响应结果进行统一处理
    const res = response.data;
    if (res.code !== 200) {
      console.error('请求失败：' + res.message);
      return Promise.reject(new Error(res.message || 'Error'));
    } else {
      return res;
    }
  },
  error => {
    // 对错误进行统一处理
    console.error('请求失败：' + error);
    return Promise.reject(error);
  }
);

// 封装get请求
export function get(url, params) {
  return instance.get(url, {
    params: params
  });
}

// 封装post请求
export function post(url, data) {
  return instance.post(url, data);
}

```

> axios二次封装分为**拦截器**和**适配器** 统一错误处理和数据预处理就用拦截器 缓存和fetch支持就用适配器 将前一个axios实例进行二次封装 对前一个的axios进行添加拦截器和适配器

> axios二次封装一般可以分为以下几类：
>
> 1. 拦截器的封装：通过拦截器对请求和响应进行预处理或后处理，例如对请求进行加密、添加请求头等操作，对响应进行数据处理、错误处理等操作。
> 2. 接口的统一管理：将多个请求分别封装成一个个函数，进行统一管理，方便调用和维护。这种方式一般需要配合拦截器使用，以实现对请求的自动处理和统一处理。
> 3. 错误处理的封装：对请求发生的各种错误进行统一处理，例如网络错误、请求超时等，以及对服务器返回的错误进行统一处理，例如状态码不正确等。
> 4. 配置的封装：将axios的配置封装成一个配置对象，方便进行全局配置和统一管理，同时也方便在不同环境下进行切换。
>
> 通过对axios的二次封装，可以让我们在使用axios时更加方便和高效，并且可以提高代码的可维护性和复用性。