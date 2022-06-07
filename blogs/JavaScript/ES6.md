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

console.log(song); 
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

## 1. 数组的解构

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
console.log(str,typeof str);// 我也是一个字符串   string
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

## 3. 变量拼接 ${}

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

## 2. 不能作为构造函数实例化对象

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

ES6中 rest参数 返回一个==数组==，可以调用数组的方法

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

# Promise

`Promise`是ES6引入的异步编程的新解决方案。语法上Promise是一个构造函数，用来封装异步操作并可以获取其成功或失败的结果。

1. Promise构造函数：Promise(excutor){}
2. Promise.prototype.then 方法
3. Promise.prototype.catch方法

```js
const p = new Promise(function(resolve,reject){
    setTimeout(function(){
        // let data = '数据库中的用户数据'
        // resolve(data)
        let err = '数据读取失败'
        reject(err)
    },1000)
})
p.then(function(value){
    console.log(value);
},function(reason){
    console.log(reason);
})
```

## 使用Promise封装读取文件

`node`的原始方法  fs

```js
const fs = require('fs')
fs.readFile('./为学.md', (err, data) => {
    if(err) throw err
    console.log(data.toString());
})
```

`Promise`封装

```js
const p = new Promise(function (resolve, reject) {
    fs.readFile("./为学.md", (err, data) => {
        if(err) reject(err)
        resolve(data)
    })
})
p.then(function (vaule) {
    console.log(vaule.toString());
    // console.log(vaule); //读取的是Buffer，<Buffer 61 73 64 61 64 73 61 64 61 73 64 61 73 64 61...>
}, function (reason) {
    console.log('读取失败');
})
```

# Set—集合介绍与API 

```js
let s = new Set()
let s2 = new Set(['大事','小事','好事','坏事','小事'])
console.log(s2.size);
s2.add('喜事儿')
s2.delete('坏事')
console.log(s2.has('好事'));
console.log(s2);
// s2.clear()
console.log(s2);
for(let v of s2){
    console.log(v);
}
```

数组去重   求交集   并集 差集

```js
let arr = [1, 2, 3, 4, 5, 4, 3, 2, 1]
//1.数组去重
let result = [...new Set(arr)]
let result1 = new Set(arr)
console.log(result1);
console.log(result);
//2.交集
let arr2 = [4, 5, 6, 5, 6]
// let result2 = [...new Set(arr2)].filter(item =>{
//     let s2 = new Set(arr2)
//     if(s2.has(item)){
//         return true
//     }else{
//         return false
//     }
// })
let result3 = [...new Set(arr)].filter(item => new Set(arr2).has(item))
console.log(result3);
// 3.并集
let union = [...new Set([...arr, ...arr2])]
console.log(union);
// 4. 差集
let diff = [...new Set(arr)].filter(item => !(new Set(arr2).has(item)))
console.log(diff);
```

# Map结构

ES6 提供了`Map`数据结构。它类似于对象，也是键值对的集合。但是"键"的范围不限于字符串，各种类型的值（包括对象）都可以当做键。Map也实现了iterator接口，所以可以使用【拓展运算符】和`for  ... of...`进行遍历。`Map`的属性和方法。

1. `size`  返回`Map`的元素个数
2. `set`  增加一个新元素，返回当前`Map`
3. `get`   返回键名对象的键值
4. `has` 检测Map中是否包含某个元素，返回`boolean`
5. `clear `  清空集合，返回`undefined`

```js
let m = new Map()
m.set('name','BIT')
m.set('change',function(){
    console.log('我们可以改变你!');
})
let key = {
    school: '理工'
}
m.set(key,['北京','上海','深圳'])
console.log(m.size);
console.log(m);
// m.delete('name')
console.log(m.get(key));
m.clear()
```

# class 类

ES6提供了更接近传统语言的写法，引入了`Class`（类）这个概念，作为对象的模板。通过class关键字，可以定义类。

ES5的实例化

```js
// ES5的实例化
function Phone(brand,price){
    this.brand = brand
    this.price = price
}
Phone.prototype.call = function(){
    console.log('我可以打电话！');
}
let Huawei = new Phone('华为',5999)
Huawei.call()
console.log(Huawei);
```

ES6—class

```js
class Shouji {
    constructor(brand,price){
        this.brand = brand
        this.price = price
    }
    call(){
        console.log("我可以打电话");
    }
}
let onePlus = new Shouji("1+",1999)
console.log(onePlus);
```

## 构造函数继承

`ES5`

```js
// ES5
function Phone(brand,price){
    this.brand = brand
    this.price = price
}
Phone.prototype.call = function(){
    console.log("我可以打电话");
}
function SmartPhone(brand,price,color,size){
    Phone.call(this,brand,price)
    this.color = color
    this.size = size
}
SmartPhone.prototype = new Phone
SmartPhone.prototype.constructor = SmartPhone
// 声明子类的方法
SmartPhone.prototype.photo = function(){
    console.log("我可以拍照");
}
SmartPhone.prototype.playGame = function(){
    console.log("我可以玩游戏");
}
const chuizi = new SmartPhone('锤子',2499,'黑色','5.5inch')
console.log(chuizi);
```

`ES6` class类的继承

```js
class Phone {
    constructor(brand, price) {
        this.brand = brand
        this.price = price
    }
    call() {
        console.log("我可以打电话");
    }
}
class SmartPhone extends Phone {
    constructor(brand, price, color, size) {
        super(brand, price)
        this.color = color
        this.size = size
    }
    photo(){
        console.log("拍照");
    }
    playGame(){
        console.log("玩游戏");
    }
}
const xiaomi = new SmartPhone('小米',444,'Black','5.5inch')
console.log(xiaomi);
```

## get 和 set 

```js
class Phone{
    get price(){
        console.log("价格属性被读取了");
        return 'aaa'
    }
    set price(newVal){
        console.log('价格属性被修改了');
        console.log(newVal);
    }
}
let s = new Phone()
s.price = 'free'
```

# ES6的数值拓展

## 1. `Number.EPSILON`是JS标识的最小精度

```js
function equal(a,b){
    if(Math.abs(a-b) < Number.EPSILON){
        return true
    }else {
        return false
    }
}
console.log(0.1 + 0.2 === 0.3);//false
console.log(equal(0.1 + 0.2, 0.3));//true
```

## 2. `Number.isFinite` 检测一个值是否为有限数

## 3. `Number.isNaN` 检测一个数是否为`NaN`

## 4. `Number.parseInt`   `Number.parseFloat`

```js
console.log(Number.parseInt('512321321lobv'));//512321321
console.log(Number.parseFloat('3.421312神奇'));//3.421312
```

## 5. `Number.isInteger` 判断一个数是否为整数

## 6. `Math.trunc` 将数字的小数部分抹掉

## 7. `Math.sign`  判断一个数到底为正数  负数  还是 零

```js
console.log(Math.sign(100));// 1
console.log(Math.sign(0)); //  0
console.log(Math.sign(-100));// -1
```

# ES6的对象方法拓展

## 1. `Object.is`判断两个值是否完全相等

```js
console.log(Object.is(120,120)); // true
console.log(Object.is(NaN,NaN)); // true
console.log(NaN === NaN); // false
```

## 2. `Object.assign` 对象的合并

```js
const config1 = {
    host:'localhost',
    port:3306,
    name:'root',
    pass:'root',
    test:'test'
}
const config2 = {
    host:'http://127.0.0.1',
    port:33061,
    name:'root1',
    pass:'root1',
}
console.log(Object.assign(config1,config2));
//{host: 'http://127.0.0.1', port: 33061, name: 'root1', pass: 'root1', test: 'test'}
```



# ES6模块

## 暴露方法

分别暴露

```js
export let school = 'BIT'
export function teach() {
    console.log("我们可以教给你开发技能！");
}
```

统一暴露

```js
let school = 'BIT'
function teach() {
    console.log("我们可以教给你开发技能！");
}
export {school,teach}
```

默认暴露

```js
export default {
    school: 'BIT',
    change: function () {
        console.log("我们可以改变你！");
    }
}
```

## 引入方法

通用引入

```js
import * as m1 from './m1.js'
```

结构赋值形式

```js
import {school,teach} from './m1.js'
import {school as BIT,findJob} from './m2.js'
import {default as m3} from './m3.js' // 针对默认暴露
```

简便形式  针对默认暴露

```js
import m3 from './m3.js'
```

# `includes`方法

```js
const mingzhu = ['西游记','红楼梦','三国演义','水浒传']
console.log(mingzhu.includes('西游记')) // true
console.log(mingzhu.includes('天工开物')) // false
```

# `async` 和 `await`

`async`和`await`两种语法结合可以让异步代码像同步代码一样。

1. `async`函数的返回值为`promise`对象
2. `promise`对象的结构由`async`函数执行的返回值决定