---
title: leetcode-746-使用最小花费爬楼梯
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: 6b716dcb
date: 2021-09-26 18:48:05
---

[$link$](https://leetcode-cn.com/problems/min-cost-climbing-stairs/)

<hr/>

![image-20210926184853278](https://gitee.com/cao_ziqiang/img/raw/master/20210926184853.png)

<hr/>

这题又是动态规划类型的题，可以推出，要达到第$i$级楼梯，有两个途径，一个是从第$i-1$级上去，一个是从第$i-2$级上去。

那么假设$dp[i]$表示上到第$i$级台阶所花费的能量，现在要求的是到$dp[cost.length]$所花费的能量，而递归关系为：

$dp[i] = min(dp[i-1]+cost[i-1],dp[i-2]+cost[i-2])$

对于第1级和第2级台阶，可以直接上，故$dp[0]、dp[1]$都为0。

```java
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        int l = cost.length;
        if(l == 0) {
            return 0;
        }
        if(l == 1) {
            return cost[1];
        }
        if(l == 2) {
            return Math.min(cost[0], cost[1]);
        }
        // dp[i]即爬到第i阶楼梯需要的体力花费
        int[] dp = new int[l + 1];
        dp[1] = 0;
        dp[2] = 0;
         for (int i = 2; i <= l; i++) {
            dp[i] = Math.min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2]);
        }
        return dp[l];
    }
}
```

