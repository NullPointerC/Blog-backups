---
title: leetcode-122-买卖股票的最佳时机Ⅱ
date: 2021-07-17 21:22:46
categories: algorithm
tags: [algorithm,Java,LeetCode]
---

[Link](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)

<hr/>

## 题目描述：

<pre>
给定一个数组 prices ，其中 prices[i] 是一支给定股票第 i 天的价格。
设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。
注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。
</pre>

## 示例1：

<pre>
输入: prices = [7,1,5,3,6,4]
输出: 7
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 。
</pre>

## 示例2：

<pre>
输入: prices = [1,2,3,4,5]
输出: 4
解释: 在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。
</pre>

## 示例3：

<pre>
输入: prices = [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
</pre>
## 思路：

<pre>
由于允许多次交易，我们定义dp数组应该是dp[i][j]表示下标为i的这一天,持股状态为j时,我们拥有的最大现金数
第一维 i 表示下标为 i 的那一天（ 具有前缀性质，即考虑了之前天数的交易 ）;
第二维 j 表示下标为 i 的那一天是持有股票,还是持有现金。这里 0 表示持有现金（cash），1 表示持有股票（stock）。
起始的时候：如果什么都不做，dp[0][0] = 0,如果持有股票,当前拥有的现金数是当天股价的相反数,即 dp[0][1] = -prices[i];
在dp的过程中：如果没有持有股票,可能前一天已经没有股票或今天将其卖出并获得了prices[i]的收益
如果持有股票,可能前一天已经持有股票或今天将其买入减去了prices[i]的收益
</pre>



## 代码：

```java
class Solution {
    public int maxProfit(int[] prices) {
        int l = prices.length;
        if (l == 0 || null == prices) {
            return 0;
        }
        /*定义dp数组
         * dp[i][j]表示下标为i的这一天,持股状态为j时,我们拥有的最大现金数
         * 第一维 i 表示下标为 i 的那一天（ 具有前缀性质，即考虑了之前天数的交易 ）;
         * 第二维 j 表示下标为 i 的那一天是持有股票,还是持有现金。这里 0 表示持有现金（cash），1 表示持有股票（stock）。
         *
         * */
        /*起始的时候：
         * 如果什么都不做，dp[0][0] = 0;
         * 如果持有股票,当前拥有的现金数是当天股价的相反数,即 dp[0][1] = -prices[i];
         * */
        int[][] dp = new int[l][2];
        dp[0][0] = 0;
        dp[0][1] = -prices[0];
        for (int i = 1; i < l; i++) {
            // 这两行调换顺序也是可以的
            /*如果没有持有股票,可能前一天已经没有股票或今天将其卖出并获得了prices[i]的收益*/
            dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + prices[i]);
            /*如果持有股票,可能前一天已经持有股票或今天将其买入减去了prices[i]的收益*/
            dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] - prices[i]);
        }
        return dp[l - 1][0];
    }
}
```

![image-20210719125247842](https://gitee.com/cao_ziqiang/img/raw/master/20210719125248.png)

也在评论区看到有人说，可以用纯数学思路。

只要股票涨了，就立即卖出。

```java
class Solution {
    public int maxProfit(int[] prices) {
        int ans=0;
        for(int i=1;i<=prices.length-1;i++)
        {
            if(prices[i]>prices[i-1])
            {
                ans+=prices[i]-prices[i-1];
            }
        }
        return ans;
    }
}
```

