
## 题目地址(104. 二叉树的最大深度)

https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/

## 题目描述

```
给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

说明: 叶子节点是指没有子节点的节点。

示例：
给定二叉树 [3,9,20,null,null,15,7]，

    3
   / \
  9  20
    /  \
   15   7

返回它的最大深度 3 。
```

## 思路

用【递归】做

【递归怎么写？】

1. 定义函数功能，不用管其具体实现。

2. 确定大问题和小问题的关系。

要解决 f(root) 这个问题。可以先解决 f(root.right) 和 f(root.left)，当然我们仍然不关心 f 怎么实现。

f(root) 与 f(root.right) 和 f(root.left) 有什么关系呢？ 不难看出 1 + max(f(root.right), f(root.left))。

3. 补充递归终止条件。

如果递归到叶子节点的时候，返回 0 即可。

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
var maxDepth = function (root) {
  if (root === null) {
    return 0;
  }
  return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
};

```


**复杂度分析**

令 n 为数组长度。

- 时间复杂度：$O(n)$
- 空间复杂度：$O(h)$【【其中 h 为树的深度，最坏的情况 h 等于 N，其中 N 为节点数，此时树退化到链表】】


