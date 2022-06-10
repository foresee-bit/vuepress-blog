---
# navbar: true
sidebar: true
title: 前端算法与数据结构面试-笔记
date: 2022-06-09
tags:
 - algorithm
categories:
 - Algorithm
---

参考：掘金小册—[前端算法与数据结构面试：底层逻辑解读与大厂真题训练](https://juejin.cn/book/6844733800300150797)

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

## 栈（Stack） — 只用pop和push完成增删的“数组"

栈是一种后进先出(LIFO,Last In First Out)的数据结构。

可以看做只能用`push`和`pop`方法的数组。

## 队列 (Queue) - 只用push和shift完成增删的"数组"

队列是一种先进先出（FIFO，First In First Out）的数据结构。

整个过程只涉及了数组的 `push` 和 `shift` 方法。

# 链表

链表中的节点，允许散落在内存空间的各个角落里， 而内存是连续空间。

![image-20220610212506175](http://cdn.yangdw.cn/img/image-20220610212506175.png)

在链表中，每一个节点的结果都包括了两部分的内容：数据域和指针域。

### 链表节点的创建

```js
function ListNode(val){
    this.val  = val;
    this.next = null;
}
```

在使用构造函数创建结点时，传入 `val` （数据域对应的值内容）、指定 `next` （下一个链表结点）即可：

```js
const node = new ListNode(1)
node.next = new ListNode(2)
```

就创建出了一个数据域值为1，next 节点数据域值为2的链表结点。

![image-20220610220852348](http://cdn.yangdw.cn/img/image-20220610220852348.png)

### 链表元素的添加

1. 尾部添加

    * 直接改变尾结点的next指向

2. ==节点中部添加（重要）==

    * 需要变更的是前驱节点和目标节点的next指针指向

    * ![image-20220610221148633](http://cdn.yangdw.cn/img/image-20220610221148633.png)

    * ```js
        //如果目标节点不存在的话，需要手动创建
        const node3 = new ListNode(3)
        //把node3的next指针指向node2（即node1.next）
        node3.next = node1.next
        //把node1的next指针指向node3
        node1.next = node3
        ```

### 链表元素的删除

只需要让它的前驱节点node1的next指针跳过它，指向node3。

![image-20220610221517655](http://cdn.yangdw.cn/img/image-20220610221517655.png)

## 

```js
// 利用 node1 可以定位到 node3
const target = node1.next  
node1.next = target.next
```

# 树结构

![image-20220610223356961](http://cdn.yangdw.cn/img/image-20220610223356961.png)

## 重点概念

* 树的层次计算： 根节点所在的那一层为第一层，子节点为第二层
* 节点和树的高度计算：叶子节点高度记为1，每向上一层高度就加1，树种节点的最大高度即为树的高度
* "度"的概念：一个节点开叉出去多少个字树，被记为节点的“度”。上图中，根节点的“度"为3
* “叶子节点”：叶子节点就是度为0的节点

## 二叉树

二叉树是指满足一下要求的树：

* 它可以没有根节点，作为一颗空树存在
* 如果不是空树，必须由**根节点、左子树和右子树组成，且左右字树都是二叉树**

![image-20220610224443047](http://cdn.yangdw.cn/img/image-20220610224443047.png)

**二叉树不能被简单定义为每个结点的度都是2的树**。普通的树并不会区分左子树和右子树，但在二叉树中，左右子树的位置是严格约定、不能交换的，也就意味着 B 和 C、D 和 E、F 和 G 是不能互换的。

### 二叉树的编码实现

在定义二叉树构造函数时，我们需要把左侧子结点和右侧子结点都预置为空：

```js
// 二叉树节点的构造函数
function TreeNode(val){
    this.val = val
    this.left = this.right = null
}
```

新建一个二叉树结点

```js
const node = new TreeNode(1)
```

