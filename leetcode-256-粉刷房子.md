---
title: leetcode-256-粉刷房子
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-09-28 10:27:42
---

[link](https://leetcode-cn.com/problems/paint-house/)

![image-20210928103116885](https://gitee.com/cao_ziqiang/img/raw/master/20210928103116.png)

<hr/>

读题读了半天可还行...![img](https://gitee.com/cao_ziqiang/img/raw/master/20210928103141.jpg)

不过读懂了题目到是不难,题目给了我们一个$costs$数组,第一维代表每一间房子,第二维代表每一间房子刷不同颜色的油漆的花费,也是和打家劫舍一样不能对相邻房子刷一样的颜色。

考虑如何定义$dp$数组，题目的自变量有两个，一个是房子的索引，一个是刷的什么颜色的油漆。

所以定位$dp$为二维数组：

$dp[i][j] \Rightarrow $第$i-1$间房子刷第$j$种油漆的花费。

我们考虑当前第$i-1$间房子刷第$j$种油漆，就可以考虑当前刷了第$j$种油漆，那么它的前一间房子就只可能刷另外的两种油漆，所以最优的情况为前一间房子刷另外两种油漆的较小的那种加上当前房子刷第$j$种油漆的花费。

状态转移方程就为：

$dp[i][0] = Math.min(dp[i - 1][1], dp[i - 1][2]) + costs[i - 1][0];$

$dp[i][1] = Math.min(dp[i - 1][0], dp[i - 1][2]) + costs[i - 1][1];$

$dp[i][2] = Math.min(dp[i - 1][0], dp[i - 1][1]) + costs[i - 1][2];$

```java
class Solution {
    public int minCost(int[][] costs) {
        int n = costs.length;
        if(n == 1) {
            int ans = costs[0][0];
            for(int i = 0; i < costs[0].length; i++) {
                if (ans > costs[0][i]) {
                    ans = costs[0][i];
                }
            }
            return ans;
        }
        // 刷到第i间房子选择第j种油漆的最小花费
        int[][] dp = new int[n + 1][3];
        // 选择红色油漆
        dp[1][0] = costs[0][0];
        // 选择蓝色油漆
        dp[1][1] = costs[0][1];
        // 选择绿色油漆
        dp[1][2] = costs[0][2];
        // int ans = Math.min(dp[1][0], dp[1][1]);
        // ans = Math.min(ans, dp[1][2]);
        // if(n == 1) {
        //     return ans;
        // }
        for(int i = 2; i <= n; i++) {
            dp[i][0] = Math.min(dp[i - 1][1], dp[i - 1][2]) + costs[i - 1][0];
            dp[i][1] = Math.min(dp[i - 1][0], dp[i - 1][2]) + costs[i - 1][1];
            dp[i][2] = Math.min(dp[i - 1][0], dp[i - 1][1]) + costs[i - 1][2];
            // System.out.println(dp[i][0]+"\t"+dp[i][1]+"\t"+dp[i][2]);
        }
        int ans = Integer.MAX_VALUE;
        ans = Math.min(ans,dp[n][0]);
        ans = Math.min(ans,dp[n][1]);
        ans = Math.min(ans,dp[n][2]);
        return ans;
    }
}
```

