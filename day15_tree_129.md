
## 题目地址(129. 求根节点到叶节点数字之和)

https://leetcode-cn.com/problems/sum-root-to-leaf-numbers/

## 题目描述

```
给你一个二叉树的根节点 root ，树中每个节点都存放有一个 0 到 9 之间的数字。

每条从根节点到叶节点的路径都代表一个数字：

例如，从根节点到叶节点的路径 1 -> 2 -> 3 表示数字 123 。

计算从根节点到叶节点生成的 所有数字之和 。

叶节点 是指没有子节点的节点。

 

示例 1：

输入：root = [1,2,3]
输出：25
解释：
从根到叶子节点路径 1->2 代表数字 12
从根到叶子节点路径 1->3 代表数字 13
因此，数字总和 = 12 + 13 = 25

示例 2：

输入：root = [4,9,0,5,1]
输出：1026
解释：
从根到叶子节点路径 4->9->5 代表数字 495
从根到叶子节点路径 4->9->1 代表数字 491
从根到叶子节点路径 4->0 代表数字 40
因此，数字总和 = 495 + 491 + 40 = 1026


 

提示：

树中节点的数目在范围 [1, 1000] 内
0 <= Node.val <= 9
树的深度不超过 10
```



## 思路

不管哪种方法，更新sum的条件都是node没有左右子节点

### 1. DFS

递归

时间on 空间 oh（树的高度）

### 2. BFS

循环遍历每一层

## 关键点

-  

## 代码

- 语言支持：JavaScript

JavaScript Code:

```javascript

/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number}
 */

//  DFS
// function sumNumbers(root) {
//   let sum = 0;
//   function dfs(root, cur) {
//     if (!root) {
//       return;
//     }
//     let curSum = cur * 10 + root.val;

//     // 如果没有左右子节点，代表这条route已经走完了，该用return直接返回值了
//     // sum 用来存储结果
//     // cursum用来传递（cur是其别名），递归计算
//     if (!root.left && !root.right) {
//       sum += curSum;
//       return;
//     }
//     dfs(root.left, curSum);
//     dfs(root.right, curSum);
//   }
//   dfs(root, 0);
//   return sum;
// }


// BFS

function sumNumbers(root) {
  let sum = 0;
  let curLevel = [];
  if (root) {
    curLevel.push(root);
  }
//   curLevel 在内部修改后在外部持续进入循环
  while (curLevel.length) {
    let nextLevel = [];
    // 用curlevel.length 更新本层数据
    // for 循环遍历每一个数据
    for (let i = 0; i < curLevel.length; i++) {
      let cur = curLevel[i];
      if (cur.left) {
        cur.left.val = cur.val * 10 + cur.left.val;
        // 下一层数据
        nextLevel.push(cur.left);
      }
      if (cur.right) {
        cur.right.val = cur.val * 10 + cur.right.val;
        nextLevel.push(cur.right);
      }
      if (!cur.left && !cur.right) {
        sum += cur.val;
      }
    //   把下一层数据赋值进来
      curLevel = nextLevel;
    }
  }
  return sum;
}

```


**复杂度分析**

令 n 为数组长度。

- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$


