
## 题目地址(146. LRU 缓存机制)

https://leetcode-cn.com/problems/lru-cache/

## 题目描述

```
运用你所掌握的数据结构，设计和实现一个  LRU (最近最少使用) 缓存机制 。

实现 LRUCache 类：

LRUCache(int capacity) 以正整数作为容量 capacity 初始化 LRU 缓存
int get(int key) 如果关键字 key 存在于缓存中，则返回关键字的值，否则返回 -1 。
void put(int key, int value) 如果关键字已经存在，则变更其数据值；如果关键字不存在，则插入该组「关键字-值」。当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。

 

进阶：你是否可以在 O(1) 时间复杂度内完成这两种操作？

 

示例：

输入
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
输出
[null, null, null, 1, null, -1, null, -1, 3, 4]

解释
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // 缓存是 {1=1}
lRUCache.put(2, 2); // 缓存是 {1=1, 2=2}
lRUCache.get(1);    // 返回 1
lRUCache.put(3, 3); // 该操作会使得关键字 2 作废，缓存是 {1=1, 3=3}
lRUCache.get(2);    // 返回 -1 (未找到)
lRUCache.put(4, 4); // 该操作会使得关键字 1 作废，缓存是 {4=4, 3=3}
lRUCache.get(1);    // 返回 -1 (未找到)
lRUCache.get(3);    // 返回 3
lRUCache.get(4);    // 返回 4


 

提示：

1 <= capacity <= 3000
0 <= key <= 10000
0 <= value <= 105
最多调用 2 * 105 次 get 和 put
```

## 前置知识

哈希表 +  双链表 

## 思路

哈希表： 存放存储位置，用来快速查找

双链表： 链表用来首尾移动node，快。由于移除链表节点后还需要把该节点前后的两个节点连起来，因此我们需要的是双向链表而不是单向链表。

## 关键点

-  定义node 类

- 利用node类定义double linked类

- 通过map 和 double linked 形成 LRU类

**对象都是node，有pre 和 next**

1. 查找get：

   1. map里找不到直接-1返回

   2. 找到则put提到最前，返回val

2. 修改增加put

   1. 有的话双链表先删除node，调用remove

   2. 没有的话移除双链表最后一个

   3. 然后把新node添加到头部

## 代码

- 语言支持：JavaScript

JavaScript Code:

```javascript

// 链表节点
class Node{
    constructor(key,val){
        this.key = key;
        this.val = val;
    }
}
// 双链表
class DoubleList{
    // 初始化头、尾节点、链表最大容量
    constructor(){
        this.head = new Node(0,0);
        this.tail = new Node(0,0);
        this.size = 0;
        //双链表 头节点的下一个是尾 尾的下一个是头
        this.head.next = this.tail;
        this.tail.prev = this.head;
    }
    // 在链表头部添加节点
    addFirst(node){
        node.next = this.head.next;
        node.prev = this.head;
        this.head.next.prev = node;
        this.head.next = node;
        this.size++;
    }
    // 删除链表中存在的node节点
    remove(node){
        node.prev.next = node.next;
        node.next.prev = node.prev;
        this.size--;
    }
    // 删除链表中最后一个节点，并返回该节点
    removeLast(){
        // 链表为空
        if(this.tail.prev == this.head){
            return null;
        }
        let last = this.tail.prev;
        this.remove(last);
        return last;
    }
}
/**
 * @param {number} capacity
 */
var LRUCache = function(capacity) {
    this.cap = capacity;
    this.map = new Map();
    this.cache = new DoubleList();
};

/** 
 * @param {number} key
 * @return {number}
 */
LRUCache.prototype.get = function(key) {
    let map = this.map;
    if(!map.has(key)){
        return -1;
    }
    let val = map.get(key).val;
    // 最近访问数据置前
    this.put(key,val);
    return val;
};

/** 
 * @param {number} key 
 * @param {number} value
 * @return {void}
 */
LRUCache.prototype.put = function(key, value) {
    let cache = this.cache;
    let map = this.map;
    let node = new Node(key,value);
    if(map.has(key)){
        // 删除旧的节点，新的插到头部
        cache.remove(map.get(key));
    }else{
        if(this.cap == cache.size){
            // 删除最后一个
            let last = cache.removeLast();
            map.delete(last.key);
        }
    }
    // 新增头部
    cache.addFirst(node);
    // 更新 map 映射
    map.set(key,node);
};

/** 
 * Your LRUCache object will be instantiated and called as such:
 * var obj = new LRUCache(capacity)
 * var param_1 = obj.get(key)
 * obj.put(key,value)
 */


```


**复杂度分析**

令 n 为数组长度。

时间复杂度：各种操作平均都是O(1)。

空间复杂度：链表占用空间O(N)，哈希表占用空间也是O(N)，因此总的空间复杂度为O(N)，其中 N 为容量大小，也就是题目中的 capacity。


