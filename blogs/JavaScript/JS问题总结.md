---
title: JS问题总结
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

## 3. 类数组和数组的区别和转化

类数组：

1. 能够用索引值访问当前元素，并且能知道当前数组的长度
2. 不能使用数组的其他方法，诸如map，filter等，类数组一般在函数参数上使用较多，arguments就是一个类数组.

对象的常用方法：

```js
obj = {a:1, b: "1",c: true,d: {} }
for ... in ...  ---数组的常用遍历方法
hasOwnProperty  --- 返回一个布尔值，判断是否有该属性
toString        --- 判断数值变量的万能方法
defineProperty  --- Vue2实现数据双向绑定的核心算法
valueOf         --- 返回这个对象的原始值  //{a:1, b: "1",c: true,d: {} }
for ... of ...  --- 可以对具有迭代器的对象进行遍历，比如map,set,array等
keys, values    --- 分别返回由这个对象的属性和属性值组成的数组
```

数组的常用方法：

```js
常用的数组本身的操作方法
shift           --- 从头部删除元素  返回值为删去的元素
unshift         --- 从头部增加元素，没有返回值
pop             --- 从尾部删去元素，返回值为删除的元素
push            --- 从尾部增加元素，没有返回值
concat          --- 将多个数组合并为一个数组，返回值为合并的数组
splice          --- 可传入多个参数，返回值为被改变的数组，
					第一个参数为删除元素的起始位置，后面没有参数的话就是全部删除
                    第二个参数为删除元素的个数，如果超出删除的个数也是全部删除
                    第三个参数是在删除的位置上加上要加的值
                    [1,2].splice(0,1,2,2)  // [2,2,2]
reverse         --- 将数组改变排列顺序
sort            --- 将数组按照升序排列，可以通过传函数变为降序，[1,2,1].sort((a,b)=>b-a)
slice           --- 将原来数组中的一部分进行切分出来并作为返回值返回，
join            --- 将数组的所有元素连接成一个字符串并返回这个字符串，默认使用逗号,分割。
常用的数组遍历的方法
filter          --- 根据给出的回调函数遍历整个数组条件相符的元素 返回值为经过筛选后的数组
forEach         --- 对整个数组进行进行遍历 返回值为undefined
map             --- 对整个数组根据回调函数进行遍历并返回经过调用的元素组成的数组
every,some      --- 对整个数组进行条件判断是否符合某个条件，every是对整个数组的元素比较，一个不符					 合就是false , some是一个符合就全部符合 返回值是一个boolean值
reduce          --- 对整个数组进行累计操作，需要至少一个累计值，一个当前值
find            --- 寻找整个数组是否含有某个值,有则返回索引,没有返回undefined
valueOf         --- 寻找整个数组是否含有某个值，有则返回索引，没有返回-1
```

### 类数组和数组的互相转化

ES6

```js
// 1.
const newArr = [...arguments]
//2. 
Array.from()
```

ES5

```js
function toArray(s){
    var arr = []
    try {
        arr = Array.prototype.slice.call(s)
    }catch{
        for(var i = 0;i < s.length;i++){
            arr.push(s[i])
        }
    }
}
```

## 4.  this, bind、call、apply的区别

### 怎么改变this的指向

1. 使用ES6的箭头函数
2. 在函数内使用_this = this
3. 使用apply,call,bind
4. new 实例化一个对象

**this永远指向最后调用的它的那个对象**

`apply`: apply()方法调用一个函数，其具有一个指定的this值，以及作为一个数组（或类数组的对象）提供的参数

```js
var a ={
        name : "Cherry",
        fn : function (a,b) {
            console.log( a + b)
        }
    }

var b = a.fn;
b.apply(a,[1,2]) // 3
b.call(a,1,2) // 3
b.bind(a,1,2)() //3
```

 apply和call基本类型，只是传入的参数不同，apply传入的参数列表为数组。

bind和apply,call区别，是创建一个新的函数，需要手动调用。

## 5. new操作符的原理

```js
function Person(name) {
  this.name = name;
}

let obj = new Person("Jalenl");
```

对象实例的创建过程：

1. 在内存中创建一个新对象
2. 这个新对象内部的[[Prototype]]特性被赋值为构造函数的prototype属性
3. 构造函数内部的this被赋值为这个新对象(即this指向新对象)
4. 执行构造函数内部的代码（给新对象添加属性）
5. 如果构造函数返回对象，则返回该对象，否则，返回刚创建的新对象（空对象）

```js
function _new(constructor,...args){
    // 1.创建新对象
    let obj = {}
    // 2.将构造函数的原型绑定到新创建的对象实例
    obj.__proto__ = Object.create(constructor.prototype)
    // 3.调用构造函数并判断返回值
    let res = constructor.apply(obj,args)
    // 如果有返回值且返回值是对象类型，那么就将它作为返回值，否则返回新创建的对象
    return res instanceof Object ? res : obj
}
```

## 6. 闭包及其作用

### 定义

闭包是指有权访问另一个函数作用域中的变量的函数。创建一个闭包最常见的方式，就是在一个函数内部创建另一个函数。

### 作用

1. 在函数外部能够访问到函数内部的变量，通过使用闭包，可以通过在外部调用闭包函数，从而在外部访问到函数内部的变量。
2. 让已经运行结束的函数上下文中的变量对象继续留在内存中，因为闭包函数保留了这个变量对象的引用，所以这个变量对象不会被回收。

## 7. 原型和原型链

### 原型

在js中使用构造函数来新建一个对象的，每一个构造函数的内部都有一个prototype属性值，这个属性值是一个对象，这个对象包含了可以由该构造函数的所有实例共享的属性和方法。当使用构造函数新建一个对象后，在这个对象的内部将包含一个指针，这个指针指向构造函数的prototype属性对应的值。 ES5中新增了一个Object.getPrototypeOf()方法，可以通过这个方法获得对象的原型。

### 原型链

当我们访问一个对象的属性时，如果这个对象内部不存在这个属性，那么它就会去它的原型对象里找这个属性，这个原型对象又有做自己的原型，就这样一直找下去，这就是原型链。原型链的尽头一般来说是Object.prototype。

## 8. JS中的深浅拷贝

浅拷贝：指的是将一个对象的属性值复制给另一个对象，如果有的属性的值为引用类型的话，那么会将这个引用的				地址复制给对象，因此两个对象会有同一个引用类型的引用。

浅拷贝可以使用Object.assign和展开运算符来实现。

```js
// 法1
function shallowCopy(object){
    if(!object || typeof object !== 'object') return
    let newObject = Array.isArray(object)? []:{}
    for(let key in object){
        if(object.hasOwnProperty(key)){
            newObject[key] = object[key]
        }
    }
    return newObject
}
// 法2 
let newObject = Object.assign({},object)
//法3 拓展运算符
let newObject = {...object}
```

深拷贝：创建一个新的对象把原始对象的所有属性拷贝一份，并且引用类型的引用地址和内存时间都会被拷贝一份，重新分配内存空间，修改新对象不会影响原始对象。

```js
function deepCopy(object){
    if(!object || typeof object !== "object") return object
    let newObject = Array.isArray(object) ? [] : {}
    for(let key in object){
        if(object.hasOwnProperty(key)){
            newObject[key] = deepCopy(object[key])
        }
    }
    return newObject
}
```

## 9. 防抖和节流

函数防抖：是指事件被触发后n秒后再执行回调，如果在这n秒内事件又被触发，则重新计时。这可以使用在一些					点击请求的事件上，避免因为用户的多次点击向后端发送多次请求。

```js
functiohn debounce(fn,wait){
    var timer = null;
    return function(){
        var context = this,
        args = arguments
        if(timer){
            clearTimeout(timer)
            timer = null
        }
        timer = setTimeout(()=>{
            fn.apply(context,args)
        },wait)
    }
}
```

函数节流：是指规定一个单位时间，在这个单位时间内，只能有一次触发事件的回调函数执行，如有在同一个单位事件内某事件被触发多次，只有一次能生效。可以使用在scroll函数的事件监听，通过事件节流来降低事件调用的频率。

```js
function throttle(fn,delay){
    var preTime = Date.now()
    return function(){
        var context = this,
        	args = arguments,
        	nowTime = Date.now();
        if(nowTime - preTime >= delay){
            preTime = Date.now()
            return fn.apply(context,args)
        }
    }
}
```

