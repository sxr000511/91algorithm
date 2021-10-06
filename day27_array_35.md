
## 题目地址(35. 搜索插入位置)

https://leetcode-cn.com/problems/search-insert-position/

## 题目描述

```
给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

请必须使用时间复杂度为 O(log n) 的算法。

 

示例 1:

输入: nums = [1,3,5,6], target = 5
输出: 2


示例 2:

输入: nums = [1,3,5,6], target = 2
输出: 1


示例 3:

输入: nums = [1,3,5,6], target = 7
输出: 4


示例 4:

输入: nums = [1,3,5,6], target = 0
输出: 0


示例 5:

输入: nums = [1], target = 0
输出: 0


 

提示:

1 <= nums.length <= 104
-104 <= nums[i] <= 104
nums 为无重复元素的升序排列数组
-104 <= target <= 104
```

## 思路

要求logn解决，用二分查找

## 关键点

-  

## 代码

- 语言支持：JavaScript

JavaScript Code:

```javascript

/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var searchInsert = function(nums, target) {

    // 二分查找

    let left = 0 ;
    let right = nums.length -1 ;
    let middle = Math.floor((left+right)/2);

    if ( target > nums[right]){
        return right+1
    }
    if( target <= nums[ left ]){
        return left 
    }

    while ( left !== right && left!==middle && right !== middle){

    if ( target > nums[right]){
        return right+1
    }
    if( target < nums[ left ]){
        return left 
    }

    if( nums[middle] > target ){
        right = middle
    }else if ( nums[middle] < target){
        left = middle
    }else {
        return middle
    }
     middle = Math.floor((left+right)/2);

}

return left + 1
    

};

```


**复杂度分析**

令 n 为数组长度。

- 时间复杂度：$O(logn)$
- 空间复杂度：$O(1)$


