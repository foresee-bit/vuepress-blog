---
title: TypeScript学习笔记
date: 2022-08-14
tags:
 - TS
categories:
 -  TypeScript
---

## 基础

### void 和 undefined 和 null 最大的区别

undefined和null是所有类型的子类型。也就是说undefined类型的变量，可以赋值给string类型的变量。

```ts
let test:undefined = undefined
let num2:string = "1"
num2 = test
```

### any和unkonwn

声明变量的时候没有指定任意类型默认为any。

TypeScript 3.0中引入的 unknown 类型也被认为是 top type ，但它更安全。与 any 一样，所有类型都可以分配给unknown,unknow unknow类型比any更加严格当你要使用any 的时候可以尝试使用unknow。

*unknown可赋值对象只有unknown 和 any*