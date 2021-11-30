---
title: leetcode-309-最佳买卖股票时间含冰冻期
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-09-29 21:50:18
---

[$link$](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

<hr/>

![image-20210929215208315](https://gitee.com/cao_ziqiang/img/raw/master/20210929215208.png)

<hr/>

相比起之前的题目，就是多了一种状态需要记录，在$dp$的第二维多记录一个冰冻期的状态。

```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        if(n < 2) {
            return 0;
        }
        // 表示第i天结束时处于状态j的金额
        int[][] dp = new int[n + 1][3];
        dp[1][0] = 0;
        dp[1][1] = 0 - prices[0];
        dp[1][2] = 0;
        for(int i = 2; i <= n; i++) {
            // 第i天没有股票可能是昨天也没有，今天也没买，或者昨天刚交易完，今天是冰冻期没法买股票
            dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][2]);
            // 第i天有股票可能是因为昨天有，或者昨天没有今天刚买的
            dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] - prices[i - 1]);
            // 冰冻期说明昨天有今天交易出去了
            dp[i][2] = dp[i - 1][1] + prices[i - 1];
        }
        return Math.max(dp[n][0], dp[n][2]);
    }
}
```

又或者这么理解：

记$dp[i][0]$为今天之前卖出，$dp[i][1]$为今天卖出，$dp[i][2]$为今天持股。

若今天之前卖出的，到了今天可能时昨天之前就卖出，也可能昨天卖出：

$dp[i][0] = Math.max(dp[i - 1][0],dp[i - 1][1])$

如果今天卖出的，就没有其他情况，直接计算收益即可

$dp[i][1] = dp[i-1][2] + prices[i - 1]$

如果是今天持股，那么可能是昨天也持股，或者昨天没有，今天刚买的，这种情况下，一定是不能昨天刚卖，因为有冷冻期

$dp[i][2] = Math.max(dp[i - 1][2], dp[i-1][0]-prices[i -1])$

最后看没有持股的两种状态：

$Math.max(dp[n][0],dp[n][1])$

```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        if( n < 2) {
            return 0;
        }
        // 0代表今天之前卖出，1代表今天卖出，2代表今天有股票
        int[][] dp = new int[n + 1][3];
        dp[1][2] = 0 - prices[0];
        for(int i = 2; i <= n; i++) {
            dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1]);
            dp[i][1] = dp[i - 1][2] + prices[ i - 1 ];
            dp[i][2] = Math.max(dp[i - 1][2], dp[i - 1][0] - prices[ i - 1 ]);
        }
        return Math.max(dp[n][0], dp[n][1]);
    }
}
```

