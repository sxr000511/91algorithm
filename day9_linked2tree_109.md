
## 题目地址(109. 有序链表转换二叉搜索树)

https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree/

## 题目描述

```
给定一个单链表，其中的元素按升序排序，将其转换为高度平衡的二叉搜索树。

本题中，一个高度平衡二叉树是指一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1。

示例:

给定的有序链表： [-10, -3, 0, 5, 9],

一个可能的答案是：[0, -3, 9, -10, null, 5], 它可以表示下面这个高度平衡二叉搜索树：

      0
     / \
   -3   9
   /   /
 -10  5

```

## 思路
提供的单链表 已经按 升序排列 

### 1. 快慢指针，分治递归

【递归】

1. 获取当前链表的中点

2. 以链表中点为根

3. 中点左边的值都小于它,可以构造左子树【参数是左子树的头和左子树的尾 head + slow】

4. 同理构造右子树【右子树的头 +  右子树的尾  slow.next（右子树的头） + tail （右子树的尾）】

5. 【循环】第一步---》递归

### 2. 在递归基础上，用数组减少查询

可以使用数组将链表的值存储,以空间换时间。

```javascirpt
// 变数组res[]
var sortedListToBST = function (head) {
  let res = [];
  while (head) {
    res.push(head.val);
    head = head.next;
  }
  return dfs(res, 0, res.length - 1);
};

// 查找位置不用On，用数组存起来以后变o1
function dfs(res, l, r) {
  if (l > r) return null;
  let mid = parseInt((l - r) / 2 + r);
  let root = new TreeNode(res[mid]);
// 左侧
  root.left = dfs(res, l, mid - 1);
//右侧  
root.right = dfs(res, mid + 1, r);
  return root;
}
```

## 关键点

【时间复杂度】如果有递归那就是：递归树的节点数 * 递归函数内部的基础操作数

【总的空间复杂度 】如果有递归那就是： 递归深度 * 每一层基础操作数

## 代码

- 语言支持：JavaScript

JavaScript Code:

```javascript

/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {ListNode} head
 * @return {TreeNode}
 */
var sortedListToBST = function(head) {
    if(!head){
        return null;
    }
    return dfs(head,null);
};
// 分治
function dfs( head, tail){
    if( head === tail) {
        return null;
    }
    let fast = head;
    let slow = head;
    // 二倍快慢指针
    while( fast !== tail && fast.next !== tail){
        fast = fast.next.next;
        slow = slow.next;
    }
    let root = new TreeNode(slow.val);
    // 上一段
    root.left = dfs( head, slow);
    // 下一段
    root.right = dfs( slow.next , tail);

    return root;
}

```


**复杂度分析**

令 n 为链表长度。

画出如下的**递归树**。

![](https://tva1.sinaimg.cn/large/008i3skNly1gqmduc0j3dj314d0jk7ju.jpg)

由于递归树的深度为 logn因此【空间复杂度就是 logn 】

 递归函数内部的空间复杂度，由于递归函数内空间复杂度为 O(1），

因此总的空间复杂度为 O(logn)。

时间复杂度：递归树的【深度为 logn】，每【一层的基本操作数为 n】，因此总的【时间复杂度为O(nlogn)】

空间复杂度：空间复杂度为O(logn)


