---
title: Ajax
date: 2022-06-08
tags:
 - Ajax
categories:
 - Ajax
---

# express基本使用

 ```js
//  1. 引入express
const { response } = require('express');
const express = require('express')
// 2. 创建应用对象
const app = express();
// 3. 创建路由规则
// request 是对请求报文的封装
// response 是对响应报文的封装
app.get('/', (request, response) => {
    // 设置响应
    response.send('HELLO EXPRESS')
})
// 4. 监听端口启动服务
app.listen(8000, () => {
    console.log('服务已经启动, 8080端口监听中...');
})
 ```

# Ajax请求的基本操作

![image-20220608115747030](http://cdn.yangdw.cn/img/image-20220608115747030.png)

## GET.html

```js
<button>点击发送请求</button>
    <div id="result"></div>
    <script>
        // 获取绑定button元素
        const btn = document.getElementsByTagName('button')[0]
        const result = document.getElementById('result')
        btn.onclick = function(){
            // console.log('test');
            // 1.创建对象
            const xhr = new XMLHttpRequest();
            // 2.初始化 设置请求方法和url
            xhr.open('GET','http://127.0.0.1:8000/server')
            // 3. 发送
            xhr.send()
            // 4. 事件绑定 处理服务端返回的结果
            xhr.onreadystatechange = function(){
                if(xhr.readyState === 4){
                    //2xx 成功
                    if(xhr.status >= 200 && xhr.status < 300){
                        // 处理响应结果
                        // 1. 响应行
                        console.log(xhr.status);
                        console.log(xhr.statusText);
                        console.log(xhr.getAllResponseHeaders());
                        console.log(xhr.response);
                        // 设置result的文本
                        result.innerHTML = xhr.response
                    }
                }
            }
        }
```

## Server.js

```js
//  1. 引入express
const { response } = require('express');
const express = require('express')
// 2. 创建应用对象
const app = express();
// 3. 创建路由规则
// request 是对请求报文的封装
// response 是对响应报文的封装
app.get('/server', (request, response) => {
    // 设置响应头 设置允许跨域
    response.setHeader('Access-Control-Allow-Origin','*')
    // 设置响应
    response.send('HELLO Ajax')
    
})
// 4. 监听端口启动服务
app.listen(8000, () => {
    console.log('服务已经启动, 8080端口监听中...');
})
```

## 设置请求头

```js
xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded')
xhr.setRequestHeader('name','BIT')
```

# 请求超时与网络异常

`GET.html`

```js
xhr.timeout = 2000
// 超时回调
xhr.ontimeout = function(){
    alert('网络异常，请稍后重试！')
}
// 网络异常回调
xhr.onerror = function(){
    alert('你的网络似乎出了一些问题')
}
```

`server.js`

```js
app.get('/delay', (request, response) => {
    // 设置响应头 设置允许跨域
    response.setHeader('Access-Control-Allow-Origin','*')
    // 设置响应
    setTimeout(() => {
        response.send('延时响应')
    },3000)
    // response.send('HELLO Ajax')
})
```

