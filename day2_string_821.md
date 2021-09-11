## 题目地址(821. 字符的最短距离)

https://leetcode-cn.com/problems/shortest-distance-to-a-character/

## 题目描述

```
给你一个字符串 s 和一个字符 c ，且 c 是 s 中出现过的字符。

返回一个整数数组 answer ，其中 answer.length == s.length 且 answer[i] 是 s 中从下标 i 到离它 最近 的字符 c 的 距离 。

两个下标 i 和 j 之间的 距离 为 abs(i - j) ，其中 abs 是绝对值函数。

 

示例 1：

输入：s = "loveleetcode", c = "e"
输出：[3,2,1,0,1,0,0,1,2,2,1,0]
解释：字符 'e' 出现在下标 3、5、6 和 11 处（下标从 0 开始计数）。
距下标 0 最近的 'e' 出现在下标 3 ，所以距离为 abs(0 - 3) = 3 。
距下标 1 最近的 'e' 出现在下标 3 ，所以距离为 abs(1 - 3) = 2 。
对于下标 4 ，出现在下标 3 和下标 5 处的 'e' 都离它最近，但距离是一样的 abs(4 - 3) == abs(4 - 5) = 1 。
距下标 8 最近的 'e' 出现在下标 6 ，所以距离为 abs(8 - 6) = 2 。


示例 2：

输入：s = "aaab", c = "b"
输出：[3,2,1,0]


 

提示：
1 <= s.length <= 104
s[i] 和 c 均为小写英文字母
题目数据保证 c 在 s 中至少出现一次
```

## 思路

### 1. 自己的版本
主角是未知的每一个字符

很简单的做法，三个指针 

【时间复杂度O（n2）】

index ：定位字符

front：字符前或空

end：字符后一个或空

通过判断`s[index]`, 's[front]', 's[end]' 是否和 c相同即可，相同push进abs坐标

### 2. 空间换时间

主角是要查询的字符

新建空数组存入查询字符的每一个下标，在循环遍历被查字符串的每一个未知

如果被查字符是目标字符，存入0

如果不是，存入差值

【时间复杂度on，新建数组导致空间复杂度on】

是时间换空间

### 3. 贪心

主角是被查位置的左侧/被查位置的右侧

步骤：

1.  左侧遍历到每个被查位置，记录其左侧最后一个目标字符和他的距离

**res里 infinite 是没有，0是该位置是目标值，>0 是该侧的最小距离**

res传递了距离

```javascript
res[i] = res[i - 1] === void 0 ? Infinity : res[i - 1] + 1;
```

2. 右侧遍历到每个被查位置，计算右侧最后一个目标字符和他的距离，选择性覆盖/添加
 

### 窗口

以每一对目标字符的位置做窗口，res数组记录窗口内每一个待查元素到窗口边界的最短距离

移动窗口来遍历每一个待查元素

时间on

注意第一个窗口的左边界，如果不是目标值，设成infinite或s.length


## 代码

- 语言支持：JavaScript

JavaScript Code:

```javascript

/**
 * @param {string} s
 * @param {character} c
 * @return {number[]}
 */
// 1. 时间复杂度n2
var shortestToChar = function(s, c) {
    let index = 0;
    let front = index +1;
    let end = index - 1;
    let answer = [];
    let len  = s.length - 1;
    while(index <= len ){
    let head = s[front]?s[front]:'';
    let tail = s[end]?s[end]:'';
    if( s[index] ===c){
        answer.push(0);
        index++;
        front = index +1;
        end = index - 1;
    }else if( head === c){
        answer.push( Math.abs(front-index));
        index++;
        front = index +1;
        end = index - 1;
    }else if( tail === c){
        answer.push(Math.abs(end-index));
        index++;
        front = index +1;
        end = index - 1;
    }else{
        front++;
        end--;
    }
    }
    return answer
};

// 2. 时间换空间  

var shortestToChar = function (S, C) {
  // 记录 C 字符在 S 字符串中出现的所有下标
  var cIndices = [];
  for (let i = 0; i < S.length; i++) {
    if (S[i] === C) cIndices.push(i);
  }

  // 结果数组 res
  var res = Array(S.length).fill(Infinity);

  for (let i = 0; i < S.length; i++) {
    // 目标字符，距离是 0
    if (S[i] === C) {
      res[i] = 0;
      continue;
    }

    // 非目标字符，到下标数组中找最近的下标
    for (const cIndex of cIndices) {
      const dist = Math.abs(cIndex - i);

      // 小小剪枝一下
      // 注：因为 cIndices 中的下标是递增的，后面的 dist 也会越来越大，可以排除
      if (dist >= res[i]) break;

      res[i] = dist;
    }
  }
  return res;
};


//3. 贪心

var shortestToChar = function (S, C) {
  var res = Array(S.length);

  for (let i = 0; i < S.length; i++) {
    if (S[i] === C) res[i] = 0;
    // 记录距离：res[i - 1] + 1
    else res[i] = res[i - 1] === void 0 ? Infinity : res[i - 1] + 1;
  }

  for (let i = S.length - 1; i >= 0; i--) {
    // 更新距离：res[i + 1] + 1
    if (res[i] === Infinity || res[i + 1] + 1 < res[i]) res[i] = res[i + 1] + 1;
  }

  return res;
};

//4. 窗口

var shortestToChar = function (S, C) {
  // 窗口左边界，如果没有就初始化为 Infinity，初始化为 S.length 也可以
  let l = S[0] === C ? 0 : Infinity,
    // 窗口右边界
    r = S.indexOf(C, 1);

  const res = Array(S.length);

  for (let i = 0; i < S.length; i++) {
    // 计算字符到当前窗口左右边界的最小距离
    res[i] = Math.min(Math.abs(i - l), Math.abs(r - i));

    // 遍历完了当前窗口的字符后，将整个窗口右移
    if (i === r) {
      l = r;
      r = S.indexOf(C, l + 1);
    }
  }

  return res;
};

```
