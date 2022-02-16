---
title: leetcode-1035-不相交的线
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: a9b14448
date: 2021-10-03 12:35:11
---

[$link$](https://leetcode-cn.com/problems/uncrossed-lines/)

<hr/>

![image-20211003123718800](https://gitee.com/cao_ziqiang/img/raw/master/20211003123718.png)

<hr/>

比较容易看出当求解$nums1[0...i - 1]$和$nums2[0...j-1]$的不想交线的问题时需要根据前面的子问题已经求解的问题和当前$nums1[i - 1]$和$nums2[j-1]$二者的关系组合求解。

故定义$dp[i][j]$为$nums1[0...i-1]$和$nums2[0...j-1]$不相交的线数量，又因为一个端点不能有多条线段，所以从1个端点发出的线只能有1条。所以在初始化时，如果遇到相等时，直接剪枝即可。

若$nums1[i - 1]==nums2[j - 1]$，则说明可构成相交线段，$dp[i][j]=dp[i-1][j-1]+1$。

若二者不等，则$dp[i][j]=max(dp[i-1][j],dp[i][j-1])$

```java
class Solution {
    public int maxUncrossedLines(int[] nums1, int[] nums2) {
        int m = nums1.length;
        int n = nums2.length;
        if(m == 0 || n == 0) {
            return 0;
        }
        // 表示nums1[0...i]与nums2[0...j]的相交线的条数
        int[][] dp = new int[m + 1][n + 1];

        for(int i = 1; i <= n; i++) {
            if(nums1[0] == nums2[i - 1]) {
                dp[1][i] = dp[1][i - 1] + 1;
                // 因为不能一个端点多条线段,所以后面的也可以先初始化
                for(int j = i + 1; j <= n; j++) {
                    dp[1][j] = dp[1][i];
                }
                break;
            }
        }
        for(int i = 1; i <= m; i++) {
            if(nums2[0] == nums1[i - 1]) {
                dp[i][1] = dp[i - 1][1] + 1;
                for(int j = i + 1; j <= m; j++) {
                    dp[j][1] = dp[i][1];
                }
                break;
            }
        }
        for(int i = 2; i <= m; i++) {
            for(int j = 2; j <= n;j++) {
                if(nums1[i - 1] == nums2[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                }else{
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }

        return dp[m][n];
    }
}
```

