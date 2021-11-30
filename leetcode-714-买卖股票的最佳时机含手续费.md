---
title: leetcode-714-买卖股票的最佳时机含手续费
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-09-29 21:03:27
---

[$link$](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)

<hr/>

![image-20210929210515713](https://gitee.com/cao_ziqiang/img/raw/master/20210929210515.png)

<hr/>

定义$dp[i][j]$为第$i$天时股票状态为$j$手上的金额数量。

对于初始情况，如果第一天没有股票，也就是没有买股票，$dp[1][0] = 0$

如果第一天买了股票，那么手上的金额就应该为:$dp[1][1] = 0 - prices[0]$。

因为我们可以认为本金是$0$。

到了后面，如果第$i$天没有股票，可能今天卖了,也可能昨天就没有。

今天卖了的话，就可以得到昨天手上还有的金额加上今天卖出的价格再减去手续费，故：

$dp[i][0] = Math.max(dp[i - 1][0], prices[i - 1] + dp[i - ][1]-fee)$

如果第$i$天有股票，可能是昨天买了剩下的，也可能是昨天没买，今天刚买的

$dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] - price[i -1])$

到了最后一天，手上的股票都要卖出去，手上不能再留了，再留只会让自己亏本！！！

```java
class Solution {
    public int maxProfit(int[] prices, int fee) {
        int n = prices.length;
        if(n == 1) {
            return 0;
        }
        // 第i天不持股/持股手上的金额
        int[][] dp = new int[n + 1][2];
        // 第一天不买手上金额就是0
        dp[1][0] = 0;
        // 第一天买入手上金额就要减少
        dp[1][1] = -prices[0];

        for(int i = 2; i <= n; i++) {
            // 手上没有股票,可能今天卖了,也可能昨天就没有
            dp[i][0] = Math.max(dp[i - 1][0], prices[i - 1] + dp[i - 1][1] - fee); 
            // 手上有股票, 可能昨天就有,也可能昨天没有,今天刚买的
            dp[i][1] = Math.max(dp[i - 1][1] ,dp[i - 1][0] - prices[i - 1]);
        }
        // 最后一天要把股票都卖出去
        return dp[n][0];
    }
}
```

<hr/>

也可以使用贪心进行分析，假设$min$为买入股票的价格最低值,而如果当天的卖出价减去交易费再减去买入股票的价格大于0时,我们就选择将其卖掉。这样做可以使得在一定区间内是最稳妥的办法，也是最赚的办法。

拓展到全局，如果一次交易的卖出价减去交易费再减去买入费是正的，就说明这次交易时获利的，多进行几次这样的交易，这样的交易在局部获得了最优，故全局也能获得最优。

在考虑买入价，当一次交易完成后，下一次的买入价应该要比这次交易的获利更低才可以保证不亏，否则可能会亏。

这一次的获利为$prices[i]-fee$，故只有当买入价低于这个值时，才选择买入，

```java
class Solution {
    public int maxProfit(int[] prices, int fee) {
        int ans = 0, min = prices[0], n = prices.length;
        for(int i = 0; i < n; i++) {
            if(prices[i] < min) {
                // 有更低的买入价
                min = prices[i];
            }else if (prices[i] - min - fee > 0) {
                // 选择卖出
                ans += (prices[i] - min - fee);
                min = prices[i] - fee;
            }
        }
        return ans;
    }
}
```

