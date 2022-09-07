---
title: JS—复习
time: 2022-09-07
tags:
 - JS
categories:
 -  JavaScript
---

## 1. Number数据类型详解

alert([value])输出时，会先进行字符串转换再输出，console.log()直接输出原始值。

JS中的数据类型

​	基本数据类型

​		number、string,boolean,null,undefined,symbol,bigint

​	引用数据类型

​		object({},[],/^$/), function...

number类型

​	1. Infinity 代指无穷大的值

	2. NaN(not a number) 不是一个有效数字，但属于number类型
 	3. isNaN(value)：检测一个值是否为非有效数字,如果value不是数字类型的值，则浏览器默认会把value转换为数字， “隐式转换”,基于 Number([value])实现。
 	4. Number([value]) 把其他数据类型转换为number数字类型
      	1. 字符串转换为数字 空字符串为0，如果字符串中出现任意一个非有效数字字符，结果为NaN
      	2. 布尔转换为数字, true=> 1; false => 0
      	3. null => 0  undefined => NaN
      	4. symbol值不能转换为数字
      	5. bigint可以转换为数字
      	6. 引用数据类型（对象|函数）转换为数字 [] => 0    {} => NaN
           	1. 首先获取[Symbol.toPrimitive]属性值
           	2. 如果没有的话，其次获取它的valueOf (原始值)
           	3. 如果还是没有原始值，最后把其转换为字符串toString，然后再转换为数字 Number

```
var num = Infinity
var num = 10 - "A"
NaN == NaN // false
```

需求：验证一个值是否为有效数字？

```
wrong:
if(num == NaN){} // num == NaN永远不会成立
right:
isNaN(value)
```

5. parseInt/parseFloat([value]) 把其他数据类型转换为数字类型
    1. 需要保证[value]是一个字符串，如果不是则首先隐式的把其转换为字符串 [value].toString()
    2. 从字符串左侧第一个字符开始查找，把找到的有效数字字符最后转换为数字（遇到非有效数字停止）

```
// Number和parseInt的区别
Number("12px") // NaN
parseInt("12px") // 12
parseInt("12.5px") // 12
parseFloat("12.5px") // 12.5
Number("") // 0
parseInt("") // NaN
```

6. [number value].toFixed([n]):保留小数点后面N位

## 2. String字符类型详解



