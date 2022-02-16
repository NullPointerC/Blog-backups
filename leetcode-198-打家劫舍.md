---
title: leetcode-198-打家劫舍
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: bc8418f1
date: 2021-09-27 18:07:28
---

[link](https://leetcode-cn.com/problems/house-robber/submissions/)

<hr/>

![image-20210927180827323](https://gitee.com/cao_ziqiang/img/raw/master/20210927180827.png)

<hr/>

对于每一间房屋,考虑是偷还是不偷,定义$dp[i][0]$表示为不偷第$i-1$间房屋的情况下的最高金额,$dp[i][1]$为偷第$i-1$间房屋的情况下的最高金额。

如果不偷第$i-1$间房屋，考虑可以是偷前一间，也可以考虑是不偷前一间，所以$dp[i][0] = max(dp[i-1][1],dp[i-1][0])$

如果偷了第$i-1$间房屋，就不能偷前一间，此时不需要考虑前一间偷不偷的问题，只能选择是不偷前一间并偷这一间，方程表示为：$dp[i][1]=dp[i-1][0]+nums[i-1]$ 

```java
class Solution {
    public int rob(int[] nums) {
        int n = nums.length;
        int[][] dp = new int[n + 1][2];
        // base case
        dp[1][1] = nums[0];
        dp[1][0] = 0;
        int ans = nums[0];
        for(int i = 2; i <= n; i++) {
            dp[i][0] = Math.max(dp[i - 1][1],dp[i - 1][0]);
            dp[i][1] = dp[i - 1][0] + nums[i - 1];
            ans = Math.max(dp[i][0], dp[i][1]);
        }
        return ans;
    }
}
```

再考虑这样的一个问题，输出打劫的路径：

可以在得到$ans$后，从尾部开始选择已经偷了的节点$dp[i][1]$，判断其值是否为$target$，是则添加进路径里并减去当前的选择的数$nums[i-1]$，直到遍历完。

```java
class Solution {
    public int rob(int[] nums) {
        int n = nums.length;
        int[][] dp = new int[n + 1][2];
        // base case
        dp[1][1] = nums[0];
        dp[1][0] = 0;
        int ans = nums[0];
        for(int i = 2; i <= n; i++) {
            dp[i][0] = Math.max(dp[i - 1][1],dp[i - 1][0]);
            dp[i][1] = dp[i - 1][0] + nums[i - 1];
            ans = Math.max(dp[i][0], dp[i][1]);
        }
        int target = ans;
        ArrayList<Integer> path = new ArrayList<>();
        for(int i = n; i >= 1; i--) {
        	if(dp[i][1] == target) {
        		path.add(nums[i - 1]);
        		target -= nums[i - 1];
        	}
        }
        System.out.println(path);
        return ans;
    }
}
```

