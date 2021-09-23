
## 题目地址(100. 相同的树)

https://leetcode-cn.com/problems/same-tree/

## 题目描述

```
给你两棵二叉树的根节点 p 和 q ，编写一个函数来检验这两棵树是否相同。

如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。

 

示例 1：

输入：p = [1,2,3], q = [1,2,3]
输出：true


示例 2：

输入：p = [1,2], q = [1,null,2]
输出：false


示例 3：

输入：p = [1,2,1], q = [1,1,2]
输出：false


 

提示：

两棵树上的节点数目都在范围 [0, 100] 内
-104 <= Node.val <= 104
```

## 前置知识

- 

## 思路

### 1.  递归

最简单的想法是递归，这里先介绍下递归三要素

递归出口，问题最简单的情况

递归调用总是去尝试解决更小的问题，这样问题才会被收敛到最简单的情况

递归调用的父问题和子问题没有交集

尝试用递归去解决相同的树

分解为子问题，相同的树分解为左子是否相同，右子是否相同

【递归出口: 当树高度为 1 时，判断递归出口】

### 2. 层序遍历

### 3. 前中序确定一棵树
前序和中序的遍历结果确定一棵树，那么当两棵树前序遍历和中序遍历结果都相同，那是否说明两棵树也相同。

```javascript
var isSameTree = function (p, q) {
  const preorderP = preorder(p, []);
  const preorderQ = preorder(q, []);
  const inorderP = inorder(p, []);
  const inorderQ = inorder(q, []);
  return (
    preorderP.join("") === preorderQ.join("") &&
    inorderP.join("") === inorderQ.join("")
  );
};

// 前序遍历
function preorder(root, arr) {
  if (root === null) {
    arr.push(" ");
    return arr;
  }
  arr.push(root.val);
  preorder(root.left, arr);
  preorder(root.right, arr);
  return arr;
}

// 中序遍历
function inorder(root, arr) {
  if (root === null) {
    arr.push(" ");
    return arr;
  }
  inorder(root.left, arr);
  arr.push(root.val);
  inorder(root.right, arr);
  return arr;
}
```


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
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {boolean}
 */
var isSameTree = function (p, q) {
  if (!p || !q) {
    return !p && !q;
//如果有空直接false
  }
  return (
    p.val === q.val &&
    isSameTree(p.left, q.left) &&
    isSameTree(p.right, q.right)
  );
};

```


**复杂度分析**

令 n 为数组长度。

- 时间复杂度：$O(n)$
- 空间复杂度：$O(h)$树的高度


