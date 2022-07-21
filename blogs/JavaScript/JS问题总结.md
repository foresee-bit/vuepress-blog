---
title: ES6
time: 2022-7-21
tags:
 - JS
categories:
 -  JavaScript
---

## 1. 原始值和引用值类型及区别、内存图

栈：原始数据类型（Undefined,Null,Boolean,Number,String,Symbol,BigInt）

堆：引用数据类型（对象、数组、函数）

原始数据：

![image-20220721234037737](http://cdn.yangdw.cn/img/image-20220721234037737.png)

引用数据：

![image-20220721234156511](http://cdn.yangdw.cn/img/image-20220721234156511.png)

区别：

1. 声明变量时不同的内存分配
    * 原始值：直接存储在栈(stack)中，因为占据空间固定，便于迅速查找
    * 引用值：存储在堆(heap)中的对象，而存储在变量处的值是一个指针，这个变量存储在栈中，指向堆中的内存地址。因为引用值大小会改变，存储在栈中会降低查寻速度。
2. 复制变量时的不同
    * 原始值：将原始值的副本复制给新变量，之后这两个变量完全独立。
    * 引用值：将保存着对象内存地址的变量复制给另一个变量，即两个变量指向同一个对象。
3. 参数传递的不同
    * ECMAScript中所有函数的参数都是按值传递的。
    * 但是原始值把值传递给参数，之后参数和这个变量互不影响
    * 引用值传递的是内存地址，也就是说函数内部对这个参数的修改会体现在外部。

## 2. 判断数据类型typeof、instanceof、Object.prototype.toString.call()、constructor

### 1. `Object.prototype.toString.call()`方法，可以判断任何类型，返回的是一个字符串

![image-20220722000521190](http://cdn.yangdw.cn/img/image-20220722000521190.png)

如果想获取后面指定的部分String的话，可以使用字符串的slice方法。

```js
function myClass(target){
    return Object.prototype.toString.call(target).slice(8,-1)
}
```

### 2. `typeof()`方法,用来判断准确判断基本类型（除了null），返回字符串

typeof()在能够准确判断基本类型和function。但是判断null和引用类型（对象，数组，正则等）都会返回'object'

因为数据存储在计算机内部时是转换为64位二进制数，引用类型的二进制的前三位为000，null转换为二进制时全为0。同时typeof()判断的原理是判断前三位二进制数，如果前三位为0，则认为是对象，再去查看是否实现了call()方法，如果实现了则返回'function'，没有实现则返回'object'

所以**typeof不能准确判断引用类型**

### 3. `instanceof()`方法，判断对象类型，返回布尔值

instanceof()的原理是通过判断对象的原型中是否能找到同类型的prototype。

![image-20220722001543598](http://cdn.yangdw.cn/img/image-20220722001543598.png)

原型链

![image-20220722002422995](http://cdn.yangdw.cn/img/image-20220722002422995.png)

模拟手写一个instanceof()

```js
function myinstanceof(left,right){
    let prototype = right.prototype
    let __proto__ = left.__proto__
    while(true){
        if(__proto__ === null){
            return false
        }
        if(__proto__ === prototype){
            return true
        }
        __proto__ = __proto__.__proto__
    }
}
```

### 4. constructor,判断对象，返回布尔值

原理：每一个实例对象都可通过constructor来访问它的构造函数，其实也是根据原型链原理。

```js
'5'.__proto__.constructor === String // true
[5].__proto__.constructor === Array // true
```

```js
undefined.__proto__.constructor // Cannot read property '__proto__' of undefined

null.__proto__.constructor // Cannot read property '__proto__' of undefined
```

但是，由于undefined和null是无效对象，没有constructor属性，所以无法进行判断。