
## 题目地址(513. 找树左下角的值)

https://leetcode-cn.com/problems/find-bottom-left-tree-value/

## 题目描述

```
给定一个二叉树的 根节点 root，请找出该二叉树的 最底层 最左边 节点的值。

假设二叉树中至少有一个节点。

 

示例 1:

输入: root = [2,1,3]
输出: 1


示例 2:

输入: [1,2,3,4,null,5,6,null,null,7]
输出: 7


 

提示:

二叉树的节点个数的范围是 [1,104]
-231 <= Node.val <= 231 - 1 
```

## 前置知识


## 思路

### 1. DFS层序遍历

BFS代码是找到最深的不是左边最深的


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

// // 1. BFS层序遍历 找到左边最下面
// var findBottomLeftValue = function(root) {
//     var cur = [root];
//     var result = null;
//     // 当前队列不为空，内部循环
//     while(cur.length){
//         var next = [];
//         for( let i = 0; i< cur.length ; i++){
//             // 左
//             cur[i].left && next.push(cur[i].left);
//             // 右
//             cur[i].right && next.push(cur[i].right);

//         }
//     // 保证是左边第一个
//         result = cur[0].val;
//         cur = next ;
//     }
//     return result
// };


// 2. BFS 找到最深的，不是左边最下
function findBottomLeftValue(root) {
  let maxDepth = 0;
  let res = root.val;

  dfs(root.left, 0);
  dfs(root.right, 0);

  return res;

  function dfs(cur, depth) {
    if (!cur) {
      return;
    }
    const curDepth = depth + 1;
    if (curDepth > maxDepth) {
      maxDepth = curDepth;
      res = cur.val;
    }
    dfs(cur.left, curDepth);
    dfs(cur.right, curDepth);
  }
}

```



