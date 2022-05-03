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

在TypeScript中，我们使用接口(Interfaces)来定义对象的类型。

TypeScript中的接口是一个非常灵活的概念，除了可用于==对类的一部分行为进行抽象==以外，也常用语对【对象的形状(Shape)】进行描述。

```typescript
interface Person {
    name: string;
    age: number;
}
let tom: Person = {
    name: 'Tom',
    age: 25
};
```

#### 可选属性

有时我们希望不要完全匹配一个形状，那么可以用可选属性 ? ,可选属性的含义是该属性可以不存在。

这时**仍然不允许添加未定义的属性**：

```typescript
interface Person {
    name: string;
    age?: number;
}
let tom: Person = {
    name: 'Tom'
};
```

#### 任意属性

如果希望一个接口允许有任意的属性，可以使用如下方式：

```typescript
interface Person {
    name:string;
    age?:number;
    [propName: string]: any;
}
let tom: Person = {
    name: 'Tom',
    gender: 'male'
}
// index.ts(3,5): error TS2411: Property 'age' of type 'number' is not assignable to string index type 'string'.
// index.ts(7,5): error TS2322: Type '{ [x: string]: string | number; name: string; age: number; gender: string; }' is not assignable to type 'Person'.
//   Index signatures are incompatible.
//     Type 'string | number' is not assignable to type 'string'.
//       Type 'number' is not assignable to type 'string'.
```

需要注意的是，**一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集**：

上例中，任意属性的值允许是 `string`，但是可选属性 `age` 的值却是 `number`，`number` 不是 `string` 的子属性，所以报错了。

一个接口中只能定义一个任意属性。如果接口中有多个类型的属性，则可以在任意属性中使用联合类型：

```typescript
interface Person {
    name: string;
    age?: number;
    [propName: string]: string | number;
}
let tom: Person = {
    name: 'Tom',
    age: 25,
    gender: 'male'
};
```

#### 只读属性

有时候我们希望对象中的一些字段只能在创建的时候被赋值，那么可以用 `readonly` 定义只读属性：

```typescript
interface Person {
    readonly id: number;
    name: string;
    age?: number;
    [propName: string]: any;
}
let tom: Person = {
    id: 89757,
    name: 'Tom',
    gender: 'male'
};
tom.id = 9527;
// index.ts(14,5): error TS2540: Cannot assign to 'id' because it is a constant or a read-only property.
```

**注意，只读的约束存在于第一次给对象赋值的时候，而不是第一次给只读属性赋值的时候**：

### 6. 数组的类型

在TypeScript中，数组类型有多种定义方式，比较灵活。

#### 类型 + 方括号 表示法

```typescript
let fibonacci: number[] = [1,2,3,4,5];
```

数组的一些方法的参数也会根据数组在定义时约定的类型进行限制：

```typescript
let fibonacci: number[] = [1,2,3,4,5];
fibonacci.push('8');
// Argument of type '"8"' is not assignable to parameter of type 'number'.
```

#### 数组泛型

我们也可以使用数组泛型(Array Generic) 来表示数组：

```typescript
let fibonacci: Array<number> = [1,2,3,4,5];
```

#### 用接口表示数组

```typescript
interface NumberArray {
    [index: number] : number;
}
let fibonacci: NumberArray = [1,2,3,4,5];
```

虽然接口也可以用来描述数组，但是我们一般不会这么做，因为这种方式比前两种方式复杂多了。

不过有一种情况例外，那就是它常用来表示**类数组**。

#### 类数组

类数组（Array-like Object）不是数组类型，比如 `arguments`:

```typescript
function sum() {
    let args: number[] = arguments;
}
// Type 'IArguments' is missing the following properties from type 'number[]': pop, push, concat, join, and 24 more.
```

上例中，`arguments` 实际上是一个类数组，不能用普通的数组的方式来描述，而应该用接口：

```typescript
function sum() {
    let args: {
        [index: number]: number;
        length: number;
        callee: Function;
    } = arguments;
}
```

事实上常用的类数组都有自己的接口定义，如 `IArguments`, `NodeList`, `HTMLCollection` 等：

```typescript
function sum() {
    let args: IArguments = arguments;
}
```

其中 `IArguments` 是 TypeScript 中定义好了的类型，它实际上就是：

```typescript
interface IArguments {
    [index: number]: any;
    length: number;
    callee: Function;
}
```

#### any在数组中的应用

一个比较常见的做法是用`any`表示数组中允许出现任意类型：

```typescript
let list: any[] = ['xcatliu', 25, { website: 'http://xcatliu.com' }];
```

