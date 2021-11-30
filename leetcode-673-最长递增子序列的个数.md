---
title: leetcode-673-最长递增子序列的个数
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-09-20 13:22:40
---

[$link$](https://leetcode-cn.com/problems/number-of-longest-increasing-subsequence/)

<hr/>

![image-20210920132405966](https://gitee.com/cao_ziqiang/img/raw/master/20210920132406.png)

<hr/>

和$leetcode-300$一样的思路,不过这里要记录下所有长度是最长的子序列个数。

记$dp[i]$表示为以$nums[i]$结尾的最长递增子序列的长度，记$cnt[i]$表示为以$nums[i]$结尾的最长递增子序列的个数。

设$nums$的最长地址子序列长度为$maxLength$，那么只需求所有$dp[i] = maxLength$的$i$其对应的$cnt[i]$之和即可。

状态转移方程为$dp[i] = max(dp[i],dp(j)+1),0\le j \lt i$且$nums[j]\lt nums[i]$。

对于$cnt[i]$，其值等于满足$dp[j] + 1 = d[i]$的所有$cnt[j]$之和，因为其由子序列而来，故个数也等于子序列个数。

```java
class Solution {
    public int findNumberOfLIS(int[] nums) {
        int n = nums.length, maxLen = 0, ans = 0;
        int[] dp = new int[n];
        int[] cnt = new int[n];
        for (int i = 0; i < n; ++i) {
            dp[i] = 1;
            cnt[i] = 1;
            for (int j = 0; j < i; ++j) {
                if (nums[i] > nums[j]) {
                    if (dp[j] + 1 > dp[i]) {
                        dp[i] = dp[j] + 1;
                        // 有更长的子序列,重置计数
                        cnt[i] = cnt[j]; 
                    } else if (dp[j] + 1 == dp[i]) {
                        cnt[i] += cnt[j];
                    }
                }
            }
            if (dp[i] > maxLen) {
                maxLen = dp[i];
                ans = cnt[i]; // 重置计数
            } else if (dp[i] == maxLen) {
                ans += cnt[i];
            }
        }
        return ans;
    }
}
```

