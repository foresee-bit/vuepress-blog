---
title: ES6
time: 2022-6-6
tags:
 - JS
categories:
 -  JavaScript
---

# let特性

## 1.变量不能重复声明

## 2. 块级作用域

```js
{
    let girl = '周扬青'
}
console.log(girl) // Uncaught ReferenceError: girl is not defined
```

## 3. 不存在变量提升

```js
console.log(song); // undefined
var song = '京津冀'

console.log(song); // undefined
let song = '京津冀'
// Uncaught ReferenceError: Cannot access 'song' before initialization
```

## 4. 不影响作用域链

```js
{
    let school = 'aaa'
    function fn(){
        console.log(school);
    }
    fn()  // 'aaa'
}
```

## 案例实践

![image-20220606161058876](http://cdn.yangdw.cn/img/image-20220606161058876.png)

```js
let items = document.getElementsByClassName('item')
for (let i = 0; i < items.length; i++) {
    items[i].onclick = function () {
        items[i].style.background = 'pink'
    }
}
//使用var会导致i成为全局变量,当调用回调函数时，出现数组越界错误。
```

# const特性

## 1. 一定要赋初始值

## 2. 一般常量使用大写

## 3. 常量值不能修改

## 4. 块级作用域

## 5. 对于数组和对象的元素修改，不会报错

不能修改的是地址的值。

# 变量的结构赋值

ES6允许按照一定模式从数组和对象中提取值，对变量进行赋值—结构赋值。

## 1. 数组的结构

```js
const F4 = ['小沈阳','刘能','赵四','宋小宝']
let [a,b,c,d] = F4
console.log(a); //小沈阳
console.log(b); //刘能
console.log(c); //赵四
console.log(d); //宋小宝
```

## 2. 对象的解构

```js
const zhao = {
    name:'赵本山',
    age:'不详',
    xiaopin:function(){
        console.log('我可以演小品')
    }
}
let {name,age,xiaopin} = zhao
console.log(name); //赵本山
console.log(age); //不详
console.log(xiaopin); // f() { console.log('我可以演小品') }
xiaopin() // 我可以演小品
```

# 模板字符串

ES6引入新的声明字符串的方式。 

```js
[``]
```

## 1. 声明

```js
let str = `我也是一个字符串`
console.log(str,typeof str);
```

## 2. 内容中可以直接出现换行符

```js
let str1 = `<ul>
                <li>沈腾</li>
                <li>沈腾</li>
                <li>沈腾</li>
                <li>沈腾</li>
			</ul>`
```

## 3. 变量拼接

 ```js
let lovest = '魏翔'
let out = `${lovest}是我心中最搞笑的演员!!`
console.log(out); //魏翔是我心中最搞笑的演员!!
 ```

# 对象的简化写法

ES6允许在大括号里面，直接写入变量和函数，作为对象的属性和方法。

```js
let name = 'BIT'
let change = function(){
    console.log('我们可以改变你!');
}
const school = {
    name,
    change,
    improve(){
        console.log('提高');
    }
}
console.log(school); // Object
```

# 箭头函数以及声明特点

ES6允许使用【箭头】(=>) 定义函数。

```js
let fn = function(a,b){
	return a + b
}
let fn1 =(a,b)=>{
    return a + b 
}
let result =  fn1(1,2)
console.log(result); // 3
```

## 1. this是静态的，this始终指向函数声明时所在作用域下的this的值

```js
function getName(){
    console.log(this.name);
}
let getName2 = () =>{
    console.log(this.name);
}
window.name = 'BIT'
const school1 = {
    name:"理工"
}
```

==直接调用==

```js
getName() //BIT
getName2() //BIT
```

==call方法调用==

```js
getName.call(school1) //理工
getName2.call(school1) //BIT
```

## 2. 不能作为构造实例化对象

```js
let Person = (name,age)=>{
    this.name = name;
    this.age = age
}
let me = new Person('xiao',30)
console.log(me);  // Uncaught TypeError: Person is not a constructor
```

## 3. 不能使用 arguments 变量

```js
let fn2 = () => {
    console.log(arguments);
}
fn2(1, 2, 3)
// Uncaught ReferenceError: arguments is not defined
```

## 4. 箭头函数的简写

### 1） 省略小括号，当形参有且只有一个的时候

```js
let add = n => {
    return n + n;
}
```

## 2)  省略花括号，当代码体只有一条语句的时候

```js
let pow = n => n * n
```

## this案例

![image-20220606171404817](http://cdn.yangdw.cn/img/image-20220606171404817.png)

需求-1： 点击div 2s后颜色变成粉色

```js
<div id="ad"></div>

let ad = document.getElementById('ad')
ad.addEventListener('click',function(){

    // 方式一：
    // let _this = this
    // setTimeout(function(){
    //     _this.style.background = 'pink'
    // },2000)
    // 方式二：
    setTimeout(()=>{
        this.style.background = 'pink'
    },2000)
})
```

需求-2： 从数组中返回偶数的元素

```js
const arr = [1,6,9,10,100,25]
// 方式一：
// const result = arr.filter(function(item){
//     if(item % 2 === 0){
//         return true
//     }else {
//         return false
//     }
// })
// 方式二：
const result = arr.filter(item => item % 2 === 0)
console.log(result); //(3) [6, 10, 100]
```

箭头函数适合与this无关的回调，定时器，数组的方法回调。

箭头数不适合与this有关的回调，事件回调，对象的方法回调。

# ES6 允许给函数参数赋值初始值

## 1. 形参初始值

```js
function add (a,b,c=10){
    return a + b + c
}
let result = add(1,2)
console.log(result); // 13
```

## 2. 与解构赋值结合

```js
function connect({host="127.0.0.1",username,password,port}){
    console.log(host);
    console.log(username);
    console.log(password);
    console.log(port);
}
connect({
    host:'localhost',
    username:'root',
    password:'root',
    port:3306
})
```

# rest参数

ES6引入rest参数，用于获取函数的实参，用来代替arguments

ES5中获取实参的方式

```js
function date(){
    console.log(arguments);
}
date('1','2','3') //Arguments(3) ['1', '2', '3', callee: ƒ, Symbol(Symbol.iterator): ƒ]
```

ES6中 rest参数 返回一个数组，可以调用数组的方法

```js
function date1(...args){
    console.log(args);
}
date1('1','2','3') //(3) ['1', '2', '3']
```

# 拓展运算符和应用

`...`拓展运算符能将`数组`转换为逗号分隔的`参数序列`

```js
const tfboys = ['易烊千玺','王源','王俊凯']
function chunwan(){
    console.log(arguments);
}
chunwan(...tfboys) // chunwan('易烊千玺','王源','王俊凯')
```

## 1. 数组的合并

```js
const kuaizi = ['王太利','肖央']
const fenghuang = ['曾毅','玲花']
const zuixuanxiaopingguo = [...kuaizi,...fenghuang]
console.log(zuixuanxiaopingguo); // (4) ['王太利', '肖央', '曾毅', '玲花']
```

## 2. 数组的克隆

```js
const sanzhihua = ['E','G','M']
const sanyecao = [...sanzhihua]
console.log(sanyecao); //(3) ['E', 'G', 'M']
```

## 3. 将伪数组转为真正的数组

```js
const divs = document.querySelectorAll('div')
const divArr = [...divs]
console.log(divArr);//数组 (4) [div, div, div, div]
console.log(divs); //Object
```

# Symbol 基本使用

ES6引入了一种新的原始数据类型Symbol,表示独一无二的值。它是JavaScript语言的第七种数据类型，是一种了类似于字符串的数据类型。

```js
let s = Symbol()
console.log(s,typeof s); //Symbol() 'symbol'
let s2 = Symbol('BIT')
let s3 = Symbol('BIT')
console.log(s2 === s3) // false
let s4 =  Symbol.for('BIT')
let s5 =  Symbol.for('BIT')
console.log(s4 === s5) // true
```

数据类型总结

USONB 

```js
u undefined
s string symbol
o object
n number null
b boolean
```

## 对象添加Symbol类型的属性



# 迭代器 Iterator

迭代器是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署`Iterator`接口，就可以完成遍历操作。

1.  ES6创造一种新的遍历命令`for ... of `循环， `Iterator`接口主要供`for ... of`消费。

2.  原生具备iterator接口的数据(可用`for  of` 遍历)
    1. `Array`，`Arguments`, `Set`, `Map`, `String`, `TypedArray`, `NodeList`
3. 工作原理：
    1. 创建一个指针对象，指向当前数据结构的起始位置
    2. 第一次调用对象的next方法，指针自动指向数据结构的第一个成员
    3. 接下来不断调用next()方法，指针一直往后移动，直到指向最后一个成员
    4. 每调用next方法返回一个包含value和done属性的对象

```js
 const xiyou = ['唐僧','孙悟空','猪八戒','沙僧']
        // 使用 for ... of 遍历数组
 for(let v of xiyou){
     console.log(v);
 }
let iterator = xiyou[Symbol.iterator]()
console.log(iterator.next()); // {value: '唐僧', done: false}
```

## 迭代器应用—自定义迭代数据

```js
const banji = {
    name:"终极一班",
    stus:[
        'xiaoming',
        'xiaoning',
        'xiaotian',
        'knight'
    ],
    [Symbol.iterator](){
        let index = 0
        let _this = this
        return {
            next: function(){
                if(index < _this.stus.length){
                    const result = {value:_this.stus[index],done:false}
                    index++
                    return result
                }else {
                    return {value:undefined,done:true}
                }
            }
        }
    }
}
for(let v of banji){
    console.log(v) 
    //xiaoming
	//xiaoning
	//xiaotian
	//knight
}
```

# 生成器

生成器是一个特殊的函数

异步编程  纯回调函数

```js
function *gen(){
    // console.log(111);
    yield '一只没有耳朵'
    // console.log(222);
    yield '一只没有尾巴'
    // console.log(333);
    yield '真奇怪'
}
let iterator1 = gen()
console.log(iterator1.next());// {value: '一只没有耳朵', done: false}
console.log(iterator1.next());// {value: '一只没有尾巴', done: false}
console.log(iterator1.next());//{value: '真奇怪', done: false}
console.log(iterator1.next());//{value: undefined, done: true}
for(let v of gen()){
    console.log(v);
    //一只没有耳朵
    //一只没有尾巴
    //真奇怪
}
```

## 生成器函数参数



## 生成器函数实例1

1秒后输出111  2秒后输出222   3秒后输出333 

```js
function one(){
    setTimeout(()=>{
        console.log(111);
        iterator.next()
    },1000)
}
function two(){
    setTimeout(()=>{
        console.log(222);
        iterator.next()
    },2000)
}
function three(){
    setTimeout(()=>{
        console.log(333);
        iterator.next()
    },3000)
}
function* gen() {
    yield one()
    yield two()
    yield three()
}
// 调用生成器函数
let iterator = gen()
iterator.next()
```

## 生成器函数实例2

1秒后获取用户数据  1秒后获取订单数据  1秒获取货物数据

通过在`iterator.next(data)`传入参数可以将这个参数传递给 `yield  xxx()` 的返回值。

```js
function getUsers(){
    setTimeout(()=>{
        let data = '用户数据'
        iterator.next(data)
    },1000)
}
function getOrders(){
    setTimeout(()=>{
        let data = '订单数据'
        iterator.next(data)
    },1000)
}
function getGoods(){
    setTimeout(()=>{
        let data = '商品数据'
        iterator.next(data)
    },1000)
}

function * gen(){
    let users = yield getUsers()
    console.log(users);
    let orders = yield getOrders()
    console.log(orders);
    let goods = yield getGoods()
    console.log(goods);

}
let iterator = gen()
iterator.next()
```

