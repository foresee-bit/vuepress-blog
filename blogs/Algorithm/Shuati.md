---
# navbar: true
sidebar: true
title: 刷题-数组
date: 2022-06-12
tags:
 - Array
categories:
 - Algorithm
---

## 数组的应用

### 两数求和问题（1）

真题描述： 给定一个整数数组 `nums` 和一个目标值 `target`，请你在该数组中找出和为目标值的那两个整数，并返回他们的数组下标。

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

---

解法1：两层循环遍历

解法2：将求和转换为求差问题，通过增加一个`Map`来记录已经遍历过的数字及其对应的索引值。每遍历到一个新的数字时，都回到Map里去查询target与该数的差值是否已经出现过。

api: Map.has()      Map.get()  Map.set()

编码实现：

```js
var twoSum = function(nums, target) {
    // 对象解法
    // const diffs = {}
    // const len = nums.length

    // for (let i = 0; i < len; i++){
    //     if (diffs[target - nums[i]] !== undefined) {
    //         return [diffs[target - nums[i]],i]
    //     }
    //     diffs[nums[i]] = i
    // }
    // return []
    // ES6-Map解法
    let myMap = new Map()
    for (let i = 0; i < nums.length; i++){
        another = target - nums[i]
        if (myMap.has(another)) {
            return [i,myMap.get(another)]
        }
        myMap.set(nums[i],i)
    }
    return []
};
```

### 合并两个有序数组（88）

真题描述：给你两个有序整数数组 nums1 和 nums2，请你将 nums2 合并到 nums1 中，使 nums1 成为一个有序数组。

```
输入：nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
输出：[1,2,2,3,5,6]
解释：需要合并 [1,2,3] 和 [2,5,6] 。
合并结果是 [1,2,2,3,5,6] ，其中斜体加粗标注的为 nums1 中的元素。
```

标准解法： 双指针法

首先定义两个指针，各指向两个数组生效部分的尾部

![image-20220612214321633](http://cdn.yangdw.cn/img/image-20220612214321633.png)

每次只对指针所指的元素进行比较，取其中较大的元素，把它从nums1的末尾往前面填补。

![image-20220612214435229](http://cdn.yangdw.cn/img/image-20220612214435229.png)

注意：由于nums1的有效部分和nums2并不一定是一样长的，我们还需要考虑其中一个提前到头的这种情况：

1. 如果提前遍历完的是nums1的有效部分，剩下的是num2。那么这时意味着nums1的头部空出来了，直接把nums2整个补到nums1前面去即可。
2. 如果提前遍历完的是nums2，剩下的是nums1。由于容器本身就是nums1，所以此时不必做任何额外的操作。

编码实现：

```js
var merge = function(nums1, m, nums2, n) {
    let i = m - 1, j = n - 1, k = m + n - 1
    while (i >= 0 && j >= 0) {
        if (nums1[i] >= nums2[j]) {
            nums1[k] = nums1[i]
            i--
            k--
        } else {
            nums1[k] = nums2[j]
            j--
            k--
        }
    }
    while (j >= 0) {
        nums1[k] = nums2[j]
        k--
        j--
    }
};
```

### 三数求和问题（15）

真题描述：给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有满足条件且不重复的三元组。

```
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
```

思路分析：

三数之和延续两数之和的思路，我们可以把求和问题变成求差问题—固定其中一个数，在剩下的数中寻找是否有两个数和这个固定数相加是等于0的。

双指针法用在涉及求和。比大小类的数组题目时，大前提是：该数组必须有序。因此，==第一步==是将数组排序

```js
nums = nums.sort((a,b)=> a - b)
```

==然后==对数组进行遍历，每次遍历到哪个数字就固定此数字，然后把左指针指向该数右边一位数字，右指针指向数组末尾。让左右指针从起点开始，向中间前进。

![image-20220612223906412](http://cdn.yangdw.cn/img/image-20220612223906412.png)

每次指针移动一次位置，就计算一下两个指针指向数字之和加上固定的那个数之后，是否等于0。根据情况分为三种情况：

* `=0`，将数组[nums[i],nums[j],nums[k]]压入数组中。  arr.push([])
* `>0`，说明右侧的数偏大，右指针左移
* `<0`，说明左侧的数偏小，左指针右移

同时需要注意==不重复==。

编码实现:

```js
const threeSum = function(nums){
    let res = []
    let len = nums.length
    nums = nums.sort((a,b)=> a - b)
    for(let i = 0;i < len - 2; i++){
        let j = i + 1
        let k = len - 1
        if(i > 0 && nums[i] === nums[i - 1]){
            continue
        }
        while(j < k){
            if(nums[i] + nums[j] + nums[k] > 0){
                k--
                while(j< k && nums[k] === nums[k + 1]){
                    k--
                }
            }else if (nums[i] + nums[j] + nums[k] < 0){
                j++
                while(j < k && nums[j] === nums[j - 1]){
                    j++
                }
            }else {
                res.push([nums[i],nums[j],nums[k]])
                j++
                k--
                while(j < k && nums[j] === nums[j - 1]){
                    j++
                }
                while(j < k && nums[k] === nums[k + 1]){
                    k--
                }
            }
        }
    }
    return res
}
```

## 链表的应用

### 合并两个有序链表（21）

==真题描述==：将两个有序链表合并为一个新的有序链表并返回。新链表是通过拼接给定的两个链表的所有结点组成的。 

![image-20220616165200857](http://cdn.yangdw.cn/img/image-20220616165200857.png)

```
输入：l1 = [1,2,4], l2 = [1,3,4]
输出：[1,1,2,3,4,4]
```

思路：处理链表的本质，是处理链表节点之间的指针关系。

编码实现：

```js
const mergeTwoLists = function(l1,l2){
    let head = new ListNode()
    let cur = head
    while(l1 && l2){
        if(l1.val <= l2.val){
            cur.next = l1
            l1 = l1.next
        }else {
            cur.next = l2
            l2 = l2.next
        }
        cur = cur.next
    }
    //处理链表不等长的情况
    cur.next = l1 !== null ? l1:l2
    return head.next
}
```

### 删除排序链表中的重复元素（83）

==真题描述==：给定一个排序链表，删除所有重复的元素，使得每个元素只出现一次。

![image-20220616170032283](http://cdn.yangdw.cn/img/image-20220616170032283.png)

```
输入：head = [1,1,2]
输出：[1,2]
```

编码实现：

```js
const deleteDuplicates = function(head){
    let cur = head
    while(cur !== null && cur.next !== null){
        if(cur.val === cur.next.val){
            cur.next = cur.next.next
        }else{
            cur = cur.next
        }
    }
    return head
}
```

### 删除排序链表中的重复元素-ii （82）

==真题描述==：给定一个排序链表，删除所有含有重复数字的结点，只保留原始链表中 没有重复出现的数字。

![image-20220616170442587](http://cdn.yangdw.cn/img/image-20220616170442587.png)

```
输入：head = [1,2,3,3,4,4,5]
输出：[1,2,5]
```

思路：与上题的异同：

相同点：删除重复元素。

不同点：只要一个元素发生重复，全部删除。

难点：无法定位到第一个节点的前驱节点，无法删除。

解决：增加一个空节点（哨兵节点）`dummy`，可以确保所有节点都有一个前驱节点。

编码实现：

```js
const deleteDuplicates = function(head){
    if(!head || !head.next){
        return head
    }
    let dummy = new ListNode()
    dummy.next = head
    let cur = dummy
    while(cur.next && cur.next.next){
        if(cur.next.val === cur.next.next.val){
            let val = cur.next.val
            while(cur.next && cur.next === val){
                cur.next = cur.next.next
            }
        }else {
            cur = cur.next
        }
    }
    return dummy.next
}
```

## 快慢指针与多指针—玩转链表

### 快慢指针—删除链表的倒数第N个节点（19）

==真题描述==：给定一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点。

![image-20220617175320953](http://cdn.yangdw.cn/img/image-20220617175320953.png)

==思路==：

难点在于这个倒数第N个节点如何定位。因为链表遍历不能从后往前走，因此`倒数第N个`可以转换为`正数第 len - n + 1`个。

快慢指针：首先两个指针`slow`和`fast`，全部指向链表的起始位—`dummy`节点：

![image-20220617180035346](http://cdn.yangdw.cn/img/image-20220617180035346.png)

快指针先触发！走上n步，在第n个节点处停住，然后快慢指针一起前进，当快指针前进到最后一个节点时，两个指针再一起停下来。

![image-20220617180200742](http://cdn.yangdw.cn/img/image-20220617180200742.png)

![image-20220617180220759](http://cdn.yangdw.cn/img/image-20220617180220759.png)

此时，慢指针所指的位置，就是倒数第n个节点的前一个节点。然后再将该节点指针指向改变，即可对倒数第n个节点进行删除。

![image-20220617180324778](http://cdn.yangdw.cn/img/image-20220617180324778.png)

```js
const removeNthFromEnd = function(head,n){
    let dummy = new ListNode()
    dummy.next = head
    let fast = dummy
    let slow = dummy
    while(n !== 0) {
        fast = fast.next
        n--
    }
    while(fast.next){
        fast = fast.next
        slow = slow.next
    }
    slow.next = slow.next.next
    return dummy.next
}
```

### 链表的反转（206）

==真题描述==：定义一个函数，输入一个链表的头结点，反转该链表并输出反转后链表的头结点。

![image-20220617215102566](http://cdn.yangdw.cn/img/image-20220617215102566.png)

```
输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]
```

思路：**处理链表的本质，是处理链表结点之间的指针关系**

编码实现：

```js
const reverseList = function(head){
    let pre = null
    let cur = head
    while(cur !== null){
        let next = cur.next
        cur.next = pre
        pre = cur
        cur = next
    }
    return pre
}
```

### 局部反转一个链表（92）

==真题描述==：给你单链表的头指针 `head` 和两个整数 `left` 和 `right` ，其中 `left <= right` 。请你反转从位置 `left` 到位置 `right` 的链表节点，返回 **反转后的链表** 。

![image-20220617215802304](http://cdn.yangdw.cn/img/image-20220617215802304.png)

```
输入：head = [1,2,3,4,5], left = 2, right = 4
输出：[1,4,3,2,5]
```

==思路==：局部反转可以使用之前的反转链表的操作，需要额外考虑的是局部链表两端的指向问题，即局部链表的前驱节点和后续节点需要一个指针进行缓存。

编码实现：

```js
const reverseBetween = function(head,left,right){
    let pre,cur,leftHead
    const dummy = new ListNode()
    dummy.next = head
    let p = dummy
    while(left > 0) {
        p = p.next
        left--
    }
    leftHead = p
    pre = p
    let start = left.next
    cur = p.next
    for(let i = left,i < right,i++){
        let next = cur.next
        cur.next = pre
        pre = cur
        cur = next
    }
    leftHead.next = pre
    start.next = cur
    return dummy.next
}
```

