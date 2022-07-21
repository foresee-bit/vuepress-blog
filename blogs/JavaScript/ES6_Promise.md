---
title: ES6_Promise
time: 2022-5-23
tags:
 - JS
categories:
 -  JavaScript
---

## 准备工作

### 1. 函数对象和实例对象

1. 函数对象：将函数作为对象使用时，简称为函数对象

2. 实例对象：new 构造函数或类产生的对象，我们称之为实例对象

```js
  <script>
    function Person(name,age){
      this.name = name
      this.age = age
    }
    Person.a = 1
    const p1 = new Person('老刘',30) //将Person看成一个对象
    console.log(p1); // p1是Person的实例对象
  </script>
```

### 2. 回调函数的分类

前序知识：什么是回调？ ---我们定义的，我们没有调用，最终执行了。

1. 同步的回调函数：
    * 理解：立即在主线程上执行，不会放入回调队列中
    * 例子：数组遍历相关的回调函数
2. 异步的回调函数
    * 理解：不会立即执行，会放入回调队列中以后执行
    * 例子：定时器回调  / ajax回调

```js
//同步回调
let arr = [1,3,5,7,9]
arr.forEach((item)=>{
    console.log(item);
})
console.log("主线程的代码"); // 1 3 5 7 9 主线程的代码
//异步回调
setTimeout(()=>{
    console.log('@');
},0)
console.log('主线程');
// 主线程  @
```

### 3. JS中的error

理解JS中的错误（Error）和错误处理

1. 错误的类型
    * `Error`：所有错误的父类型
    * `ReferenceError`：引用的变量不存在
    * `TypeError`：数据类型不正确
    * `RangeError`：数据值不在其所允许的范围内--死循环
    * `SyntaxError`：语法错误
2. 错误处理
    * 捕获错误： try{} catch(){}
    * 抛出错误：throw error
3. 错误对象
    * message属性：错误相关信息
    * stack属性：记录信息

## Promise

### 初识Promise

Promise是什么？

1. 抽象表达：
    1. Promise是一门新的技术（ES6提出）
    2. Promise是JS中**异步编程**的新方案（旧方案是谁？---纯回调函数）
2. 具体表达：
    1. 从语法上说：Promise是一个**内置**构造函数
    2. 从功能上说：Promise的实例对象可以用来封装一个异步操作，并可以获取其成功/失败的值

1. `Promise`不是回调，是一个内置的构造函数，是程序员自己new 调用的
2. new Promise的时候，都要传入一个回调函数，它是**同步**的回调，会立即在**主线程**上执行，它被称为executor ---执行器
3. 每一个Promise实例都有3种状态：初始化`pengding`、成功`fulfilled`、失败`rejected`
4. 每一个Promise实例在刚被new出来的那一刻，状态都是初始化`pending`
5. executor函数会接收到2个参数，他们都是函数，分别用形参：`resolve`、`reject`接收
    1. 调用resolve函数会：
        1. 让Promise实例状态变为成功`fulfilled`
        2. 可以指定成功的value
    2. 调用reject函数会：
        1. 让Promise实例状态变为失败`rejected`
        2. 可以指定失败的reason

### 封装一个简单的ajax

