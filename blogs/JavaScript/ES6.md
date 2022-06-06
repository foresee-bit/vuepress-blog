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

![image-20220606151734824](E:\anzhuang\PicGo\pic\image-20220606151734824.png)

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



