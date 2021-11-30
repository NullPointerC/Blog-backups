---
title: leetcode-55-跳跃游戏
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-09-04 10:26:33
---

[link](https://leetcode-cn.com/problems/jump-game/)

<hr/>

tags: (dp + 剪枝)

**题目描述:**

<pre>
给定一个非负整数数组 nums ，你最初位于数组的 第一个下标 。
数组中的每个元素代表你在该位置可以跳跃的最大长度。
判断你是否能够到达最后一个下标。
</pre>

**示例1:**

<pre>
输入：nums = [2,3,1,1,4]
输出：true
解释：可以先跳 1 步，从下标 0 到达下标 1, 然后再从下标 1 跳 3 步到达最后一个下标。
</pre>

**示例2:**

<pre>
输入：nums = [3,2,1,0,4]
输出：false
解释：无论怎样，总会到达下标为 3 的位置。但该下标的最大跳跃长度是 0 ， 所以永远不可能到达最后一个下标。
</pre>

**思路:**

<pre>
可以比较明显的看出每一步的结果需要依托前一步的结果,所以是很典型的动态规划;
那么我们使用dp[i]	=>	第i个位置能否到达数组尾部
很容易发现dp[len] =>	num[len-1] > 0 ? true : false;
再从nums尾部开始遍历,遍历到当前位置时,首先看自己后一个位置能否到数组尾,
若自己的后一个位置能到且当前位置nums[i]>0,那么说明当前位置也能到数组尾,
再看如果自己的当前位置不能到时,就看当前位置能到达的位置中是否有能够达到数组尾的
</pre>

**代码:**

```java
class Solution {
    public boolean canJump(int[] nums) {
        int len = nums.length;
        boolean[] dp = new boolean[len + 1];
        Arrays.fill(dp, false);
        if (nums[len - 1] >= 0) {
            dp[len] = true;
        }
        for (int i = len - 1; i >= 1; i--) {
            if (dp[i + 1]) {
                if (nums[i - 1] > 0) {
                    dp[i] = true;
                } else {
                    dp[i] = false;
                }
            } else {
                for (int j = 1; j <= nums[i - 1]; j++) {
                    if (dp[i + j]) {
                        dp[i] = true;
                        break;
                    } else {
                        dp[i] = false;
                    }
                }
            }
        }
        return dp[1];
    }
}
```

补充一个优化后的dp:

```java
class Solution {
    public boolean canJump(int[] nums) {
        int len = nums.length;
        boolean[] dp = new boolean[len];
        Arrays.fill(dp, false);
        dp[len - 1] = nums[len - 1] >= 0;
        for (int i = len - 2; i >= 0; i--) {
            if (nums[i] >= (len - 1 - i)) {
                dp[i] = true;
            } else {
                for (int j = 1; j <= nums[i]; j++) {
                    if (dp[i + j]) {
                        dp[i] = true;
                        break;
                    }
                }
            }
        }
        return dp[0];
    }
}
```

再补充一个贪心

```java
class Solution {
    public boolean canJump(int[] nums) {
        int n = nums.length;
        if (n <= 1) {
            return true;
        }
        int maxDis = nums[0];
        for (int i = 1; i < n - 1; i++) {
            if (i <= maxDis) {
                maxDis = Math.max(maxDis, nums[i] + i);
            } else {
                return false;
            }
        }
        return maxDis >= n - 1;
    }
}
```

<pre>
用maxDis表示自己可达的最大范围,不断在遍历过程中更新maxDis,可以做到在O(n)内得到结果;
并且如果i>maxDis,说明当前不可达,直接return即可,做了一些剪枝处理;
</pre>

