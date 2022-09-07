---
title: HTTP学习笔记
date: 2022-05-06
tags:
 - HTTP
categories:
 - Network
---

## 课前提问

* 什么是三次握手？

* HTTPS链接的创建过程，为什么HTTPS就是安全的

* 什么是长链接，为什么需要长链接

* HTTP2的信道复用为什么能提高性能

* 浏览器输入URL后HTTP请求返回的完整过程

![image-20220810172048140](http://cdn.yangdw.cn/img/image-20220810172048140.png)

## 1. HTTP协议基础及发展历史

### 网络协议分层 - 五层模型

![image-20220810172346857](http://cdn.yangdw.cn/img/image-20220810172346857.png)

**低三层**

* 物理层：定义物理设备如何传输数据
* 数据链路层：在通信的实体间建立数据链路连接
* 网络层：数据在结点之间传输创建逻辑链路

**传输层**

向用户提供可靠的端到端(End-to-End)服务。

**应用层**

为应用软件提供服务，构建于TCP协议之上，屏蔽网络传输相关细节。

### HTTP协议的发展历史

#### HTTP/0.9

只有一个命令 GET

没有HEADER等描述数据的信息。

服务器发送完毕，就关闭TCP 连接。

#### HTTP/1.0

增加了很多命令 POST PUT DELETE

增加了status code和header

增加了多字符集支持、多部分发送、权限、**缓存**等

短连接：浏览器的每次请求都需要与服务器建立一个TCP连接，请求处理完成后立即断开。

在加入keep-alive后也可以实现长连接。

#### HTTP/1.1

持久连接（长连接）

pipeline（在一个TCP连接上可以传送多个HTTP请求和响应）

增加host和其他一些命令（在同一个物理服务器上可以使用多个web服务）

**HTTP/2.0**

所有数据以二进制传输

同一个连接里面发送多个请求不再需要按照顺序

头信息压缩以及推送等提高效率

### HTTP的三次握手

![image-20220810174308687](http://cdn.yangdw.cn/img/image-20220810174308687.png)

三次握手是指建立一个TCP连接的时候，需要客户端和服务器总共发送3个包。主要作用是为了确定双方的接收能力和发送能力是否正常、指定自己的初始化序列号为后面的可靠性传输做准备。

SYN，ACK为标志位。

### URI、URL、URN

URI: Uniform Resource Identifier / 统一资源标志符

用来唯一标识互联网上的信息资源，包括URL和URN。

URL : Uniform Resource Locator / 统一资源定位器

```js
http://user:pass@host.com:80/path?query=string#hash
协议名称/用户认证@服务器位置:端口/路由?搜索参数#文档片段（锚点定位）
```

URN：永久统一资源定位符

在资源移动之后还能被找到

