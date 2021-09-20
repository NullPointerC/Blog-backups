---
title: leetcode-322-零钱兑换
date: 2021-09-18 10:23:45
categories: LeetCode
tags: [algorithm,Java,LeetCode]

---

[link](https://leetcode-cn.com/problems/coin-change/)

<hr/>

![image-20210918102517747](https://gitee.com/cao_ziqiang/img/raw/master/20210918102517.png)

<hr/>

首先定义$dp[i]$为要得到金额为$i$所需要的硬币数量。

并且初始化为$Integer.MAX \underline\quad VALUE$。

以不同面额的硬币为外层$for$的起点，再以当前面值的硬币到所求的$amout$为终点来遍历，如果子问题有解，则尝试更新$dp[i]$

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        
        
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, Integer.MAX_VALUE);
        dp[0] = 0;
        
        for (int c : coins) {
            for (int i = c; i <= amount; i++) {
                if (dp[i - c] != Integer.MAX_VALUE) {
                    dp[i] = Math.min(dp[i], dp[i - c] + 1);
                }
            }
        }
        return dp[amount] == Integer.MAX_VALUE ? -1 : dp[amount];
    }
}
```

