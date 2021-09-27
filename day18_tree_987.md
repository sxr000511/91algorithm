
## 题目地址(987. 二叉树的垂序遍历)

https://leetcode-cn.com/problems/vertical-order-traversal-of-a-binary-tree/

## 题目描述

```
给你二叉树的根结点 root ，请你设计算法计算二叉树的 垂序遍历 序列。

对位于 (row, col) 的每个结点而言，其左右子结点分别位于 (row + 1, col - 1) 和 (row + 1, col + 1) 。树的根结点位于 (0, 0) 。

二叉树的 垂序遍历 从最左边的列开始直到最右边的列结束，按列索引每一列上的所有结点，形成一个按出现位置从上到下排序的有序列表。如果同行同列上有多个结点，则按结点的值从小到大进行排序。

返回二叉树的 垂序遍历 序列。

 

示例 1：

输入：root = [3,9,20,null,null,15,7]
输出：[[9],[3,15],[20],[7]]
解释：
列 -1 ：只有结点 9 在此列中。
列  0 ：只有结点 3 和 15 在此列中，按从上到下顺序。
列  1 ：只有结点 20 在此列中。
列  2 ：只有结点 7 在此列中。

示例 2：

输入：root = [1,2,3,4,5,6,7]
输出：[[4],[2],[1,5,6],[3],[7]]
解释：
列 -2 ：只有结点 4 在此列中。
列 -1 ：只有结点 2 在此列中。
列  0 ：结点 1 、5 和 6 都在此列中。
          1 在上面，所以它出现在前面。
          5 和 6 位置都是 (2, 0) ，所以按值从小到大排序，5 在 6 的前面。
列  1 ：只有结点 3 在此列中。
列  2 ：只有结点 7 在此列中。


示例 3：

输入：root = [1,2,3,4,6,5,7]
输出：[[4],[2],[1,5,6],[3],[7]]
解释：
这个示例实际上与示例 2 完全相同，只是结点 5 和 6 在树中的位置发生了交换。
因为 5 和 6 的位置仍然相同，所以答案保持不变，仍然按值从小到大排序。

 

提示：

树中结点数目总数在范围 [1, 1000] 内
0 <= Node.val <= 1000
```


## 思路  ：DFS

使用 DFS（没有特殊理由），

将节点存储到一个哈希表中，其中 key 为节点的 x 值，value 为横坐标为 x 的节点值列表（不妨用数组表示）

最后需要需要进行三次排序，分别是对 x 坐标，y 坐标 和 值。

先是x再是y最后是值，值最优先。

然后打印的时候当列数发生改变就push进一个新的[] 用来存node的value

## 代码

- 语言支持：JavaScript

JavaScript Code:

```javascript

var verticalTraversal = function(root) {
    const nodes = [];
    dfs(root, 0, 0, nodes);
    nodes.sort((tuple1, tuple2) => {
        if (tuple1[0] !== tuple2[0]) {
            // 不同行，行数小的在先，即下面的在先
            return tuple1[0] - tuple2[0];
        } else if (tuple1[1] !== tuple2[1]) {
            // 同行不同列（优先）（后排），列数小的在先，即左边的在先
            return tuple1[1] - tuple2[1];
        } else {
            // 值（如果行列相同），同行同列值小的在先
            return tuple1[2] - tuple2[2];
        }
    });

    const ans = [];
    // 用来记录第几列了
    let lastcol = -Number.MAX_VALUE;
    // for of 遍历所有的node
    // console.log(nodes)
    for (const tuple of nodes) {
        let col = tuple[0], row = tuple[1], value = tuple[2];
        // 不同列的时候push进新的[]
        if (col !== lastcol) {
            // 改变当前列数
            lastcol = col;
            ans.push([]);
        }
        ans[ans.length - 1].push(value);
    }
    return ans;
}

const dfs = (node, row, col, nodes) => {
    if (node === null) {
        return;
    }
    nodes.push([col, row, node.val]);
    dfs(node.left, row + 1, col - 1, nodes);
    dfs(node.right, row + 1, col + 1, nodes);
}


```


**复杂度分析**

令 n 为数组长度。
时间复杂度：$O(nlogn)$

