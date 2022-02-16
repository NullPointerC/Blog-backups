---
title: leetcode-377-组合总和Ⅳ
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: fc2fccaf
date: 2021-10-29 09:58:42
---

[$link$](https://leetcode-cn.com/problems/combination-sum-iv/)

<hr/>

![image-20211029161823371](https://gitee.com/cao_ziqiang/img/raw/master/20211029161823.png)

<hr/>

比较经典的背包问题，定义$dp[i]$为整数$i$可以由$dp[i]$个组合个数。

```java
class Solution {
    public int combinationSum4(int[] nums, int target) {
        int[] dp = new int[target + 1];
        int n = nums.length;
        dp[0] = 1;
        for(int i = 1; i <= target; i++) {
            // 遍历背包
            for(int num : nums) {
                if(i >= num) {
                    dp[i] += dp[i - num];
                }
            }
        }
        return dp[target];
    }
}
```

