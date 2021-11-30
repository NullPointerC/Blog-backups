---
title: leetcode-414-第三大的数
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-10-06 12:03:10
---

[$link$](https://leetcode-cn.com/problems/third-maximum-number/)

![image-20211007120410998](https://gitee.com/cao_ziqiang/img/raw/master/20211007120411.png)

<hr/>

比较简单的题目,维护三个变量即可,$max$,$secondMax$,$thirdMax$.注意题目给的最大和最小都达到了$Integer$的上限和下限.所以可以先用$long$型存储

```java
class Solution {
    public int thirdMax(int[] nums) {
        long max = Long.MIN_VALUE;
        long secondMax = Long.MIN_VALUE;
        long thirdMax = Long.MIN_VALUE;
        for(int num : nums) {
            if(num > max) {
                thirdMax = secondMax;
                secondMax = max;
                max = num;
            }else if (num > secondMax && num < max) {
                thirdMax = secondMax;
                secondMax = num;
            }else if (num > thirdMax && num < secondMax) {
                thirdMax = num;
            }else {
                continue;
            }
        }
        if(thirdMax == Long.MIN_VALUE) {
            return (int)max;
        }
        return (int)thirdMax;
    }
}
```

<hr/>

如果是为了获取前$topK$个元素,也可以考虑建堆.在$Java$中就比较方便,$jdk$提供了优先队列.

```java
class Solution {
    public int thirdMax(int[] nums) {
        int n = nums.length;
        PriorityQueue<Integer> heap = new PriorityQueue<>((a, b) -> b.compareTo(a));
        for (int i = 0; i < n; i++) {
            heap.offer(nums[i]);
        }
        int k = 2;
        int max = heap.peek();
        int res = heap.poll();
        while (k > 0) {
            if (heap.isEmpty()) {
                break;
            }
            if (heap.peek() != res) {
                res = heap.poll();
                k--;
            } else {
                heap.poll();
            }
        }
        if (k > 0) res = max;
        return res;
    }
}
```

