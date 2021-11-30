---
title: leetcode-413-等差数列划分
date: 2021-10-18 17:58:12
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
---

[$link$](https://leetcode-cn.com/problems/arithmetic-slices/)

<hr/>

![image-20211018175937175](https://gitee.com/cao_ziqiang/img/raw/master/20211018175937.png)

<hr/>

记$dp[i]$为以数字$nums[i-1]$结尾的等差数列子数组个数,则每个位置上的等差子数组个数如下图:

![image-20211018185041593](https://gitee.com/cao_ziqiang/img/raw/master/20211018185041.png)

故易知:$nums[i]-nums[i-1]=nums[i-1]-nums[i-2]$:

$dp[i]=dp[i-1]+1$

最后对$dp$数组求和即所求的所有等差子数组个数。

```java
class Solution {
    public int numberOfArithmeticSlices(int[] nums) {
        int n = nums.length;
        if(n < 3) {
            return 0;
        }
        // 以nums[i-1]结尾子数组包含的等差数列个数
        int[] dp = new int[n + 1];
        int ans = 0;
        for(int i = 3; i <= n; i++) {
            if(nums[i - 1] - nums[i - 2] == nums[i - 2] - nums[i - 3]) {
                dp[i] = dp[i - 1] + 1;
            }else{
                dp[i] = 0;
            }
            ans += dp[i];
        }
        return ans;
    }
}
```

