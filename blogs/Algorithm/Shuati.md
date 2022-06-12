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

# 数组的应用

## 两数求和问题

真题描述： 给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那两个整数，并返回他们的数组下标。

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



## 合并两个有序数组

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

## 三数求和问题

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

