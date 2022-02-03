---
title: leetcode-1725-可以形成最大正方形的矩形数目
date: 2022-02-04 00:09:21
categories: LeetCode
tags: [LeetCode,algorithm]
---

[1725. 可以形成最大正方形的矩形数目](https://leetcode-cn.com/problems/number-of-rectangles-that-can-form-the-largest-square/)

![image-20220204000953831](https://gitee.com/cao_ziqiang/img/raw/master/20220204000953.png)

比较简单的模拟，每次取两条$edge$中较短的那条，如果等于$maxLen$，则结果加上1，如果小于$maxLen$，则无影响，如果大于$maxLen$，则重置计算，并更新$maxLen$。

```java
class Solution {
    public int countGoodRectangles(int[][] rectangles) {
        int cnt = 0, n = rectangles.length, maxLen = Integer.MIN_VALUE;
        for(int i = 0; i < n; i++) {
            int[] edges = rectangles[i];
            int edge = Math.min(edges[0], edges[1]);
            if(edge > maxLen) {
                maxLen = edge;
                cnt = 1;
            } else if(edge < maxLen) {
                continue;
            } else if(edge == maxLen) {
                cnt++;
            }
        }
        return cnt;
    }
}
```

