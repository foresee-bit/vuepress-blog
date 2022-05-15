---
title: node学习笔记
data: 2022-05-13
---

## http模块

1. 创建web服务器的基本步骤

    1. 创建web服务器示例

        1. 导入http模块

            ```html
            const http = require('http')
            ```

        2. 调用http.createServer()方法，快速创建一个web服务器实例。

            1. ```html
                const server = http.createServer()
                ```

    2. 为服务器实例绑定request事件，监听客户端的请求

        * 使用服务器实例的 .on() 方法，为服务器绑定一个request 事件

        * ```html
            server.on('request',(req,res) => {
            	//只要有服务端来请求我们的服务器，就会触发request事件，从而调用这个事件处理函数。
            	console.log('Someone visit our web server.')
            })
            ```

    3. 启动服务器

        * 调用服务器实例的 `.listen()`方法，即可启动当前的web服务器实例。

        * ```html
            server.listen(80,()=> {
            	console.log('http server running at http://127.0.0.1')
            })
            ```

### 根据不同的url响应不同的html内容

实现步骤

1. 获取请求的url地址
2. 设置默认的响应内容为 404 Not found
3. 判断用户请求的是否`/`或 `/index.html`页面
4. 判断用户请求的是否为`/about.html`关于页面
5. 设置Content-Type响应头，防止中文乱码
6. 使用res.end()把内容响应给客户端