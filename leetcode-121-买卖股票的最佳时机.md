---
title: leetcode-121-买卖股票的最佳时机
date: 2021-07-04 11:15:07
categories: LeetCode
tags: [algorithm,Java,LeetCode]
---

[link](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

![image-20210704112651973](https://gitee.com/cao_ziqiang/img/raw/master/20210704112652.png)

<hr/>

看到这种题目,求数组中最值,而且是一个变化的过程的,一般我们都要想到动态规划,并且,我们要看这题的两个限制,一个是买入时的价格,一个是卖出时获得的利润,那么我们设dp为第i天能够获得的最大利润,设buy为这个购买过程中最小的买入价格

那么在遍历过程中我们要维护的就是两个值

<pre>
buy = min(price, buy);
dp(i) = max(dp(i-1), price - buy);
</pre>

这里两个dp代表不同的含义,后一个dp代表的是前面i-1天的最大利润,和当天的价格减去最低价的比较

dp的状态转移方程列好了之后,我们再看dp初值和buy初值应该怎么设

很明显buy初值应该设置为prices[0],代表第一天买入的价格

dp因为还没有买入,应该设置为0

代码:

```java
class Solution {
    public int maxProfit(int[] prices) {
        int l = prices.length;
        if(l == 0 || null == prices) {
            return 0;
        }
        int dp = 0;
        int buy = prices[0];
        for(int price : prices) {
            buy = Math.min(price, buy);
            dp = Math.max(dp, price - buy);
        }
        return dp;
    }
}
```

![image-20210704113151003](https://gitee.com/cao_ziqiang/img/raw/master/20210704113151.png)

<hr/>

![image-20210704113446481](https://gitee.com/cao_ziqiang/img/raw/master/20210704113446.png)

