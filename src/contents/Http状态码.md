---
title: Http状态码
author: isolcat
datetime: 2022-12-11T02:05:51Z
featured: false
draft: false
tags:
  - 面试题
description: http状态码的区别
---

# Http状态码

## 1xx 信息性

接受的信息处理中，表示**请求还在进行中**，1开头虽然很少用，但在请求中还是很好用

<img src="https://yzf.qq.com/fsna/kf-file/kf_pic/20221127/KFPIC_kfh5221fa29cfc019f_h5cded9881fc7d6fdfece5fb364b_WXIMAGE_22e72d2b041448a4a64d3809f6ee19dd.png" alt="image.png" style="zoom:50%;" />

## 2xx 成功

2开头表示**成功**

- 请求方法有很多种(`GET/HEAD/PUT/POST/TRACE`)，无论使用哪一种，只要请求成功，就返回200
- 创建用户成功返回201
- 接收到请求，但没有东西返回，返回204

<img src="https://yzf.qq.com/fsna/kf-file/kf_pic/20221127/KFPIC_kfh5221fa29cfc019f_h5cded9881fc7d6fdfece5fb364b_WXIMAGE_9aa644fc4f284ba3bf9f3ae9067cddb9.png" alt="image.png" style="zoom:50%;" />

## 3xx 重定向

2开头表示资源已经不在了，已经**移到其他地方了**

- 301让你**重定向**，给你永久移动的新地址(url)
- 302给你**临时url**，暂时使用该地址，下次还得是旧地址
- 304**无需重新下载/加载**，服务器会检查缓存是否过期，如果没过期直接用缓存

<img src="https://yzf.qq.com/fsna/kf-file/kf_pic/20221127/KFPIC_kfh5221fa29cfc019f_h5cded9881fc7d6fdfece5fb364b_WXIMAGE_67c872fe8f874ee1860154948289f2a3.png" alt="image.png" style="zoom:50%;" />

## 4xx 客户端错误

4开头代表这个“锅”和服务端无关，是**客户端出现问题**

- 400代表**语法错误**，无法被理解
- 401表示数据库**查询失败**，无法通过身份认证
- 403表示**权限不够**，被禁止访问
- 404**无法查询资源和路径**，也是大家最常见的`Not Found`
- 409表示出现了**冲突**

<img src="https://yzf.qq.com/fsna/kf-file/kf_pic/20221127/KFPIC_kfh5221fa29cfc019f_h5cded9881fc7d6fdfece5fb364b_WXIMAGE_90548629515447b6a9f383e1bef80c83.png" alt="image.png" style="zoom:50%;" />

## 5xx 服务器错误

5开头代表着**服务器出现问题**

- 500代表**内部出错**
- 502代表**网关错误**
- 503代表**服务器超载或正在维护**

<img src="https://yzf.qq.com/fsna/kf-file/kf_pic/20221127/KFPIC_kfh5221fa29cfc019f_h5cded9881fc7d6fdfece5fb364b_WXIMAGE_5c852407451a414d81cfe9e04a07d1ec.png" alt="image.png" style="zoom:50%;" />