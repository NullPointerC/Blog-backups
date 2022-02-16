---
title: leetcode-213-打家劫舍Ⅱ
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: 6f47d828
date: 2021-09-27 19:31:12
---

[$link$](https://leetcode-cn.com/problems/house-robber-ii/)

<hr/>

![image-20210927193159205](https://gitee.com/cao_ziqiang/img/raw/master/20210927193159.png)

<hr/>

和"leetcode-198"一样的思路,都是线性dp,但是需要考虑的是第一间房子和最后一间房子不能同时偷了,因为这是环形的.

依然定义$dp$数组同上题，但是分两次进行$dp$。

定义$dp1$为$nums[0...n-2]$，定义$dp2$为$nums[1...n-1]$。

然后再比较二者中的最大值：

```java
class Solution {
    public int rob(int[] nums) {
        int n = nums.length;
        if(n == 0) {
            return 0;
        }
        if(n == 1) {
            return nums[0];
        }
        // dp1从1到n-1
        int[][] dp1 = new int[n+1][2];
        // dp2从2到n
        int[][] dp2 = new int[n+1][2];
        dp1[1][0] = 0;
        dp1[1][1] = nums[0];
        int max1 = nums[0];
        for(int i = 2; i <= n - 1; i++) {
            dp1[i][0] = Math.max(dp1[i - 1][0],dp1[i-1][1]);
            dp1[i][1] = dp1[i-1][0] + nums[i - 1];
        }
        max1 = Math.max(dp1[n-1][0],dp1[n-1][1]);
        dp2[2][0] = 0;
        dp2[2][1] = nums[1];
        int max2 = nums[1];
        for(int i = 3; i <= n ;i++) {
            dp2[i][0] = Math.max(dp2[i - 1][0], dp2[i - 1][1]);
            dp2[i][1] = dp2[i - 1][0] + nums[i - 1];
        }
        max2 = Math.max(dp2[n][0],dp2[n][1]);
        return Math.max(max1,max2);
    }
}
```

也可以考虑三维$dp$，再引入一维记录是否选第一个。

```java
class Solution {
    public int rob(int[] nums) {
        int n = nums.length;
        if(n == 0) {
            return 0;
        }
        if(n == 1) {
            return nums[0];
        }
        if(n == 2) {
            return Math.max(nums[0], nums[1]);
        }
        // 第1维表示到第i个数字,第2维表示是否偷第i个数字,第3维表示是否偷第1个数字
        int[][][] dp = new int[n + 1][2][2];
        // 不选第3个也不选第1个,那么就只能选nums[1]
        dp[3][0][0] = nums[1];
        // 选第3个就不能再选相邻的第2个,也不选第1个,那么就只能选nums[2]
        dp[3][1][0] = nums[2];
        // 不选第3个,选了第1个就不能选相邻的第2个,那么就只能选nums[0]
        dp[3][0][1] = nums[0];
        // 选了第3个,也选了第1个,结果等于二者之和
        dp[3][1][1] = nums[0] + nums[2];
        for(int i = 4; i <= n; i++) {
            // 不选第i个也不选第1个,那么可以选择偷或不偷前一个中的较大的
            dp[i][0][0] = Math.max(dp[i - 1][1][0],dp[i - 1][0][0]);
            // 选了第i个就不能再选前一个,结果等于前一个不偷且不偷第1个
            dp[i][1][0] = dp[i - 1][0][0] + nums[i - 1];
            // 不偷第i个偷第1个,可以选择偷或不偷前一个中的较大的那个
            dp[i][0][1] = Math.max(dp[i - 1][1][1], dp[i - 1][0][1]);
            // 偷第i个且偷第1个
            dp[i][1][1] = dp[i - 1][0][1] + nums[i - 1];
        }
        // 看不偷第1个时的最大值
        int ans = Math.max(dp[n][1][0], dp[n][0][0]);
        // 再看偷第1个但是不偷最后一个,因为不能相邻
        ans = Math.max(ans, dp[n][0][1]);
        return ans;
    }
}
```

