---
title: JS高级
time: 2022-5-15
---

## 数据类型

1. 分类
    1. 基本类型
        * `string` :  任意字符串
        * `number`： 任意数字
        * `boolean`： true / false
        * `undefined` : undefined
        * `null` : null
    2. 对象类型
        * `Object` : 任意对象
        * `function` ： 一种特别的对象（可以执行）
        * `Array` ： 一种特别的对象（数值下标，内部数据是有序的）
2. 判断
    * `typeof`
        * 可以判断： undefined / number / string / boolean
    * `instanceof` : 判断对象的具体类型
    * `===` 
        * 可以判断： undefined, null

==案例==： undefined 是一个值。 'undefined' 是一个字符串，表示undefined这个数据类型。

```javascript
var a;
console.log(a,typeof a, typeof a === 'undefined', a === undefined)
// undefined  'undefined'  true  true
```

==Question==:

1. undefined 与 null 的区别？
    * undefined 代表定义未赋值
    * null 代表定义并赋值了,只是值为null
2. 什么时候给变量赋值为null呢？
    * 初始赋值，表明将要赋值为对象
    * 结束前，让对象成为垃圾对象（被垃圾回收器回收）
3. 严格区别变量类型与数据类型？
    * 数据的类型
        * 基本类型
        * 对象类型
    * 变量的类型（变量内存值的类型）
        * 基本类型：保存就是基本类型的数据
        * 引用类型：保存的地址值

### 数据 变量 内存

==Question==:

1. 什么是数据？
    * 存储在内存中代表特定信息的东西，本质上是0101...
2. 什么是内存？
    * 内存条通电后产生的用来可存储数据的空间（临时的） 
3. 什么是变量？
    * 可以变化的量，由变量名和变量值组成
    * 每个变量都对应一块小内存，变量名用来查找对应的内存，变量值就是内存中保存的数据