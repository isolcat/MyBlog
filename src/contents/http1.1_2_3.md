---
title: HTTP/1.1，HTTP/2和HTTP/3的区别
datetime: 2023-1-18T15:00:11Z
featured: false
draft: false
tags:
  - 面试题
description: HTTP/1.1，HTTP/2和HTTP/3的区别
---

# HTTP/1.1，HTTP/2和HTTP/3的区别

## HTTP/1.1

核心为**一次一份**，HTTP1.1才是互联网真正意义上的第一份HTTP标准

在1.1版本当中，必须收到http响应之后才能进行下一次http请求

![image.png](https://pic6.58cdn.com.cn/nowater/webim/big/n_v2a95b2f6b891d4b05b88921cdeca3ccbf.png)

<img src="https://pic8.58cdn.com.cn/nowater/webim/big/n_v25eb5abab4ba5417fa7da1e65e191e292.png" alt="image.png" style="zoom:67%;" />

## HTTP/2

优点：

- 1.二进制分帧

  **客户端发送的数据变成更小的帧，优化了队头阻塞**

  **但是TCP协议无法根治队头阻塞 http3基于UDP协议解决了队头阻塞**

- 2.多路复用

  **对于统一域名下的请求都是基于流的，也就是同一域名不管访问多少文件，也只建立一路连接**

- 3.头部压缩

  **将http1中每次重复发送相同的头部信息缓存下来，减少传输header的大小**

- 4.服务器推送(server push)

  **与websocket进行区分**

  **http2server push只有请求是才能向客户端推送**

  **websocket可以实时推送**

## HTTP/3

核心：**整合**

![image.png](https://pic6.58cdn.com.cn/nowater/webim/big/n_v2425a2c89af0f4c0e884c4e5942536f36.png)

**减少了来回带来的开销**，如果是恢复的会话，还直接实现0消耗

HTTP3默认就需要**加密传输**

