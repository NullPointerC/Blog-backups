---
title: leetcode-494-目标和
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: b5d89e0b
date: 2022-02-17 16:13:22
---

[494. 目标和](https://leetcode-cn.com/problems/target-sum/)

![image-20220217161419294](https://gitee.com/cao_ziqiang/img/raw/master/20220217161419.png)

很显然递归是一种比较方便的解法,我们可以暴搜每一个位置填`+`或`-`时的结果,并以这个结果继续递归下去,若递归到底了且结果为0时,说明这是一种解法,我们加上这次的结果即可。

```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        return dfs(nums, 0, target);
    }

    private int dfs(int[] nums, int pos, int cur) {
        if(pos == nums.length) {
            if(cur == 0) {
                return 1;
            } else {
                return 0;
            }
        }
        return dfs(nums, pos + 1, cur + nums[pos]) + dfs(nums, pos + 1, cur - nums[pos]);
    }
}
```

改记忆化搜索

```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        if(nums == null || nums.length == 0) {
            return 0;
        }
        Map<Integer, Map<Integer, Integer>> dp = new HashMap<>();
        return dfs(nums, 0, target, dp);
    }

    private int dfs(int[] nums, int pos, int cur, Map<Integer, Map<Integer, Integer>> dp) {
        if(dp.containsKey(pos) && dp.get(pos).containsKey(cur)) {
            return dp.get(pos).get(cur);
        }
        int ans = 0;
        if(pos == nums.length) {
            if(cur == 0) {
                ans = 1;
            }
        } else {
            ans += (dfs(nums, pos + 1, cur - nums[pos], dp) + dfs(nums, pos + 1, cur + nums[pos], dp));
        }
        if(!dp.containsKey(pos)) {
            dp.put(pos, new HashMap<>());
        }
        dp.get(pos).put(cur, ans);
        return ans;
    }
}
```

还有一种方法是数学方法，自己属实是没有想出来。

令`P`集合是符号取正的数，`N`集合是符号取负的数，那么显然有`sum(P)`-`sum(N)`=`target`。

两边同时加上`sum(P)`和`sum(N)`。

则`2 * sum(P) = target + sum`。

于是我们只需要找到这样的集合`P`使得`P`中所有元素之和等于右边的和除以2即可。

于是转化为一个背包问题，在`nums`中找到若干个数之和为`(targe + sum) / 2`的方案数。

```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        int sum = 0;
        for (int num: nums) {
            sum += num;
        }
        if (sum < target || (sum + target) % 2 == 1 || sum + target < 0) return 0;
        int bagSize = (sum + target) / 2;
        int[] dp = new int[bagSize + 1];
        dp[0] = 1;
        for (int i = 0; i < nums.length; i++) {
            for (int j = bagSize; j >= nums[i]; j--) {
                dp[j] += dp[j-nums[i]];
            }
        }
        return dp[bagSize];
    }
}
```

