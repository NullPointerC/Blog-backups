---
title: leetcode-1480-一维数组的动态和
date: 2021-08-29 09:56:58
categories: LeetCode
tags: [algorithm,Java,LeetCode]
---

[link](https://leetcode-cn.com/problems/running-sum-of-1d-array/)

<hr/>

题目描述:

<pre>
给你一个数组 nums 。数组「动态和」的计算公式为：runningSum[i] = sum(nums[0]…nums[i]) 。
请返回 nums 的动态和。
</pre>

示例1:

<pre>
输入：nums = [1,2,3,4]
输出：[1,3,6,10]
解释：动态和计算过程为 [1, 1+2, 1+2+3, 1+2+3+4] 。
</pre>

示例2:

<pre>
输入：nums = [1,1,1,1,1]
输出：[1,2,3,4,5]
解释：动态和计算过程为 [1, 1+1, 1+1+1, 1+1+1+1, 1+1+1+1+1] 。
</pre>

示例3:

<pre>
输入：nums = [3,1,2,10,1]
输出：[3,4,6,16,17]
</pre>

思路:

<pre>
dp[i]	=>	索引i处的前缀和
dp[i]	=>	dp[i-1]+nums[i]
</pre>

代码:

```java
class Solution {
    public int[] runningSum(int[] nums) {
        int l = nums.length;
        if (l == 0) {
            return new int[]{};
        }
        int[] dp = new int[l];
        dp[0] = nums[0];
        for (int i = 1; i < l; i++) {
            dp[i] = nums[i] + dp[i - 1];
        }
        return dp;
    }
}
```

![image-20210829100005230](https://gitee.com/cao_ziqiang/img/raw/master/20210829100005.png)

