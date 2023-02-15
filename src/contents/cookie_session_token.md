---
title: Cookie、Session、Token究竟区别在哪
datetime: 2023-1-18T22:00:11Z
featured: false
draft: false
tags:
  - 面试题
  - 计算机网络
description: Cookie、Session、Token究竟区别在哪
---
# Cookie、Session、Token究竟区别在哪

http**无状态**，所以我们无法知道每次登录的是谁，要实现让网页知道每次登录的是谁，就需要cookie

## cookie和session

![image.png](https://ldbbs.ldmnq.com/bbs/topic/attachment/2023-2/d407616a-d34f-465e-b83d-84c9776654dd.png)

但这里其实我们打开浏览器就可以看见保存了哪些cookie，如果浏览器被黑，用户账号和信息就会泄露，所以把**用户名和密码放在cookie里面是不安全的**

## session

意思是**会话**，不同的网站对于每个用户的会话都设定了时间和唯一的id（**session ID**）这里的时间就是**结束会话的时间**，而这些信息就保存在**数据库**中

![image.png](https://ldbbs.ldmnq.com/bbs/topic/attachment/2023-2/c3eec108-98fb-4a9e-b922-faca345406dc.png)

Session id即使被黑客拿到也没有多大的作用

### 为什么需要cookie

**http协议是无状态**的

使用cookie可以保存上次的状态

### cookie和session的区别

cookie存在客户端

session存在服务端

![image.png](https://ldbbs.ldmnq.com/bbs/topic/attachment/2023-2/d5a7c4b3-8b46-4cf1-a15b-7d04efcb4f86.png))

## token

JWT：JSON Web Token

![image.png](https://ldbbs.ldmnq.com/bbs/topic/attachment/2023-2/d16279da-1141-441b-8723-f9c7e433dd36.png)

jwt组成：

![image.png](https://ldbbs.ldmnq.com/bbs/topic/attachment/2023-2/a59b9d5b-bce4-4f3d-9332-2727f54d56de.png)

## token和cookie也可以进行加密，为啥一定要用JWT呢?

Token 和 Cookie 都可以加密，并且它们都可以用于存储会话状态信息。但是，在一些特定情况下，JWT 比 Token 和 Cookie 更加适合。

主要原因如下：

- JWT 可以被解码：JWT 是一种可以在客户端和服务端之间直接传递的格式，因为它可以被解码，所以不需要再次请求服务器，以获取关于用户身份的信息。

- JWT 可以跨域：JWT 是一种跨域解决方案，因为它可以在不同的域名间传递，并且不需要使用 Cookie。

- JWT 不会存储在客户端：JWT 不会存储在客户端，而是存储在 HTTP 请求头中，这可以防止数据被篡改。

因此，如果您需要跨域解决方案，或者需要在客户端和服务端之间直接传递数据，那么 JWT 可能是一个更好的选择。但是，如果不需要这些功能，则 Token 和 Cookie 也可以适当的解决您的需求。

## 总结

session诞生并保存在**服务器**

cookie则是一种**数据载体**，把session放在cookie中送到客户端那边，cookie跟随http的每个请求发送出去

token**诞生在服务器，但保存在浏览器**，持有token就像持有令牌，可以访问不同服务器
