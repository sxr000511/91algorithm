
## 题目地址(61. 旋转链表)

https://leetcode-cn.com/problems/rotate-list/

## 题目描述

```
给你一个链表的头节点 head ，旋转链表，将链表每个节点向右移动 k 个位置。

 

示例 1：

输入：head = [1,2,3,4,5], k = 2
输出：[4,5,1,2,3]


示例 2：

输入：head = [0,1,2], k = 4
输出：[2,0,1]


 

提示：

链表中节点的数目在范围 [0, 500] 内
-100 <= Node.val <= 100
0 <= k <= 2 * 109
```

## 前置知识


## 思路

【不管是用计算方法 还是双指针 ，思路是不变的】
1. 获取单链表的倒数第 1 （尾节点）与倒数第 2 个节点
2. 将倒数第 2 个节点的 next 指向 null
3. 将尾节点的 next 指向 head（拼起来）
4. 返回倒数第 1 个节点

### 双指针

双指针快慢一次可以获得新链表的末尾【  k = k % count;】 和原来的末尾

原来的末尾用来连接到头形成新链表

```javascript
var rotateRight = function (head, k) {
  if (!head || !head.next) return head;
  let count = 0,
    now = head;
  while (now) {
  //先取到总长
    now = now.next;
    count++;
  }
  k = k % count;
  let slow = (fast = head);
  while (fast.next) {
    if (k-- <= 0) {
    //slow到达新链表末端
      slow = slow.next;
    }
    //fast到达链表末端
    fast = fast.next;
  }
  fast.next = head;
  let res = slow.next;
  slow.next = null;
  return res;
};
```

### 计算

计算法需要先走到末尾，同时也获得了总长
然后再计算到哪截断，
在从头next到截断位置
```javascript
var rotateRight = function(head, k) {
    if (k === 0 || !head || !head.next) {
        return head;
    }
    let n = 1;
    let cur = head;
    //先走到末尾取到总长
    while (cur.next) {
        cur = cur.next;
        n++;
    }
    
    //计算截断位置

    let add = n - k % n;
    if (add === n) {
        return head;
    }

    cur.next = head;
    while (add) {
        cur = cur.next;
        add--;
    }

    const ret = cur.next;
    cur.next = null;
    return ret;
};

```
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
 * @param {number} k
 * @return {ListNode}
 */
var rotateRight = function (head, k) {
  if (!head || !head.next) return head;
  let count = 0,
    now = head;
  while (now) {
    now = now.next;
    count++;
  }
  k = k % count;
  let slow = (fast = head);
  while (fast.next) {
    if (k-- <= 0) {
      slow = slow.next;
    }
    fast = fast.next;
  }
  fast.next = head;
  let res = slow.next;
  slow.next = null;
  return res;
};


```


**复杂度分析**

令 n 为数组长度。

- 时间复杂度：$O(n)$ 最多遍历两边
- 空间复杂度：$O(1)$


