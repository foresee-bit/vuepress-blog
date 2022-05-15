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
        * `object` : 任意对象
        * `function` ： 一种特别的对象（可以执行）
        * `array` ： 一种特别的对象（数值下标，内部数据是有序的）
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

