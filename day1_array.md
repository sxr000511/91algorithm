
## 题目地址(989. 数组形式的整数加法)

https://leetcode-cn.com/problems/add-to-array-form-of-integer/

## 题目描述

```
对于非负整数 X 而言，X 的数组形式是每位数字按从左到右的顺序形成的数组。例如，如果 X = 1231，那么其数组形式为 [1,2,3,1]。

给定非负整数 X 的数组形式 A，返回整数 X+K 的数组形式。

 

示例 1：

输入：A = [1,2,0,0], K = 34
输出：[1,2,3,4]
解释：1200 + 34 = 1234


示例 2：

输入：A = [2,7,4], K = 181
输出：[4,5,5]
解释：274 + 181 = 455


示例 3：

输入：A = [2,1,5], K = 806
输出：[1,0,2,1]
解释：215 + 806 = 1021


示例 4：

输入：A = [9,9,9,9,9,9,9,9,9,9], K = 1
输出：[1,0,0,0,0,0,0,0,0,0,0]
解释：9999999999 + 1 = 10000000000


 

提示：

1 <= A.length <= 10000
0 <= A[i] <= 9
0 <= K <= 10000
如果 A.length > 1，那么 A[0] != 0
```

## 思路
模仿加法计算过程，通过商和余数完成进位

js里没整型数据，需要Math.floor()向下取整，要不然`/`得到的是小数

## 关键点
有几个边界条件要考虑

1. 输入[0] 23 或者  [0],10000---》数组短，数据长，不能在数组结束就跳出循环

```javascript
        const x = i >= 0 ? A[i] : 0
        const y = K != 0 ? K % 10 : 0
```
2. 输入 [2,1,5],806   ---》最高位有进位，不能跳出循环

```javascript
 if (carry) res.push(carry)
```
3. 循环的条件是：(i >=0 || K != 0)，K不为零用来处理数组短的情况
## 代码

- 语言支持：JavaScript

JavaScript Code:

```javascript

/**
 * @param {number[]} num
 * @param {number} k
 * @return {number[]}
 */
var addToArrayForm = function(A, K) {
    const res = []
    let i = A.length - 1, carry = 0
    while (i >=0 || K != 0) {
        //处理边界条件
        const x = i >= 0 ? A[i] : 0
        const y = K != 0 ? K % 10 : 0

        const sum = x + y + carry
        res.push(sum % 10)
        carry = Math.floor(sum / 10)

        i--
        K = Math.floor(K / 10)
    }
    //处理最高位进位
    if (carry) res.push(carry)
    return res.reverse()
};


```


**复杂度分析**

令 n 为数组长度。

- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$


