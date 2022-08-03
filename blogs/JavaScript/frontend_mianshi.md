---
title: 前端面试相关
time: 2022-7-25
tags:
 - JS
categories:
 -  JavaScript
---
# 前端知识体系

* CSS基础知识

* JS基础语法

* JS-Web-API

* 开发环境

* 运行环境
* HTTP协议

![](http://cdn.yangdw.cn/img/%E5%89%8D%E7%AB%AF%E9%9D%A2%E8%AF%95%E8%AF%BE-%E6%80%9D%E7%BB%B4%E5%AF%BC%E5%9B%BE.png)

# 面试准备

## 面试的环节和流程

一面（基础知识）、二面（交叉面试）、三面、hr面试

## JD 分析

JD是用人单位发布的招聘信息。

职位描述，岗位要求

工作内容，技术栈

## 如何写简历

# HTML面试题

1. 如何理解HTML语义化

    ```js
    1.让人更容易读懂（增加代码可读性）
    2.让搜索引擎更容易读懂（SEO）
    ```

    

2. 默认情况下，哪些HTML标签是块级元素，哪些是内联元素

    ```js
    display:block/table; 有div h1 h2 table ul ol p等
    display:inline/inline-block 有span img input button等
    ```

    



# CSS面试题

知识模块

* 布局
* 定位
* 图文样式
* 响应式
* CSS3

## 布局

1. 盒子模型的宽度如何计算？

![image-20220725220313079](http://cdn.yangdw.cn/img/image-20220725220313079.png)

```js
offsetWidth = (内容宽度 + 内边距 + 边框)，无外边距,答案为122px
补充：如果让offsetWidth等于100px，该如何做？
加一个  box-sizing:border-box
盒模型分类：
标准盒模型
盒子总高 = height + padding + border + margin
IE怪异盒模型
盒子总高 = height + margin
```

1. margin纵向重叠的问题

    ```js
    在同一个BFC中，相邻元素的margin-tom和margin-bottom纵向会重叠，合并成margin较大的值。
    空白内容的<p> </p> 也会重叠
    ```

2. margin负值的问题

    ```js
    margin-left和margin-top影响自己，自己会移动
    margin-right和margin-bottom影响别的盒子，右边和下面元素会移动
    ```

1. BFC理解和应用

    ```js
    BFC：块级格式化上下文
        一块独立的渲染区域，内部元素的渲染不会影响边界以外的元素
    形成BFC的常见条件：
        float不是none
        position是absolute或fixed
        overflow不是visible
        fisplay是flex  inline-block
    应用：
    	清除浮动
    ```

2. float布局的问题，以及clearfix

    * 如何实现圣杯布局和双飞翼布局

        * 目的：
            * 1. 三栏布局，中间一栏最先加载和渲染
                2. 两侧内容固定，中间内容随着宽度自适应
        * 技术总结
            * 使用float布局
            * 两侧使用margin负值，以便和中间内容横向重叠
            * 防止中间内容被两侧覆盖，一个用padding，一个用margin

    * 手写clearfix

        * ```js
            .clearfix:after {
                content:'';
                display:table;
                clear:both;
            }
            ```

        * 

3. flex画色子

    flex布局

    常用语法： flex-direction、justify-content、align-items 、flex-wrap、align-self

## 选择器

### 选择器

```js
-- id选择器(#box) B
-- 类选择器(.one) C
-- 标签选择器(div) D
-- 后代选择器(#box div)
-- 子选择器(.one>one_1) 选择父元素为.one的所有.one_1的元素
-- 伪类选择器(:link  :visited :active :hover :first-child) C
-- 伪元素选择器 D
-- 属性选择器 C
```

### 优先级

```js
内联A > ID选择器B > 类选择器C > 标签选择器D

如果外部样式需要覆盖内联样式，就需要使用!important
```

## BFC的理解

```js
BFC（Block Formatting Context），即块级格式化上下文。
1. 内部盒子会在垂直方向上一个接一个的放置
2. 对于同一个BFC的两个相邻的盒子的margin会发生重叠，与方向无关。
3. BFC的区域不会与float的元素区域重叠
4. 计算BFC的高度时，浮动子元素也参与计算
-------
触发BFC的条件
1. 浮动float
2. overflow不为visible，一般为hidden
3. display的值为flex,inline-block,grid等
4. position的值为absolute或fixed
```

## 定位 

### 水平垂直居中

1. absolute和relative分别依据什么定位？

    ```js
    relative依据自身定位
    absolute依据最近的一层的定位元素定位。
    定位元素是指absolute relative fixed
    ```

2. 居中对齐有哪些实现方式

    ```js
    水平居中：
    	inline元素：text-align:center
        block元素： margin:auto
        absolute元素： left:50% + margin-left负值 （必须知道宽高）
    垂直居中
    	inline元素：line-height的值等于height值
    	absolute元素：top:50% + margin-top负值 （必须知道宽高）
    	absolute元素：top:50%;left:50% transform:translate(-50%,-50%)
    	absolute元素： top,left,bottom,right = 0 + margin:auto
    ```

3. 水平垂直居中

```js
flex弹性布局
.father {
    display:flex;
    justify-content:center;
    align-items:center;
}
grid网格布局
```

小结：不知道元素宽高大小仍能实现水平垂直居中：

1. 定位子绝父相 + margin:auto
2. 定位子绝父相 + top: 50%, left: 50% transform:transparent(-50%,50%)
3. 定位子绝父相 + margin：负值 
4. flex布局
5. grid布局

### 两栏布局，右侧自适应，三栏布局

方法1：使用float左浮左边栏，右边模块使用margin-left撑出内容块做展示，为父级元素添加BFC

```js
html
  <div class="box">
    <div class="left">左边</div>
    <div class="right">右边</div>
  </div>
css
  <style>
    .box {
      overflow: hidden;
    }
    .left {
      float: left;
      width: 200px;
      height: 400px;
      background-color: gray;
    }
    .right{
      height: 400px;
      margin-left: 200px;
      background-color: lightgray;
    }
  </style>
```

方法2： flex弹性布局

```js
html
  <div class="box">
    <div class="left">左边</div>
    <div class="right">右边</div>
  </div>
css
  <style>
    .box {
      display: flex;
      height: 200px;
    }
    .left {
      width: 100px;
      background-color: gray;
    }
    .right {
      background-color: lightgray;
      flex: 1;
    }
  </style>
```

### 三栏布局

1. 两边使用float，中间使用margin
2. 两边使用absolute,中间使用margin
3. flex实现 display:flex; 子：flex:1;
4. grid实现 display:grid; width:100%; grid-template-columns: 300px auto 300px;

```js
html
  <div class="container">
    <div class="left">左边固定宽度</div>
    <div class="main">中间自适应</div>
    <div class="right">右边固定宽度</div>
  </div>
css
  <style>
    .container {
      position: relative;
    }
    .left,.right,.main {
      height: 200px;
    }
    .left {
      position: absolute;
      top: 0;
      left: 0;
      width: 100px;
      background: green;
    }
    .right {
      position: absolute;
      top: 0;
      right: 0;
      width: 100px;
      background: green;
    }
    .main{
      margin: 0 auto;
      background: pink;
    }
  </style>
```



## 图文样式

1. line-height的继承问题

    ```js
    写具体数值，如30px，则继承30px
    写比例，如2/1.5,则继承该比例
    写百分比，如200%,则继承计算出来的值，
    如父元素{
    	font-size:20px;
    	line-height:200%;
    }
    子元素{
        font-size:16px;
    }
    则子元素行高为20px * 2 = 40px
    ```

## CSS绘制三角形

```js
.wrap {
  width: 0;
  height: 0;
  border-width: 12px;
  border-color: transparent transparent red transparent;
  border-style: solid;
}
```

## 预处理器

```js
变量（variables）
作用域（scope）
代码混合（ mixins）
嵌套（nested rules）
代码模块化（Modules）
```

## 响应式

1. rem是什么？

    ```js
    rem是一个相对长度单位，相对于根元素，常用于响应式布局。
    ```

2. 如何实现响应式，响应式的常见方案？

    ```js
    media-query,根据不同的屏幕宽度设置根元素font-size
    ```

3. vw/vh 网页视口尺寸  rem的弊端

```js
rem弊端：存在阶梯性
网页视口尺寸：
	window.screen.height //屏幕高度
    window.innerHeight // 网页视口高度
	document.body.clientHeight //body高度

vh:网页视口高度的1/100
vw:网页视口高度的1/100
```

## CSS3 

1. 关于CSS3动画

# JS常见面试题

## 1. 数组常用的方法

操作方法

```js
增
push、unshift、splice(起始位置，删除个数，插入元素)、concat
// 前三个会修改原数组，同时splice返回的是空数组
```

```js
删
pop、shift、splice、slice
// 前三个修改原数组，
改
splice
查
indexOf、includes、find
// indexOf返回-1,includes返回true或false,find传入一个function,返回第一个找到的元素
```

排序方法

```js
reverse()、sort((a,b)=> a - b)
```

转换方法

```js
join() //接收一个参数，即字符串分隔符，返回包含所有项的字符串
```

迭代方法

```js
都不改变原数组
some()、every()、forEach()、fliter()、map()、reduce()

let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
let everyResult = numbers.every((item, index, array) => item > 2);
```

## 2.字符串常用方法

字符串是不可被改变的，方法都是创建一个副本来实现操作。

操作方法

```
增
字符串拼接  +  ${} 
concat() 返回字符串的副本
```

```js
删
slice()、substring(from,to)、substring(from,length)
改
trim()、repeat(number)、toLowerCase、toUpperCase
查
charAt(number)、indexOf()、includes()、startWith()
```

转换方法

```js
split() //把字符串按照指定的分隔符，拆分成数组中的每一项
```

模板匹配语法

```js
match(//)、search(//)、replace(string,string)、replaceAll()
```

## 3. 浅拷贝和深拷贝

浅拷贝 Object.assign() 、 拓展运算符、

```js
function shallowClone(obj) {
    const newObj = {};
    for(let prop in obj) {
        if(obj.hasOwnProperty(prop)){
            newObj[prop] = obj[prop];
        }
    }
    return newObj;
}
```

深拷贝

```js
function deepClone(obj) {
  if (typeof obj !== 'object' || obj == null) {
    // obj 是null 或者不是对象和数组，直接返回
    return obj
  }
  // 初始化返回结果
  let result
  if (obj instanceof Array) {
    result = [] 
  } else {
    result = {}
  }
  for (let key in obj) {
    // 保证key不是原型的属性
    if (obj.hasOwnProperty(key)) {
      result[key] = deepClone(obj[key])
    }
  }
  // 返回结果
  return result
}
```

## 4. 防抖和节流



# JS基础-变量类型和计算面试题

题目：

1. typeof能判断哪些类型

    ```js
    识别所有值类型
    识别函数
    判断是否是引用类型 'object' ,往下无法细分，即无法识别是{}还是[] 还是null等
    typeof null  // 'object'
    ```

2. 何时使用=== 何时使用 ==

    ```js
    //除了 == null 之外，其他一律都用 == ,因为==会做类型转换
    ```

3. 值类型和引用类型的区别

4. 手写深拷贝

知识点：

值类型

```js
let a = 100
let b = a
a = 200
console.log(b) // 100
```

引用类型

```js
let a = {age :20}
let b = a 
b.age = 21
console.log(a.age) // 21 
```

常见值类型

```js
let a //undefined
const s = 'abc' 
const n = 100
const b = true
const s = Symbol('s')
```

常见引用类型

```js
const obj = {x:100}
const arr = ['a','b','c']
const n = null //特殊引用类型，指针指向空地址
function fn(){} //特殊引用类型，不用于存储数据，没有拷贝、复制等
```

变量计算--类型转换

* 字符串拼接

```js
const a = 100 + 10 //110
const b = 100 + '10' //'10010'
const c = true + '10' //'true10'
```

* == 运算符

```js
100 == '100' // true
0 == '' // true
0 == false // true
false == '' // true
null == 'undefined' // true
//除了 == null 之外，其他一律都用 ===,例如
const obj = {x : 100}
if(obj.a == null){}
// 相当于：
// if(obj.a === null || obj.a === undefined){}

```

* if语句和逻辑运算

```js
truthy变量：!!a === true 的变量
falsy变量： !!a === false的变量
-----------------------------------
//以下是falsely变量。除此之外都是truly变量
!!0 === false
!!NaN === false
!!'' === false
!!null === false
!!undefined === false
!!false === false 
```

```js
console.log(10 && 0) // 0
console.log('' || 'abc') // 'abc'
console.log(!window.abc) // true
```

# JS基础-原型和原型链面试题

题目：

1. 如何准确判断一个变量是不是数组？

    ```js
    1. Array.isArray(arr)
    2. arr.__proto__
    3.arr instanceof Array
    4. Object.prototype.toString.call(arr)
    ```

2. 手写一个简易的jQuery,考虑插件和拓展性

3. class的原型本质，怎么理解？

知识点

1. class和继承

    ```js
    constructor、属性、方法
    class Student{
      constructor(name, number) {
        this.name = name
        this.number = number
        // this.gender = 'male'
      }
      sayHi() {
        console.log(`姓名 ${this.name}, 学号 ${this.number}`);
      }
    }
    //通过类 new 对象/实例
    const xialuo = new Student('夏洛', 100)
    console.log(xialuo.name); // 夏洛
    console.log(xialuo.number);// 100
    xialuo.sayHi() // 姓名 夏洛, 学号 100
    ```

    ```js
    // 继承 extends  super 拓展或重写方法
    // 父类
    class People {
      constructor(name) {
        this.name = name
      }
      eat() {
        console.log(`${this.name} eat something`);
      }
    }
    // 子类
    class Student extends People {
      constructor(name, number) {
        super(name)
        this.number = number
      }
      sayHi() {
        console.log(`姓名 ${this.name} 学号 ${this.number}`);
      }
    }
    const xialuo = new Student('夏洛', 100)
    console.log(xialuo.name);
    console.log(xialuo.number);
    xialuo.sayHi()
    xialuo.eat()
    ```

2. 类型判断instanceof

    ```js
    console.log(Student.prototype.__proto__)
    console.log(People.prototype)
    console.log(People.prototype === Student.prototype.__proto__) // true
    ```

3. 原型和原型链

    ```js
    // 原型
    每个class都有显式原型prototype
    每个实例都有隐式原型 __proto__
    实例的__proto__ 指向对应class的prototype
    基于原型的执行规则
    获取属性xialuo.name或执行方法时
    	先在自身属性和方法查找
        如果找不到则自动去 __proto__中查找
    ```

    ​																			原型链

    ![image-20220726212538026](http://cdn.yangdw.cn/img/image-20220726212538026.png)

继续：

![image-20220726212820339](http://cdn.yangdw.cn/img/image-20220726212820339.png)



# JS基础-作用域和闭包 this面试题

题目：

1. this的不同应用场景，如何取值？

2. 手写bind函数

    ```js
    Function.prototype.bind1 = function () {
      // 将参数拆解为数组
      // const args = Array.prototype.slice.call(arguments)
      const args = [...arguments]
      // 获取this(数组第一项)
      const t = args.shift()
      //fn1.bind(...)中的fn1
      const self = this
      // 返回一个函数
      return function () {
        return self.apply(t,args)
      }
    }
    ```

3. 实际开发中闭包的应用场景，举例说明

    ```js
    隐藏数据
    // 闭包隐藏数据，只提供API
    function createCache() {
      const data = {}
      return {
        set: function (key, val) {
          data[key] = val
        },
        get: function (key) {
          return data[key]
        }
      }
    }
    
    const c = createCache()
    c.set('a',100)
    console.log(c.get('a'));
    ```

4. ![image-20220727133449656](http://cdn.yangdw.cn/img/image-20220727133449656.png)

知识点

作用域和自由变量

```js
全局作用域 函数作用域、块级作用域（ES6新增）
if(true){
    let x =100
}
console.log(x) //会报错
自由变量
	一个变量在当前作用域没有定义，但被使用了
    向上级作用域，一层一层依次寻找，直至找到为止
    如果到全局作用域都没找到，则报错 xx is not defined
```

闭包

```js
作用域应用的特殊情况，有两种表现
	函数作为参数被传递
    函数作为返回值被返回

```

![image-20220727134336310](http://cdn.yangdw.cn/img/image-20220727134336310.png)

```js
// 函数作为返回值
function create() {
  let a = 100
  return function () {
    console.log(a);
  }
}
const fn = create()
const a = 200
fn() // 100

// 函数作为参数被传递
function print(fn) {
  const b = 200
  fn()
}
const b = 100
function fn() {
  console.log(b);
}
print(fn) // 100
// 闭包：自由变量的查找，是在函数定义的地方，向上级作用域查找，不是在执行的地方。
```

this

```js
this的取值，是在函数执行的时候确定的。
应用场景：
	作为普通函数
    使用call apply bind
    作为对象方法被调用
    在class方法中调用
    箭头函数
```

![image-20220727140127950](http://cdn.yangdw.cn/img/image-20220727140127950.png)

![image-20220727140146540](http://cdn.yangdw.cn/img/image-20220727140146540.png)

![image-20220727140229926](http://cdn.yangdw.cn/img/image-20220727140229926.png)

# JS基础-异步和单线程

题目：

1. 同步和异步的区别是什么？

    ```js
    基于JS是单线程语言
    异步不会阻塞代码执行
    同步会阻塞代码执行
    ```

2. 手写Promise加载一张图片

    ```js
    function loadImag(src) {
      const p =  new Promise(
        (resolve, reject) => {
          const img = document.createElement('img')
          img.onload = () => {
            resolve(img)
          }
          img.onerror = () => {
            const err = new Error(`图片加载失败 ${src}`)
            reject(err)
          }
          img.src = src
        }
      )
      return p
    }
    const url1 = 'http://cdn.yangdw.cn/img/image-20220727154020395.png'
    const url2 = 'http://cdn.yangdw.cn/img/image-20220727140146540.png'
    // loadImag(url).then(img => {
    //   console.log(img.width);
      // return img // 普通对象
    // }).then(img => {
    //   console.log(img.height);
    // }).catch(err => console.log(err))
    loadImag(url1).then(img1 => {
      console.log(img1.width, img1.height);
      return loadImag(url2) // Promise实例
    }).then(img2 => {
      console.log(img2.width, img2.height);
    })
    ```

3. 前端使用异步的场景有哪些？

    ```js
    网络请求，如ajax图片加载
    定时任务，如setTimeout
    ```

4. ![image-20220727154020395](http://cdn.yangdw.cn/img/image-20220727154020395.png)

    ```js
    1
    3
    5
    4
    2
    ```

知识点

单线程和异步

应用场景

```js
网络请求，如ajax图片加载
定时任务，如setTimeout
```

callback hell 和 Promise

# JS异步进阶

面试题

1. 描述event loop机制

    ```js
    event loop的执行过程
    	同步代码，一行一行放在Call Stack执行
        遇到异步，会先'记录'下，等待时机（定时、网络请求等）
        时机到了，就移动到Callback Queue
        如果Call Stack为空（即同步代码执行完） Event Loop开始工作
        轮询查找Callback Queue ，如有则移动到Call Stack执行
        然后继续轮询查找
    ```

2. 什么是宏任务和微任务，两者区别

3. Promise的三种状态，如何变化

    ```js
    pending resolved rejected
    ```

4. ```js
    async function fn(){
        return 100
    }
    (async function(){
        const a = fn() // ??  返回的是一个Promise 
        const b = await fn() // ?? 返回的是100，相当于Promise.then
    })()
    ```

    ```js
    (async function(){
        console.log('start')
        const a = await 100
        console.log('a',a)
        const b = await Promise.resolve(200)
        console.log('b',b)
        const c = await Promise.reject(300) // 必须用try catch
        console.log('c',c)
        console.log('end')
    })() // 执行完毕，打印出哪些内容？
    // start a 100 b 200 报错
    ```

5. ```js
    console.log(100)
    setTimeout(()=>{
        console.log(200)
    })
    Promise.resolve().then(()=>{
        console.log(300)
    })
    console.log(400)
    // 输出 100 400 300 200
    ```

6. ![image-20220728152017357](http://cdn.yangdw.cn/img/image-20220728152017357.png)

    执行后，输出什么？

    

知识点

## event loop（事件循环/事件轮询）

JS是单线程运行的

异步要基于回调来实现

event loop 就是异步回调的实现原理。

```js
JS如何执行
从前到后，一行一行执行
如果某一行执行报错，则停止下面代码的执行
先把同步代码执行完，再执行异步
```

```js
event loop的执行过程
	同步代码，一行一行放在Call Stack执行
    遇到异步，会先'记录'下，等待时机（定时、网络请求等）
    时机到了，就移动到Callback Queue
    如果Call Stack为空（即同步代码执行完） Event Loop开始工作
    轮询查找Callback Queue ，如有则移动到Call Stack执行
    然后继续轮询查找
DOM事件和event loop
	JS是单线程的
    异步(setTimeout,ajax等)使用回调，基于event loop
    DOM事件也使用回调，基于event loop
```

![image-20220728105606725](http://cdn.yangdw.cn/img/image-20220728105606725.png)

## Promise进阶

三种状态

```js
pending resolved rejected
pending --> resolved或 pending --> rejected
```

状态的变化和变现

```js
pending状态，不会触发then和catch
resolved(fulfilled)状态，会触发后续的then回调函数
rejected状态，会触发后续的catch回调函数
```

then和catch对状态的影响

```js
then正常返回resolved，里面有报错则返回rejected
catch正常返回resolved，里面有报错则返回rejected
```

Promise进阶面试题

```js
//第一题
Promise.resolve().then(()=>{
    console.log(1) // 1
}).catch(()=>{
    console.log(2)
}).then(()=>{
    console.log(3) // 3
}) // resolved
// 打印出1 3
```

```js
// 第二题
Promise.resolve().then(()=>{ // rejected
    console.log(1) // 1
    throw new Error('error1')
}).catch(()=>{ //resolved
    console.log(2) // 2
}).then(()=>{ // resolved
    console.log(3) // 3
}) // resolved
// 打印出 1 2 3
```

```js
// 第三题
Promise.resolve().then(()=>{ // rejected
    console.log(1) // 1
    throw new Error('error1')
}).catch(()=>{ //resolved
    console.log(2) // 2
}).catch(()=>{ 
    console.log(3)
}) // resolved
// 打印出 1 2
```

## async/await

async/await是同步语法，彻底消灭回调函数,但和Promise并不互斥

```js
async function loadImg1(){
    const img1 = await loadImg(src1)
    return img1
}
```

async/await和Promise的关系

```js
执行async函数，返回的是Promise对象
await相当于Promise的then
try...catch可捕获异常，代替了Promise的catch
```

异步的本质

```js
async/await是同步语法，彻底消灭回调函数
JS还是单线程，还是异步，还是基于event loop
async/await 只是一个语法糖
```

```js
async function async(){
    console.log('async1 start') // 2 重要
    await async2()				
    // await 的后面都可以看做是callback里的内容，即异步
    console.log('async1 end')	// 5
}
async function async2(){
    console.log('async2')		// 3 重要
}
console.log('script start') 	// 1
async1()
console.log('script end')		// 4
```

## 微任务/宏任务

microTask/macroTask

```js
console.log(100)
setTimeout(()=>{
    console.log(200)
})
Promise.resolve().then(()=>{
    console.log(300)
})
console.log(400)
// 输出 100 400 300 200
```

什么是宏任务，什么是微任务

```js
宏任务：setTimeout，setInterval,Ajax,Dom事件
微任务：Promise async/await
微任务执行时机比宏任务要早。
```

event loop 和DOM渲染

```js
JS是单线程的，而且和DOM渲染共用一个线程
JS执行的时候，得留一些时机供DOM渲染
---------------
每次Call Stack清空（即每次轮询结束），即同步任务执行完
都是DOM重新渲染的机会，DOM结构如有改变则重新渲染
然后再去触发下一次Event Loop
```

![image-20220728145321655](E:\anzhuang\PicGo\pic\image-20220728145321655.png)

微任务和宏任务的区别

```js
宏任务：DOM渲染后触发，如setTimeouut
微任务：DOM渲染前触发，如Promise
```

从event loop解释，为什么微任务执行更早？

```js
微任务是ES6语法规定的
宏任务是由浏览器规定的
```

![image-20220728150348587](http://cdn.yangdw.cn/img/image-20220728150348587.png)

## 手写 Promise

```js
初始化&异步调用
then catch 链式调用
API .resolve .reject .all .race
```

# JS-Web-API-DOM

DOM操作是基础，必备知识

DOM的本质

题目：

1. DOM是哪种数据结构

2. DOM操作的常用API

3. attr和property的区别

4. 一次性插入多个DOM节点，考虑性能

知识点

DOM本质

```js
DOM的本质是一棵树,从HTML解析而来
```

DOM节点操作

```js
获取DOM节点
const div1 = document.getElementById('div1') // 元素
const divList = document.getElementsByTagName('div') // 集合
const containerList = document.getElementsByClassName('.container') // 集合
property // 对JS变量进行修改，修改对象属性，不会体现到html结构中
const pList = document.querySelectorAll('p')
const p1 = pList[0]
p1.style.width = '100px'
attribute //对HTML节点进行修改，能够作用到节点上去
p1.setAttribute('data-name','imooc')
p1.getAttribute('data-name') 
// 两者都有可能引起DOM重新渲染 主要使用property
```

DOM结构操作

```js
新增/插入节点
const div1 = document.getElementById('div1')
//新建节点
const p1 = document.createElement('p')
p1.innerHTML = 'this is p1'
//插入节点
div1.appendChild(p1)
//移动已有节点
const p2 = document.getElementById('p2')
div1.appendChild(p2)
获取子元素列表，获取父元素
p1.parentNode //父元素
div1.childNodes //子元素列表
删除子元素
div.removeChild(child[0])
```

DOM性能

```js
DOM操作非常昂贵，避免频繁的DOM操作
对DOM查询做缓存
将频繁操作改为一次性操作
// 创建一个文档片段，此时还没有插入到DOM树中
const listNode = document.getElementById('list')
const frag = document.createDocumentFragment() //文档片段
// 执行插入
for(let x = 0; x < 10; x++){
    const li = document.createElement("li")
    li.innerHTML = "List item" + x
    frag.appendChild(li)
}
// 都完成之后，再插入到DOM树中
listNode.appendChild(frag)
```

# JS-Web-API-BOM

题目：

1. 如何识别浏览器的类型
2. 分析拆解url各个部分

知识点

navigator

screen

location

```js
location.href
location.protocol 'https'
location.pathname
location.search
location.hash
```

history

```js
history.back()
history.forward()
```

# Web-API-事件

题目：

1. 编写一个通用的事件监听函数

2. 描述事件冒泡的流程

    ```js
    基于DOM树形结构
    事件会顺着触发元素向上冒泡
    应用场景：代理
    ```

3. 无限下拉的图片列表，如何监听每个图片的点击？

    ```js
    事件代理
    用e.target获取触发元素
    用matches来判断是否是触发元素
    ```

    

知识点

事件绑定

```js
//通用的事件绑定函数
function bindEvent(elem,type,fn){
    elem.addEventListener(type,fn)
}
const btn1 = document.getElementById('btn1')
bindEvent(btn1,'click',evevt =>{
    console.log(event.target)  // 获取触发的元素
    event.preventDefault() // 阻止默认行为
    alert('clicked')
})
```

事件冒泡

![image-20220729123804044](http://cdn.yangdw.cn/img/image-20220729123804044.png)

事件代理

# WEB-API-AJAX

题目

1. 手写一个简易的ajax

    ```js
    function ajax(){
        const p = new Promise((resolve,reject)=>{
        const xhr = new XMLHttpRequest()
        xhr.open('GET',url,true)
        xhr.onreadystatechange = function(){
            if(xhr.readyState === 4){
                if(xhr.status === 200){
                    resolve(
                        JSON.parse(xhr.responseText)
                    )
                }else if (xhr.status === 404){
                    reject(new Error('404 not found'))
                }
            }
    	}
    	xhr.send(null)
    	})
        return p
    }
    
    ```

2. 跨域的常用实现方式

    ```js
    JSONP
    CORS
    ```

    

知识点

XMLHttpRequest

```js
// get请求
const xhr = new XMLHttpRequest()
xhr.open('GET','/api',true)
xhr.onreadystatechange = function(){
    if(xhr.readyState === 4){
        if(xhr.status === 200){
            alert(xhr.responseText)
        }else{
            console.log('其他情况')
        }
    }
}
xhr.send(null)
//POST请求
const xhr = new XMLHttpRequest()
xhr.open('POST','/login',true)
xhr.onreadystatechange = function(){
    if(xhr.readyState === 4){
        if(xhr.status === 200){
            alert(xhr.responseText)
        }else{
            console.log('其他情况')
        }
    }
}
const postData = {
    usename:'zhangsan',
    password:'xxx'
}
xhr.send(JSON.stringify(postData))
```

状态码

```js
2xx -- 表示成功处理请求，如200
3xx -- 需要重定向，浏览器直接跳转，如301(永久重定向) 302(临时重定向) 304(资源未改变，使用缓存)
4xx -- 客户端请求错误，如404 403(没有权限)
5xx -- 服务器端错误
```

跨域：同源策略，跨域解决方案

```js
什么是跨域（同源策略）
ajax请求时，浏览器要求当前网页和server必须同源（安全）
同源：协议、域名、端口，三者必须一致
    <img src=跨域的图片地址/>
    <link href=跨域的css地址 />
    <script src=跨域的js地址></script>
JSONP

CORS - 服务器设置http header
response.setHeader("Access-Control-Allow-Origin","http://localhost:8011")
response.setHeader("Access-Control-Allow-Headers","X-Requested-With")
response.setHeader("Access-Control-Allow-Methods","PUT,POST,GET,DELETE,OPTIONS")
```

# WEB-API-存储

题目：描述 cookie localStorage sessionStorage区别

```js
容量
API易用性
是否跟随http请求发送
```



知识点

cookie

```js
cookie
	本身用于浏览器和server通讯
    被"借用"到本地存储来
    可用document.cookie = '...' 来修改
缺点：存储大小，最大4KB
	 http请求时需要发送到服务端，增加请求数据量
```

localStorage和sessionStorage

```js
HTML5专门为存储而设计，最大可存5M
API简单易用 setItem getItem
不会随着http请求被发送出去
// localStorage数据会永久存储，除非代码或手动删除
// sessionStorage数据只存在于当前会话，浏览器关闭则清空
```

# HTTP面试题

题目：

1. http常见的状态码有哪些？
2. http常见的header有哪些？
3. 什么是Restful API
4. 描述一下http的缓存机制（重要）

知识点

## http状态码

```js
状态码分类
	1xx 服务器收到请求
    2xx 请求成功 如200
    3xx 重定向 如301 永久重定向(配合location,浏览器自动处理)   302 临时重定向 304 资源未修改
    4xx 客户端错误，如404资源未找到 403没有权限
    5xx 服务端错误，如500
```

Restful API

```js
一种新的API设计方法
传统API设计：把每个url当做一个功能
Restful API设计： 把每个url当做一个唯一的资源
// 不使用url参数
传统：        /api/list?pageIndex=2
Restful API: /api/list/2
// 用method表示操作类型
post请求：/api/blog
patch请求：/api/blog/100
get请求 /api/blog/100
```

http headers

```js
常见的Request Headers
	Accept 浏览器可接收的数据格式
    Accept-Encoding 浏览器可接收的压缩算法，如gzip
    Accept-Language

	Connection:keep-alive 一次TCP链接重复使用
    cookie
    Host
    User-Agent (UA) 浏览器信息
    Content-type 发送数据的格式，如application/json
常见的Response Headers
	Content-type 返回数据的格式
    Content-length
	Cache-Control
	Content-Encoding
	Set-cookie
```

## http缓存

关于缓存的介绍

```js
什么是缓存？

为什么需要缓存？

哪些资源可以被缓存？ --静态资源（js css img)
```

http缓存策略（强制缓存+协商缓存）

```
强制缓存
Cache-Control
	max-age  最大过期时间
	no-cache 不使用强制缓存
	no-store  不使用强制缓存，也不让服务端做处理，只需要重新返回一份资源
Response Headers中
控制强制缓存的逻辑
例如Cache-Control: max-age = 31536000
```

![image-20220729165151035](http://cdn.yangdw.cn/img/image-20220729165151035.png)

缓存失效后，重新请求服务端

协商缓存

```js
服务端缓存策略
服务器判断客户端资源，是否和服务端资源一样
一致则返回304，否则返回200和最新的资源
```

![image-20220729165935030](http://cdn.yangdw.cn/img/image-20220729165935030.png)

资源标识

```js
在Respon Headers中，有两种
    Last-Modified   资源的最后修改时间
    Etag  			资源的唯一标识(一个字符串)
```

Last-Modified: 资源的最后修改时间

![image-20220729170134175](http://cdn.yangdw.cn/img/image-20220729170134175.png)

Etag: 资源的唯一标识(一个字符串)

![image-20220729170328899](http://cdn.yangdw.cn/img/image-20220729170328899.png)

```js
两者共存的话，
	会优先使用Etag
    Last-Modified只能精确到秒级
	如果资源被重复生成，而内容不变，则Etag更精确
```

![image-20220729170622333](E:\anzhuang\PicGo\pic\image-20220729170622333.png)

刷新页面,不同刷新操作，不同的缓存策略

```js
正常操作：强制缓存有效，协商缓存有效
手动刷新：强制缓存失效，协商缓存有效
强制刷新：强制缓存失效，协商缓存失效
```

## HTTPS

http和https区别

```js
http是明文传输，敏感信息容易被劫持
https = http + 加密，劫持了也无法解密
```



加密方式：

```js
对称加密 
	用一个key来对数据进行加密，用同一个key解密
非对称加密
	一对key,公钥和私钥，A加密之后，只能用B来解密
HTTPS同时用到了两种方式
```

https证书

```js
中间人攻击
	使用第三方证书
    浏览器校验证书
```





