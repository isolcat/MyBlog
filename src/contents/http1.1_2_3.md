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

![image.png](https://ldbbs.ldmnq.com/bbs/topic/attachment/2023-2/17882d4b-3492-44a6-b4b3-a46e04ebd0dd.png)

![image.png](https://ldbbs.ldmnq.com/bbs/topic/attachment/2023-2/11891685-7e77-4b32-9fa1-e39c407f6d8e.png)

## HTTP/2

HTTP/2 解决了 HTTP/1 的多重问题：

- 单次连接限制：HTTP/1 限制单次连接只能下载一个资源，导致大量的请求和响应数据包，HTTP/2 通过增加多路复用的技术，允许多个请求和响应数据在单个连接中共存，减少了请求数。

- 顺序加载：HTTP/1 会因为顺序加载的限制而降低页面的加载速度，HTTP/2 通过增加服务器推送技术，允许服务器主动推送资源，提高了页面的加载速度。

- 报文头压缩：HTTP/1 使用的是明文格式的报文头，带宽占用较大，HTTP/2 则采用了二进制制式的报文头，并且使用了 HPACK 算法进行压缩，减小了带宽占用。

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

![image.png](https://ldbbs.ldmnq.com/bbs/topic/attachment/2023-2/5b2218a7-63b5-43b9-99fc-407468635931.png)

**减少了来回带来的开销**，如果是恢复的会话，还直接实现0消耗

HTTP3默认就需要**加密传输**

