
## 题目地址(24. 两两交换链表中的节点)

https://leetcode-cn.com/problems/swap-nodes-in-pairs/

## 题目描述

```
给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

 

示例 1：

输入：head = [1,2,3,4]
输出：[2,1,4,3]


示例 2：

输入：head = []
输出：[]


示例 3：

输入：head = [1]
输出：[1]


 

提示：

链表中节点的数目在范围 [0, 100] 内
0 <= Node.val <= 100

 

进阶：你能在不修改链表节点值的情况下解决这个问题吗?（也就是说，仅修改节点本身。）
```

## 思路

### 1. 递归

后序遍历：后面所有的节点都已经处理好了【后序遍历： 迭代在操作的前面】

```javascript
def dfs(head):
    if not head or not head.next: return head
    res = dfs(head.next)
    # 主逻辑（改变指针）在进入后面的节点的后面，也就是递归返回的过程会执行到
    head.next.next = head
    head.next = None

    return res
```
时间on 空间on （递归调用栈）

题解：

```javascript
var swapPairs = function(head) {
    if (head === null|| head.next === null) {
        return head;
    }
//第二个node是newHead
    const newHead = head.next;
//递归newHead.next 表示newHead.next后面的都已经处理好了，旧头接上了，来处理新头（来处理最开始的俩）
    head.next = swapPairs(newHead.next);
    newHead.next = head;
    return newHead;
};
```

### 2. 迭代：以每两个node为一组，添加虚拟头节点

添加一个虚拟头节点，因为head也要被更改

再把这个虚拟头拷贝一份出来， 用作迭代

1. 虚拟头记录新头

2. 内部更改方向

3. 新尾部指向原来的尾部

4. 把虚拟头移动到第二个上面

## 关键点

-  

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
 * @param {ListNode} head
 * @return {ListNode}
 */
var swapPairs = function(head) {
    var newhead = new ListNode();
    newhead.next = head;
    var myhead = newhead;
    while(myhead.next !==null  && myhead.next.next !== null ){
    var fast = myhead.next.next;
    var slow = myhead.next;
    // start to exchange
    myhead.next = fast;
    slow.next = fast.next;
    fast.next = slow;
    myhead = slow;
    }
return newhead.next
};

```


**复杂度分析**

令 n 为数组长度。

- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$


