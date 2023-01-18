---
title: HTTPS是什么？加密原理和证书。SSL/TLS握手过程
datetime: 2023-1-18T15:00:11Z
featured: false
draft: false
tags:
  - 面试题
description: HTTPS是什么？加密原理和证书。SSL/TLS握手过程
---

# HTTPS是什么？加密原理和证书。SSL/TLS握手过程

## HTTPS

https并不是一个单独的协议，它在http的基础上用`TSL/SSL`进行加密，这样通信就不容易受到拦截和攻击

## SSL/TLS

SSL为TLS的**前身**，他们两个都是加密协议，但现在绝大多数浏览器都不支持SSL了，只支持TLS，**TLS两种加密方式都用了**

## 对称加密

加密和解密的规则是一样的算法

<img src="https://pic1.58cdn.com.cn/nowater/webim/big/n_v2393563424a6e4318abf38f880ad6cdd2.png" alt="image.png" style="zoom:80%;" />

## 非对称加密

<img src="https://pic5.58cdn.com.cn/nowater/webim/big/n_v2e8b05e1421154edf89965e5350bf16bb.png" alt="image.png" style="zoom:80%;" />

非对称加密的核心就是用**两个密钥**来进行加密和解密，分别为`公开和私有`两种，公开的密钥是大家都知道的密钥，私有密钥是仅有持有方才有的。**一般来说私钥放在服务器里，数据经过公钥加密就只能被私钥解密，数据经过私钥加密就只能通过公钥解密**

如果用在服务端里，就是服务端拥有公钥和私钥，然后公布公钥让客户端知道

<img src="https://pic8.58cdn.com.cn/nowater/webim/big/n_v2814ceb02e775460eaf2855988bd3b35f.png" alt="image.png" style="zoom:80%;" />

## SSL证书

**SSL证书就是保存在源服务器的数据文件，要想让ssl证书生效，就必须得像CA申请**，这个证书里有特定的公钥和私钥，安装证书后就可以通过https进行访问了

## TLS握手过程（重点）

![image.png](https://pic6.58cdn.com.cn/nowater/webim/big/n_v20ed433f4d8624aa88174de0d0a736fa2.png)

**会话密钥只用于当前会话，如果与其他服务器进行沟通，就建立新的密钥，前6个为非对称密钥，在第七点的时候为对称密钥**

**预主密钥不会直接发送出去，而是用刚刚收到的公钥进行加密之后再发出去**

**当客户端和服务端都得到会话密钥后，就会直接进行对称密钥，节省了非对称密钥的消耗同时也保证了安全性**