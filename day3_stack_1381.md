
## 题目地址(1381. 设计一个支持增量操作的栈)

https://leetcode-cn.com/problems/design-a-stack-with-increment-operation/

## 题目描述

```
请你设计一个支持下述操作的栈。

实现自定义栈类 CustomStack ：

CustomStack(int maxSize)：用 maxSize 初始化对象，maxSize 是栈中最多能容纳的元素数量，栈在增长到 maxSize 之后则不支持 push 操作。
void push(int x)：如果栈还未增长到 maxSize ，就将 x 添加到栈顶。
int pop()：弹出栈顶元素，并返回栈顶的值，或栈为空时返回 -1 。
void inc(int k, int val)：栈底的 k 个元素的值都增加 val 。如果栈中元素总数小于 k ，则栈中的所有元素都增加 val 。

 

示例：

输入：
["CustomStack","push","push","pop","push","push","push","increment","increment","pop","pop","pop","pop"]
[[3],[1],[2],[],[2],[3],[4],[5,100],[2,100],[],[],[],[]]
输出：
[null,null,null,2,null,null,null,null,null,103,202,201,-1]
解释：
CustomStack customStack = new CustomStack(3); // 栈是空的 []
customStack.push(1);                          // 栈变为 [1]
customStack.push(2);                          // 栈变为 [1, 2]
customStack.pop();                            // 返回 2 --> 返回栈顶值 2，栈变为 [1]
customStack.push(2);                          // 栈变为 [1, 2]
customStack.push(3);                          // 栈变为 [1, 2, 3]
customStack.push(4);                          // 栈仍然是 [1, 2, 3]，不能添加其他元素使栈大小变为 4
customStack.increment(5, 100);                // 栈变为 [101, 102, 103]
customStack.increment(2, 100);                // 栈变为 [201, 202, 103]
customStack.pop();                            // 返回 103 --> 返回栈顶值 103，栈变为 [201, 202]
customStack.pop();                            // 返回 202 --> 返回栈顶值 202，栈变为 [201]
customStack.pop();                            // 返回 201 --> 返回栈顶值 201，栈变为 []
customStack.pop();                            // 返回 -1 --> 栈为空，返回 -1


 

提示：

1 <= maxSize <= 1000
1 <= x <= 1000
1 <= k <= 1000
0 <= val <= 100
每种方法 increment，push 以及 pop 分别最多调用 1000 次
```


## 思路

### 我的版本：（菜狗）

没啥好讲的，好简单的思路，js总是不讲武德

### 通过hashMap 保存 inc操作

![](https://pic.leetcode-cn.com/9e63bf59b9f83c4407bf0ef6a7a58a14b9e63fe01bc91b9a56c189982439acdd-custom_stack.png)



## 代码

- 语言支持：JavaScript

JavaScript Code:

### 1. array模拟stack

```javascript

/**
 * @param {number} maxSize
 */
var CustomStack = function(maxSize) {
    this.stack = [];
    this.maxSize = maxSize;

};

/** 
 * @param {number} x
 * @return {void}
 */
CustomStack.prototype.push = function(x) {
if( this.stack.length < this.maxSize){
    this.stack.push(x);
    return null;
}else{
    return null;
}
};

/**
 * @return {number}
 */
CustomStack.prototype.pop = function() {
    if( this.stack.length === 0 ){
        return -1
    }else{
        return this.stack.pop();
    }
};

/** 
 * @param {number} k 
 * @param {number} val
 * @return {void}
 */
CustomStack.prototype.increment = function(k, val) {
    let helper = [];
     k = k > this.stack.length ? this.stack.length : k;
    for ( let i = this.stack.length-1 ; i >= 0 ; i--){
        helper.push(i >= k ? this.stack.pop() : this.stack.pop()+val)
    }
    for ( let i = helper.length-1 ; i >= 0 ; i--){
        this.stack.push(helper.pop())
    }
};

//或者……………………
CustomStack.prototype.increment = function(k, val) {
      if(k>= this.stack.length ){
        this.stack = this.stack.map(item => item+val)
    }else{
        for(let i=0;i<k;i++){
            this.stack[i] += val
        }
    }
};


/**
 * Your CustomStack object will be instantiated and called as such:
 * var obj = new CustomStack(maxSize)
 * obj.push(x)
 * var param_2 = obj.pop()
 * obj.increment(k,val)
 */

```

### 2.  通过hashMap ：
```javascript
/**
 * @param {number} maxSize
 */
var CustomStack = function (maxSize) {
  this.list = []
  this.maxSize = maxSize
  this.hashMap = {}
};

/**
 * @param {number} key
 * @param {number} value
 * @return {void}
 */
CustomStack.prototype._setInc = function (key, value) {
  if (!(key in this.hashMap)) {
    this.hashMap[key] = 0
  }
  this.hashMap[key] += value
};

/**
 * @param {number} key
 * @return {number}
 */
CustomStack.prototype._getInc = function (key) {
  return this.hashMap[key] || 0
};

/**
 * @return {number}
 */
CustomStack.prototype._size = function () {
  return this.list.length
};

/** 
 * @param {number} x
 * @return {void}
 */
CustomStack.prototype.push = function (x) {
  if (this._size() < this.maxSize) {
    this.list.push(x)
  }
};

/**
 * @return {number}
 */
CustomStack.prototype.pop = function () {
  const top = this._size() - 1
  const inc = this._getInc(top)

  let item = this.list.pop()
  if (item === void 0) {
    return -1
  }

  item += inc
  const newTop = top - 1
  this._setInc(newTop, inc)
  this.hashMap[top] = 0
  return item
};

/** 
 * @param {number} k 
 * @param {number} val
 * @return {void}
 */
CustomStack.prototype.increment = function (k, val) {
  const size = this._size()
  k = k < size ? k - 1 : size - 1
  this._setInc(k, val)
};

/**
 * Your CustomStack object will be instantiated and called as such:
 * var obj = new CustomStack(maxSize)
 * obj.push(x)
 * var param_2 = obj.pop()
 * obj.increment(k,val)
 */

```

**复杂度分析**

令 n 为数组长度。

- 时间复杂度：
1. 方法1
$O(1)$ push pop      $O(k)$inc取决于修改几个

2. 方法2
时间复杂度 O(1) 的增量操作，不过代价就是额外的 O(n) 空间。



