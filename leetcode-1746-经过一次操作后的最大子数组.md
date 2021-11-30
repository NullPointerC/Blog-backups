---
title: leetcode-1746-经过一次操作后的最大子数组
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-10-01 15:45:20
---

[$link$](https://leetcode-cn.com/problems/maximum-subarray-sum-after-one-operation/)

<hr/>

![image-20211001154602480](https://gitee.com/cao_ziqiang/img/raw/master/20211001154602.png)

<hr/>

定义$dp[n][2]$，其中$n$为数组长度，$dp[i][0]$表示为到第$i$个为止，没有进行过替换操作时的最大子数组和，

$dp[i][1]$表示为到第$i$个数位置，进行了1次替换操作后的最大子数组和。

$dp[i][0] = max(dp[i - 1][0],0) + nums[i - 1]$，因为没有进行过替换操作，也只能累加前面没有进行替换操作的时的数，也因为当前面的和$\lt$0时，要选择更大的和，就要从自身开始。

$dp[i][1] = max(dp[i - 1][1] + nums[i - 1], max(dp[i - 1][0],0) + nums[i - 1] \times nums[i - 1])$，如果考虑当前进行了1次替换操作，那么就考虑是对第$i$个数操作了还是之前的操作了，如果是第$i$个数，那么和为$max(dp[i-1][0] ,0)+ nums[i-1] \times nums[i - 1]$，如果是之前的数，那么当前的数就不能进行操作，和为$dp[i - 1][1] + nums[i - 1]$。取二者中较大的即可。

```java
class Solution {
    public int maxSumAfterOperation(int[] nums) {
        int n = nums.length;
        if(n == 1) {
            return Math.max(nums[0], nums[0] * nums[0]);
        }
        int[][] dp = new int[n + 1][2];
        int ans = Integer.MIN_VALUE;
        dp[1][0] = nums[0];
        dp[1][1] = nums[0] * nums[0];
        for(int i = 2; i <= n; i++) {
            dp[i][0] = Math.max(dp[i - 1][0], 0) + nums[i - 1];
            dp[i][1] = Math.max(dp[i - 1][1] + nums[i - 1], Math.max(0, dp[i - 1][0]) + nums[i - 1] * nums[i - 1]);
            ans = Math.max(ans, Math.max(dp[i][0], dp[i][1]));
        }
        return ans;
    }
}
```

