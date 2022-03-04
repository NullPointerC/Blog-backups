---
title: leetcode-2104-子数组范围和
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: 2a13d827
date: 2022-03-04 13:18:22
---

[2104. 子数组范围和](https://leetcode-cn.com/problems/sum-of-subarray-ranges/)

![image-20220304131900242](https://gitee.com/cao_ziqiang/img/raw/master/20220304131900.png)

<hr/>

比较好想到的解法是区间dp。

因为当我们已知区间`[l...r-1]`的最值后，即可根据`[l...r-1]`的最值和`nums[r]`推导出`[l...r]`区间的最值。

如区间`[l...r]`的最小值`min([l...r])=min(min([l...r-1]), nums[r])`，对于最大值来说也是同理。

使用`dp[l][r][0]`表示区间`[l...r]`的最小值，`dp[l][r][1]`表示为区间`[l...r]`的最大值。

可以得出`dp[l][r][0]=min(dp[l][r-1][0], nums[r])`,`dp[l][r][1]=max(dp[l][r-1][1], nums[r])`。

所以我们可以枚举每一个起点`l`和这个起点可能的长度`len`，根据`dp`公式枚举出每个区间的最值，然后把所有区间的最值相加即可。

```java
class Solution {
    public long subArrayRanges(int[] nums) {
        int n = nums.length;
        // dp[l][r][0]表示[l,r]区间内的最小值,dp[l][r][1]表示[l,r]区间内的最大值
        long[][][] dp = new long[n][n][2];
        // 初始化
        for(int i = 0; i < n; i++) {
            dp[i][i][0] = nums[i];
            dp[i][i][1] = nums[i];
        }
        for(int len = 2; len <= n; len++) {
            for(int l = 0; l + len - 1 < n; l++) {
                int r = l + len - 1;
                dp[l][r][0] = Math.min(dp[l][r - 1][0], nums[r]);
                dp[l][r][1] = Math.max(dp[l][r - 1][1], nums[r]);
            }
        }
        long res = 0;
        for(int i = 0; i < n; i++) {
            for(int j = i; j < n; j++) {
                res += (dp[i][j][1] - dp[i][j][0]);
            }
        }
        return res;
    }
}
```



