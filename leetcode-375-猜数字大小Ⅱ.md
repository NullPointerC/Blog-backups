---
title: leetcode-375-猜数字大小Ⅱ
date: 2021-11-12 10:52:10
categories: LeetCode
tags: [LeetCode,algorithm]
---

[$link$](https://leetcode-cn.com/problems/guess-number-higher-or-lower-ii/)

<hr/>

![image-20211112105253251](https://gitee.com/cao_ziqiang/img/raw/master/20211112105314.png)

![img](https://gitee.com/cao_ziqiang/img/raw/master/20211112105318.png)

<pre>
输入：n = 10
输出：16
解释：制胜策略如下：
- 数字范围是 [1,10] 。你先猜测数字为 7 。
    - 如果这是我选中的数字，你的总费用为 $0 。否则，你需要支付 $7 。
    - 如果我的数字更大，则下一步需要猜测的数字范围是 [8,10] 。你可以猜测数字为 9 。
        - 如果这是我选中的数字，你的总费用为 $7 。否则，你需要支付 $9 。
        - 如果我的数字更大，那么这个数字一定是 10 。你猜测数字为 10 并赢得游戏，总费用为 $7 + $9 = $16 。
        - 如果我的数字更小，那么这个数字一定是 8 。你猜测数字为 8 并赢得游戏，总费用为 $7 + $9 = $16 。
    - 如果我的数字更小，则下一步需要猜测的数字范围是 [1,6] 。你可以猜测数字为 3 。
        - 如果这是我选中的数字，你的总费用为 $7 。否则，你需要支付 $3 。
        - 如果我的数字更大，则下一步需要猜测的数字范围是 [4,6] 。你可以猜测数字为 5 。
            - 如果这是我选中的数字，你的总费用为 $7 + $3 = $10 。否则，你需要支付 $5 。
            - 如果我的数字更大，那么这个数字一定是 6 。你猜测数字为 6 并赢得游戏，总费用为 $7 + $3 + $5 = $15 。
            - 如果我的数字更小，那么这个数字一定是 4 。你猜测数字为 4 并赢得游戏，总费用为 $7 + $3 + $5 = $15 。
        - 如果我的数字更小，则下一步需要猜测的数字范围是 [1,2] 。你可以猜测数字为 1 。
            - 如果这是我选中的数字，你的总费用为 $7 + $3 = $10 。否则，你需要支付 $1 。
            - 如果我的数字更大，那么这个数字一定是 2 。你猜测数字为 2 并赢得游戏，总费用为 $7 + $3 + $1 = $11 。
在最糟糕的情况下，你需要支付 $16 。因此，你只需要 $16 就可以确保自己赢得游戏。
</pre>



题目就看了半天，又是不会做的一天，自信快被这连着几天的困难题和$dp$打击没了。

一开始以为是二分查找啊，没想到又是动态规划。

区间$dp$。定义$dp[i][j]$为从区间$[i,j]$确保猜出正确数字的最小代价。

当在$[i,j]$中猜到数字$x$并且猜错了时，需要花费的代价为$x + max(dp[i][x-1]+dp[x][j])$。

即在猜大或猜小的那边找最大的确保一定能找到，所以要取二者中的最大值。

为了确保$dp[i][j]$正确性，需要遍历从$[1,n]$之间的所有$x$ ，使得$dp[1][n]$的值最小。

当$i =j$时，很明显只有一个数字可以猜，所以不需要猜，花费的代价为0；

当$i > j$时，不存在这样的区间，所以也可以认为代价为0；

所以只有$i < j$时，这样的区间才存在，需要计算这个二维矩阵的右上部分。

```java
class Solution {
    private static final int[][] f = new int[201][201];
    public int getMoneyAmount(int n) {
        for (int i = n - 1; i >= 1; i--) {
            for (int j = i + 1; j <= n; j++) {
                int minCost = Integer.MAX_VALUE;
                for (int k = i; k < j; k++) {
                    int cost = k + Math.max(f[i][k - 1], f[k + 1][j]);
                    minCost = Math.min(minCost, cost);
                }
                f[i][j] = minCost;
            }
        }
        return f[1][n];
    }
}
```

 

Go:

```go
var dp [][]int
func init() {
    n := 200
    dp = make([][]int, n+1)
    for i := 1; i <= n; i++ {
        dp[i] = make([]int, n+1)
    }
    for i := n-1; i >= 1; i-- {
        for j := i+1; j <= n; j++ {
            dp[i][j] = j + dp[i][j-1]
            for k := i; k < j; k++ {
                dp[i][j] = min(dp[i][j], k+max(dp[i][k-1], dp[k+1][j]))
            }
        }
    }
}

func getMoneyAmount(n int) int {
    return dp[1][n]
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}

func min(a, b int) int {
    if a > b {
        return b
    }
    return a
}
```

