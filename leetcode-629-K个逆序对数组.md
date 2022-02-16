---
title: leetcode-629-K个逆序对数组
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: ff194c2a
date: 2021-11-11 20:47:22
---

[$link$](https://leetcode-cn.com/problems/k-inverse-pairs-array/)

<hr/>

![image-20211111204948107](https://gitee.com/cao_ziqiang/img/raw/master/20211111204948.png)

<hr/>

今天的困难题对我这种小菜鸡来说还是很难的,看了一晚上的题解才敢比较模糊的写出自己的理解。

看到这种比较明显的依赖前面的结果的题就可以想到动态规划上去。

要求的是$n$个数组成$k$个逆序对的序列，考虑能否从前$n-1$个数的序列转移过来。

考虑$n$个数的序列比起$n-1$的序列多了最后一个$n$，这个$n$显然比序列中前面的所有数都更大，而且这个$n$可以选择加入在$n-1$个数的序列中的不同位置。

若前$n-1$个数的序列已经有$j$个逆序对，那么直接在这个序列末尾添加即可，得到的新序列也是$j$个逆序对，此时$dp[i][j]+=dp[i-1][j]$；

若前$n-1$个数的序列已经有$j-1$个逆序对，那么可以将$n$插入到这个序列的倒数第二位，这样就可以额外获得一组逆序对，总的来说还是得到了$j$个逆序对，此时$dp[i][j]+=dp[i-1][j-1]$；

依次类推，若前$n-1$个数的序列有$0(j-j)$个逆序对，那么可以将$n$插入到这个数组的倒数第$k+1$位，这就得到了$k$个新的逆序对，即原序列的最后$k$位与$n$组成的逆序对，此时$dp[i][j]+=dp[i-1][0]$

由此推导出：$dp[i][j]=\sum_{x=0}^{j}dp[i][x]$。

公式推导完了，剩下来的是处理边界问题，首先是上述的倒数第$x$位，不能超过数组的下标$0$。因为加上1个$n$最多可能增加$i$个逆序对，所以前$n-1$个数中最多可能正好有$k-i$个逆序对，故$n-1\ge k-i \Rightarrow i \ge (k+1-n)$；

第二个边界是，对于$n$个元素的序列最多有多少个逆序对，这种情况很明显是所有元素都是逆序的排列，那么就是有$max = \sum_{1}^{n-1}=n\times(n-1)/2$，故若$k \gt max \Rightarrow 0$。

所以，假如写成暴力递归则是：

```java
class Solution {
	private static final int MOD = (int) 1e9 + 7;
    public int kInversePairs(int n, int k) {
        return (int)(dfs(n, k) % MOD);
    }
    private long dfs(int n, int k) {
        if (k > n * (n - 1) / 2) {
            return 0;
        }
        if (n == 1) {
            return k == 0 ? 1 : 0;
        }
        long ans = 0;
        for(int i = Math.max(0, k - n + 1); i <= k; ++i) {
            ans += dfs(n - 1, i);
        }
        return ans % MOD;
    }
}
```

换成动态规划：

```java
class Solution {
    private static final int MOD = (int)1e9 + 7;
    public int kInversePairs(int n, int k) {
        long[][] dp = new long[n + 1][k + 1];
        dp[1][0] = 1;
        for (int i = 2; i <= n; i++) {
            for (int j = 0; j <= Math.min(k, i * (i - 1) / 2); j++) {
                for (int x = Math.max(0, j - i + 1); x <= j; x++) {
                    dp[i][j] = (dp[i][j] + dp[i - 1][x]) % MOD;
                }
            }
        }
        return (int) dp[n][k];
    }
}
```

通过观察可以分析出，其实再计算$dp[i][j]$这一步的时候，前面的$dp[i-1]$已经被计算过了。

$dp[n+1][k]=dp[n][k]+dp[n][k-1]+...+dp[n][k-n+1]$；

$dp[n+1][k+1]=dp[n][k+1]+dp[n][k]+...+dp[n][k-n+2]$；

二者相减得：

$dp[n+1][k+1]-dp[n+1][k]=dp[n][k+1]-dp[n][k-n+1]$

故$dp[n][k]=dp[n][k-1]+dp[n-1][k]-dp[n-1][k-n]$；

```java
class Solution {
    private static final int MOD = (int)1e9 + 7;
    public int kInversePairs(int n, int k) {
        // dp[i][j] = dp[i-1][j] + dp[i-1][j-1] + ... + dp[i-1][j-i-1]
        // dp[i][j-1] =            dp[i-1][j-1] + ... + dp[i-1][j-1-(i-1-1)] + dp[i-1][j-1-i-1]
        // dp[i][j] = dp[i-1][j] + dp[i][j-1] - dp[i-1][j-1-i-1]
        long[][] dp = new long[n + 1][k + 1];
        dp[1][0] = 1;
        for (int i = 2; i <= n; i++) {
            for (int j = 0; j <= Math.min(k, i * (i - 1) / 2); j++) {
                // dp[i][j] = dp[i-1][j] + dp[i][j-1] - dp[i-1][j-1-i-1]
                dp[i][j] = ((j >= 1 ? dp[i][j - 1] : 0) + dp[i - 1][j] - (j >= i ? dp[i - 1][j - i] : 0) + MOD) % MOD;
            }
        }
        return (int) dp[n][k];
    }
}
```

