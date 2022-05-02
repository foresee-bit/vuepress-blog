---
title: TypeScript学习笔记
date: 2022-05-02
---

# typescript学习笔记

笔记参考 [TypeScript入门教程](https://ts.xcatliu.com/introduction/get-typescript.html)



## 简介

### 关于TypeScript

TypeScript是JavaScript的一个超集，主要提供类型系统和对ES6的支持。它的第一个版本发布于 2012 年 10 月，不仅在 Microsoft 内部得到广泛运用，而且 Google 开发的 [Angular](https://angular.io/) 从 2.0 开始就使用了 TypeScript 作为开发语言，[Vue](https://vuejs.org/) 3.0 也使用 TypeScript 进行了重构。



### 什么是TypeScript

#### ==1. TypeScript是静态类型==

* TypeScript在运行前需要先编译为JavaScript,而在编译阶段就会进行类型检查。
* JavaScript是解释性语言，没有编译阶段，所以是动态类型。

一段完整的TypeScript代码如下：

```typescript
let foo: number = 1;
foo.split(' ');  //Property 'split' does not exist on type 'number'.
//编译时会报错
```

#### ==2. TypeScript是弱类型==

类型系统按照是否允许隐式类型转换来分类，可以分为强类型和弱类型。

以下这段代码不管是在 JavaScript 中还是在 TypeScript 中都是可以正常运行的，运行时数字 `1` 会被隐式类型转换为字符串 `'1'`，加号 `+` 被识别为字符串拼接，所以打印出结果是字符串 `'11'`。

### 安装TypeScript

全局安装

```typescript
npm install -g typescript
```

编译一个TypeScript文件

```typescript
tsc hello.ts
```

## 基础

### 1. 原始数据类型

JavaScript 的类型分为两种：原始数据类型（[Primitive data types](https://developer.mozilla.org/en-US/docs/Glossary/Primitive)）和对象类型（Object types）。

原始数据类型包括：布尔值、数值、字符串、`null`、`undefined` 以及 ES6 中的新类型 [`Symbol`](http://es6.ruanyifeng.com/#docs/symbol) 和 ES10 中的新类型 [`BigInt`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/BigInt)。

### 2. 任意值

任意值（Any）用来表示允许赋值为任意类型。

### 3. 类型推断

如果没有明确的指定类型，那么 TypeScript 会依照类型推论（Type Inference）的规则推断出一个类型。

以下代码虽然没有指定类型，但是会在编译的时候报错：

```typescript
let myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
// index.ts(2,1): error TS2322: Type 'number' is not assignable to type 'string'.
```

等价于：

```typescript
let myFavoriteNumber: string = 'seven';
myFavoriteNumber = 7;
// index.ts(2,1): error TS2322: Type 'number' is not assignable to type 'string'.
```

### 4. 联合类型

联合类型（Union Types）表示取值可以为多种类型中的一种。

联合类型使用 `|` 分隔每个类型。

```typescript
let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```

这里的 `let myFavoriteNumber: string | number` 的含义是，允许 `myFavoriteNumber` 的类型是 `string` 或者 `number`，但是不能是其他类型。

当TypeScript不确定一个联合类型的变量到底哪个类型的时候，我们只能访问此联合类型的所有类型的所有类型里**共有的属性或方法**。

### 5. 对象的类型——接口

