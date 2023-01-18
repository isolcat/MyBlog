---
title: Cookie、Session、Token究竟区别在哪
datetime: 2023-1-18T22:00:11Z
featured: false
draft: false
tags:
  - 面试题
description: Cookie、Session、Token究竟区别在哪
---
# Cookie、Session、Token究竟区别在哪

http**无状态**，所以我们无法知道每次登录的是谁，要实现让网页知道每次登录的是谁，就需要cookie

## cookie和session

![image.png](https://pic1.58cdn.com.cn/nowater/webim/big/n_v21b022333041e4800b63e8316e0f8ab12.png)

但这里其实我们打开浏览器就可以看见保存了哪些cookie，如果浏览器被黑，用户账号和信息就会泄露，所以把**用户名和密码放在cookie里面是不安全的**

## session

意思是**会话**，不同的网站对于每个用户的会话都设定了时间和唯一的id（**session ID**）这里的时间就是**结束会话的时间**，而这些信息就保存在**数据库**中

![image.png](https://pic5.58cdn.com.cn/nowater/webim/big/n_v299eead38f8af496bb915461add17cd88.png)

Session id即使被黑客拿到也没有多大的作用

### 为什么需要cookie

**http协议是无状态**的

使用cookie可以保存上次的状态

### cookie和session的区别

cookie存在客户端

session存在服务端

 ![image.png](https://pic1.58cdn.com.cn/nowater/webim/big/n_v2d9df8af7e510447db4500413337a3c2e.png)

## token

JWT：JSON Web Token

![image.png](https://pic5.58cdn.com.cn/nowater/webim/big/n_v20c595c1d395f4b63805d140611536344.png)

jwt组成：

![image.png](https://pic2.58cdn.com.cn/nowater/webim/big/n_v24aae9639ffd2418fb9871703f835c9a6.png)

## 总结

session诞生并保存在**服务器**

cookie则是一种**数据载体**，把session放在cookie中送到客户端那边，cookie跟随http的每个请求发送出去

token**诞生在服务器，但保存在浏览器**，持有token就像持有令牌，可以访问不同服务器