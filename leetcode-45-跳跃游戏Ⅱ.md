---
title: leetcode-45-跳跃游戏Ⅱ
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-08-28 09:27:34
---

[link](https://leetcode-cn.com/problems/jump-game-ii/)

<hr/>

(tags:dp)

题目描述:

<pre>
给你一个非负整数数组 nums ，你最初位于数组的第一个位置。
数组中的每个元素代表你在该位置可以跳跃的最大长度。
你的目标是使用最少的跳跃次数到达数组的最后一个位置。
假设你总是可以到达数组的最后一个位置。
</pre>

示例1:

<pre>
输入: nums = [2,3,1,1,4]
输出: 2
解释: 跳到最后一个位置的最小跳跃数是 2。
     从下标为 0 跳到下标为 1 的位置，跳 1 步，然后跳 3 步到达数组的最后一个位置。
</pre>

**示例 2:**

<pre>
输入: nums = [2,3,0,1,4]
输出: 2
</pre>

思路:

<pre>
一般这类在数组中求最值的问题都会用到前面计算的结果,所以使用动态规划求解。
我们使用dp[i]表示第i个位置到数组尾部的最小跳跃次数,从尾部开始遍历数组
base case为数组尾部的第一个元素是否大于0,大于则表示无需操作即可到达尾部,dp[len] = 0;
再从len-1位置开始往前遍历,如果nums[i-1] > len - i,则表示可以直达尾部;
如果不大于,则需要通过此位置可达的其他位置到达尾部;
再遍历nums[i-1]处可达的位置,得到最小跳跃次数,更新dp[i];
最终返回dp[1]
</pre>

代码:

```java
class Solution {
    public int jump(int[] nums) {
int len = nums.length;
        if (len == 0) {
            return 0;
        }
        // 代表从第i个位置到数组尾部所需要的次数
        int[] dp = new int[len + 1];
        Arrays.fill(dp, Integer.MAX_VALUE - 1);
        if (nums[len - 1] >= 0) {
            dp[len] = 0;
        }
        for (int i = len - 1; i > 0; i--) {
            if (nums[i - 1] >= len - (i)) {
                dp[i] = 1;
            } else {
                for (int j = 0; j < nums[i - 1]; j++) {
                    dp[i] = Math.min(dp[i], 1 + dp[i + 1 + j]);
                }
            }
        }
        // System.out.println(Arrays.toString(dp));
        return dp[1];
    }
}
```

![image-20210828093511152](https://gitee.com/cao_ziqiang/img/raw/master/20210828093511.png)

<pre>
也可以对dp进行优化,可以从头开始dp
</pre>

```java
class Solution {
    public int jump(int[] nums) {
        int n = nums.length;
        int dp[] = new int[n + 1];
        Arrays.fill(dp, Integer.MAX_VALUE);
        if(n == 1) return 0;
        dp[0] = 0;
        for(int i = 0; i < n; i++){
            for(int j = 0; j <= nums[i] && i + j < n; j++){
                dp[i + j] = Math.min(dp[i] + 1, dp[i + j]);
            }
        }
        return dp[n - 1];
    }
}
```

![image-20210828100532116](https://gitee.com/cao_ziqiang/img/raw/master/20210828100532.png)

<pre>
最快还是贪心策略,因为题目有个条件说到一定能够到达终点
</pre>

```java
class Solution {
    public int jump(int[] nums) {
        int len = nums.length;
        if (len == 1) {
            return 0;
        }
        // 当前覆盖最远距离下标
        int currDistance = 0;
        // 记录走的步数
        int steps = 0;
        // 下一步覆盖的最远距离下标
        int nextDistance = 0;
        for (int i = 0; i < len; i++) {
            // 更新下一步覆盖最远距离下标
            nextDistance = Math.max(nums[i] + i, nextDistance);
            if (i == currDistance) {
                // 遇到当前覆盖最远距离下标
                if (currDistance != len - 1) {
                    // 如果当前覆盖最远距离下标不是终点
                    // 需要走下一步
                    steps++;
                    // 更新当前覆盖最远距离下标
                    currDistance = nextDistance;
                    // 下一步的覆盖范围已经可以达到终点，结束循环
                    if (nextDistance >= len - 1) {
                        break;
                    }
                } else {
                    // 当前覆盖最远距离下标是集合终点，不用做ans++操作了，直接结束
                    break;
                }
            }
        }
        return steps;
    }
}
```

