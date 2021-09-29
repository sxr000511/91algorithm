
## 题目地址(347. 前 K 个高频元素)

https://leetcode-cn.com/problems/top-k-frequent-elements/

## 题目描述

```
给你一个整数数组 nums 和一个整数 k ，请你返回其中出现频率前 k 高的元素。你可以按 任意顺序 返回答案。

 

示例 1:

输入: nums = [1,1,1,2,2,3], k = 2
输出: [1,2]


示例 2:

输入: nums = [1], k = 1
输出: [1]

 

提示：

1 <= nums.length <= 105
k 的取值范围是 [1, 数组中不相同的元素的个数]
题目数据保证答案唯一，换句话说，数组中前 k 个高频元素的集合是唯一的

 

进阶：你所设计算法的时间复杂度 必须 优于 O(n log n) ，其中 n 是数组大小。
```



## 思路

### 1. js.map + JS sort 数组函数

时间复杂度：O(nlogn)
空间复杂度：O(n)

```javascript

/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number[]}
 */
let topKFrequent = function(nums, k) {
    let map = new Map(), arr = [...new Set(nums)]
    nums.map((num) => {
        if(map.has(num)) map.set(num, map.get(num)+1)
        else map.set(num, 1)
    })
    
    return arr.sort((a, b) => map.get(b) - map.get(a)).slice(0, k);
};


```


### 2. js map + 最小堆

最小堆： 根比左右小

构造最小堆

具体步骤如下：

遍历数据，统计每个元素的频率，并将元素值（ key ）与出现的频率（ value ）保存到 map 中

遍历 map ，将前 k 个数，构造一个小顶堆

从 k 位开始，继续遍历 map ，每一个数据出现频率都和小顶堆的【堆顶】元素出现频率进行比较，如果小于堆顶元素，则不做任何处理，继续遍历下一元素；如果大于堆顶元素，则将这个元素替换掉堆顶元素，然后【再堆化成一个小顶堆。】

遍历完成后，【堆中的数据就是前 k 大的数据】

```javascirpt

let topKFrequent = function(nums, k) {
    let map = new Map(), heap = [,]
    nums.map((num) => {
        if(map.has(num)) map.set(num, map.get(num)+1)
        else map.set(num, 1)
    })
    
    // 如果元素数量小于等于 k
    if(map.size <= k) {
        return [...map.keys()]
    }
    
    // 如果元素数量大于 k，遍历map，【【构建小顶堆】】
    let i = 0
    map.forEach((value, key) => {
        if(i < k) {
            // 取前k个建堆, 插入堆
            heap.push(key)
            // 原地建立前 k 堆
            if(i === k-1) buildHeap(heap, map, k)
        } else if(map.get(heap[1]) < value) {
            // 替换并堆化
            heap[1] = key
            // 自上而下式堆化第一个元素
            heapify(heap, map, k, 1)
        }
        i++
    })
    // 删除heap中第一个元素
    heap.shift()
    return heap
};

// 原地建堆，从后往前，自上而下式建小顶堆
let buildHeap = (heap, map, k) => {
    if(k === 1) return
    // 从最后一个非叶子节点开始，自上而下式堆化
    for(let i = Math.floor(k/2); i>=1 ; i--) {
        heapify(heap, map, k, i)
    }
}

// 堆化
let heapify = (heap, map, k, i) => {
    // 自上而下式堆化
    while(true) {
        let minIndex = i
        if(2*i <= k && map.get(heap[2*i]) < map.get(heap[i])) {
            minIndex = 2*i
        }
        if(2*i+1 <= k && map.get(heap[2*i+1]) < map.get(heap[minIndex])) {
            minIndex = 2*i+1
        }
        if(minIndex !== i) {
            swap(heap, i, minIndex)
            i = minIndex
        } else {
            break
        }
    }
}

// 交换
let swap = (arr, i , j) => {
    let temp = arr[i]
    arr[i] = arr[j]
    arr[j] = temp
}

```

时间复杂度：遍历数组需要 O(n) 的时间复杂度，一次堆化需要 O(logk) 时间复杂度，所以利用堆求 Top k 问题的时间复杂度为 O(nlogk)
空间复杂度：O(n)


### 3. js map + 桶排序

首先使用 map 来存储频率
然后创建一个数组（有数量的桶），将【频率】作为【数组下标】，对于出现频率不同的数字集合，【存入对应的数组下标】（桶内）即可。

```javascript
let topKFrequent = function(nums, k) {
    let map = new Map(), arr = [...new Set(nums)]
    nums.map((num) => {
        if(map.has(num)) map.set(num, map.get(num)+1)
        else map.set(num, 1)
    })
    
    // 如果元素数量小于等于 k
    if(map.size <= k) {
        return [...map.keys()]
    }
    
    return bucketSort(map, k)
};

// 桶排序
let bucketSort = (map, k) => {
    let arr = [], res = []
    map.forEach((value, key) => {
        // 利用映射关系（出现频率作为下标）将数据分配到各个桶中
        if(!arr[value]) {
            arr[value] = [key]
        } else {
            arr[value].push(key)
        }
    })
    // 倒序遍历获取出现频率最大的前k个数
    for(let i = arr.length - 1;i >= 0 && res.length < k;i--){
        if(arr[i]) {
            res.push(...arr[i])
        }
	}
	return res
}


```

## 关键点

-  

## 代码

- 语言支持：JavaScript

JavaScript Code:

```javascript

/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number[]}
 */
let topKFrequent = function(nums, k) {
    let map = new Map(), arr = [...new Set(nums)]
    nums.map((num) => {
        if(map.has(num)) map.set(num, map.get(num)+1)
        else map.set(num, 1)
    })
    
    return arr.sort((a, b) => map.get(b) - map.get(a)).slice(0, k);
};


```
时间复杂度：O(n)
空间复杂度：O(n)


