---
title: 前端算法与数据结构面试-笔记
date: 2022-06-09
tags:
 - algorithm
categories:
 - Algorithm
---

# 数组

## 数组的创建 new Array()   fill

```js
const arr = [1,2,3,4] //方式1
```

```js
const arr = new Array() //构造函数创建  一般方法
//等同于 
const arr = []
//指定长度
const arr = new Array(7)
```

`fill`方法 : 创建一个长度确定，每一个元素值也确定的数组

```js
const arr = (new Array(7)).fill(1)
```

## 数组的访问和遍历

访问数组元素，指定索引

```js
arr[0]
```

遍历数组

### 1. for循环

```js
const len = arr.length
for(let i = 0; i < len; i++){
    console.log(arr[i],i)
}
```

### 2. forEach方法

通过取`forEach`方法中传入函数的第一个入参和第二个入参，可以取到数组每个元素的值及其对应索引。

```js
arr.forEach((item,index)=>{
    console.log(item,index)
})
```

### 3. Map方法

`map`方法会根据传入的函数逻辑对数组中每个元素进行处理，进而返回一个全新的数组。

当我们需要对数组内容做批量修改、同时修改的逻辑高度一致时，可以调用`map`。

```js
const newArr = arr.map((item,index)=>{
    console.log(item,index)
    return item + 1
})
```

## 二维数组

```js
const arr = [
  [1,2,3,4,5],
  [1,2,3,4,5],
  [1,2,3,4,5],
  [1,2,3,4,5],
  [1,2,3,4,5]
]
```

### fill的局限性

```js
const arr = (new Array(7)).fill([])
arr[0][0] = 1
```

当修改某一个数值时，会将整个一整列的元素都设为了1。

当你给 fill 传递一个入参时，**如果这个入参的类型是引用类型，那么 fill 在填充坑位时填充的其实就是入参的引用**。

### 二维数组的初始化



```js
const len = arr.length
for(let i = 0; i < len;i++){
    arr[i] = []
}
```

### 二维数组的访问

访问二维数组和访问一维数组差别不大，需要的是两层循环。

```js
// 缓存外部数组的长度
const outerLen = arr.length
for(let i=0;i<outerLen;i++){
    const innerLen = arr[i].length
    for(let j = 0;j < innerLen;j++){
        console.log(arr[i][j],i,j)
    }
}
```

## 数组中增加元素的三种方法

### unshift方法 -添加元素到数组的头部

```js
const arr = [1,2]
arr.unshift(0)
```

### push 方法 - 添加元素到数组的尾部

```js
const arr = [1,2]
arr.push(3)
```

### splice 方法 - 添加元素到数组的任何位置 (3参数)

```js
const arr = [1,2]
arr.splice(1,0,3)
//在这个例子里，我们就指明了从 arr[1] 开始，删掉 0 个元素，并且在索引为1的地方新增了值为3的元素。
```

### splice 方法 （2参数 - 删除）

第一个入参是起始的索引值，第二个入参表示从起始索引开始需要删除的元素个数。

## 数组中删除元素的三种

### shift 方法- 删除数组头部的元素

```js
const arr = [1,2,3]
arr.shift() //[2,3,]
```

### pop 方法-删除数组尾部的元素

```js
const arr = [1,2,3]
arr.pop() // [1,2]
```

### spilce 方法 - 如上









