---
title: leetcode-712-两个字符串的最小ASCII删除和
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-10-03 13:21:04
---

[$link$](https://leetcode-cn.com/problems/minimum-ascii-delete-sum-for-two-strings/)

<hr/>

![image-20211003132143048](https://gitee.com/cao_ziqiang/img/raw/master/20211003132143.png)

<hr/>

定义$dp[i][j]$为$s1[0...i-1]$到$s2[0...j-1]$所需要删除的$ASCII$和。

初始化时，假定$s1$或$s2$为空串，则$dp[i][0]=dp[i-1][0]+s1[i-1]$，$dp[0][i]=dp[0][i-1]+s2[i-1]$。

对于后续状态转移，若$s1[i-1]==s2[j-1]$。二者都不要删除，故$dp[i][j]=dp[i-1][j-1]$。

若$s1[i-1] \ne s2[j-1]$，可以二者都删除，或删除二者中的较小的一个，这两种情况取更小的那种。

$dp[i][j]=min(dp[i-1][j-1]+s1[i-1]+s2[j-1],dp[i-1][j]+s1[i-1],dp[i][j-1]+s2[j-1])$

```java
class Solution {
    public int minimumDeleteSum(String s1, String s2) {
        int l1 = s1.length(), l2 = s2.length();
        // 代表从s1[0...i-1]到s2[0...j-1]所需要删减字符的ASCII和
        int[][] dp = new int[l1 + 1][l2 + 1];
        for(int i = 1; i <= l1; i++) {
            dp[i][0] = dp[i - 1][0] + s1.charAt(i - 1);
        }
        for(int i = 1; i <= l2 ; i++) {
            dp[0][i] = dp[0][i - 1] + s2.charAt(i - 1);
        }
        for(int i = 1; i <= l1; i++) {
            for(int j = 1; j <= l2; j++) {
                if(s1.charAt(i - 1) == s2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1];
                }else{
                    dp[i][j] = Math.min(dp[i - 1][j - 1] + s1.charAt(i - 1)+ s2.charAt(j - 1),
                    Math.min(dp[i - 1][j] + s1.charAt(i - 1), dp[i][j - 1] + s2.charAt(j - 1)));
                }
            }
        }
        return dp[l1][l2];
    }
}
```

