---
title: JS—巩固
time: 2022-6-20
tags:
 - JS
categories:
 -  JavaScript
---

## 数据类型检测





## 对this的了解

![image-20220622095731185](http://cdn.yangdw.cn/img/image-20220622095731185.png)

![image-20220622101342233](http://cdn.yangdw.cn/img/image-20220622101342233.png)

## 基于HTTP网络层的“前端性能优化”

### 从输入URL地址到看到页面，中间都经历了啥

==第一步==：URL解析

* 地址解析：

![image-20220622183622647](http://cdn.yangdw.cn/img/image-20220622183622647.png)

* 编码解码
    * 对整个URL的编码：处理空格/中文 `encodeURI` `decodeURI`
    * 主要对传递的参数信息编码  `encodeURIComponent`    `decodeURIComponent`

==第二步==：缓存检查   强缓存  协商缓存

缓存位置：

* Memory Cache：内存缓存
* Disk Cache: 硬盘缓存

打开网页：查找disk cache中是否有匹配，如有则使用，如没有则发送网络请求

普通刷新（F5）： 因TAB没关闭，因此memory cache是可用的，会被优先使用，其次才是disk cache

强制刷新（Ctrl + F5）: 浏览器不适用缓存，因此发送的请求头部均带有 Cache-control: no-cache，服务器直接返回`200`和最新内容

先检测是否存在强缓存

	+ 有，且未失效，走强缓存
 + 如果没有，或者失效
     + 检测是否有协商缓存
        	+ 有
        	+ 没有，获取最新数据

**强缓存**  **Expires / Cache-Control**

![image-20220622211656954](http://cdn.yangdw.cn/img/image-20220622211656954.png)

![image-20220622211800420](http://cdn.yangdw.cn/img/image-20220622211800420.png)

问题：服务器资源更新后，缓存的资源没有更新的问题？

1) 服务器更新资源后，让资源名称和之前不一样，这样页面导入全新的资源

2) 当文件更新后，我们在HTML导入的时候，设置一个后缀（时间戳）

```js
<script src = 'index.js?12321313'>
<script src = 'index.js?21432522'>
```

**协商缓存  Last-Modified (HTTP1.0)  /  ETag  (HTTP1.1)**

协商缓存就是强缓存失效后，浏览器携带缓存标识向服务器发起请求，由服务器根据缓存标识决定是否使用缓存。

![image-20220622213247031](http://cdn.yangdw.cn/img/image-20220622213247031.png)

![image-20220622213258454](http://cdn.yangdw.cn/img/image-20220622213258454.png)

协商缓存和强缓存区别：

协商缓存总会和服务器协商，所以一定要发HTTP请求。

```
第一次向服务器发送请求时：
协商缓存没有
向服务器发送请求（没有传递任何的标识）
服务器收到请求准备内容
Last-Modified: 资源文件最后更新的时候
ETag: 记录的是一个标识（也是根据资源文件更新生成的，每一次资源更新都会重新生成一个ETag）
============
客户端拿到信息后渲染
把信息和标识缓存到本地
```

```
第二次发请求
if-Modified-Since = Last-Modified值
if-Node_Match = ETag 值
给服务器
--------
服务器根据标识判断文件是否更新
```

强缓存和协商缓存针对于静态资源文件，而且是不经常更新的。

**数据缓存**

![image-20220622225634393](http://cdn.yangdw.cn/img/image-20220622225634393.png)

![image-20220622225756609](http://cdn.yangdw.cn/img/image-20220622225756609.png)

==第三步==：DNS解析

* 递归查询

![image-20220622230207957](http://cdn.yangdw.cn/img/image-20220622230207957.png)

* 迭代查询

![image-20220622230238048](http://cdn.yangdw.cn/img/image-20220622230238048.png)

第一次DNS解析事件预计在20—120ms.

* 减少DNS请求次数  （）
* DNS预获取 (DNS Prefetch)