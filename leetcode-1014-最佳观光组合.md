---
title: leetcode-1014-最佳观光组合
date: 2021-10-16 20:49:11
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
---

[link](https://gitee.com/cao_ziqiang/img/raw/master/20211016204944.png)

![image-20211016204944137](https://gitee.com/cao_ziqiang/img/raw/master/20211016204944.png)

<hr/>

题中给定的公式可分为$score=value[i]+value[j]+i-j \Rightarrow value[i] +i + value[j] - j$。

故：$max(socre) = max(value[i] + i) + max(value[j]-j)\quad i \lt j$。

```java
class Solution {
    public int maxScoreSightseeingPair(int[] values) {
        int n = values.length;
        int left = values[0] + 0;
        int ans = Integer.MIN_VALUE;
        for(int i = 1; i < n; i++) {
            ans = Math.max(values[i] - i + left, ans);
 			// 更新左边的最小值即(value[i] + i)
            left = Math.max(values[i] + i, left);
        }
        return ans;
    }
}
```

