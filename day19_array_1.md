
## 题目地址(1. 两数之和)

https://leetcode-cn.com/problems/two-sum/

## 题目描述

```
给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

 

示例 1：

输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。


示例 2：

输入：nums = [3,2,4], target = 6
输出：[1,2]


示例 3：

输入：nums = [3,3], target = 6
输出：[0,1]


 

提示：

2 <= nums.length <= 104
-109 <= nums[i] <= 109
-109 <= target <= 109
只会存在一个有效答案

进阶：你可以想出一个时间复杂度小于 O(n2) 的算法吗？
```

## 思路

优化的常见思路是用空间换时间。

使用哈希表。内存复杂度变为 O(n)，时间复杂度则降至 O(n)【之前on2】

【哈希表的优化】

要对上述优化，就要深入细节了。比如有一个表超级大，但前两个元素就能够满足题目要求。在上述解法中，依然要建一个完整的哈希表，空间占用一点没省下来，理想解法是边查边存

## 关键点

-  

## 代码

- 语言支持：JavaScript

JavaScript Code:


1. 普通哈希表

```javascript

/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
const twoSum = function(nums, target) {
  const len = nums.length;

  // 使用哈希表存储当前值
  const map = new Map();
  for (let i = 0; i < len; i++) {
    map.set(nums[i], i);
  }

  for (let i = 0; i < len; i++) {
    const needNum = target - nums[i];

    if (map.has(needNum) && i !== map.get(needNum)) {
      return [i, map.get(needNum)]
    }
  }
}


```

2. 哈希表优化，边差边存

如果没查到就存起来等着以后用

```javascript
const twoSum = function(nums, target) {
  const len = nums.length;
  const map = new Map();

  for (let i = 0; i < len; i++) {
    const needNum = target - nums[i];

    if (map.has(needNum) && i !== map.get(needNum)) {
      return [i, map.get(needNum)];
    }

    // 边读边存
    map.set(nums[i], i);
  }
}

```

