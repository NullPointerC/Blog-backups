---
title: leetcode-740-删除并获得点数
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-10-11 10:33:52
---

[$link$](https://leetcode-cn.com/problems/delete-and-earn/)

<hr/>

![image-20211011103451231](https://gitee.com/cao_ziqiang/img/raw/master/20211011103451.png)

<hr/> 

- 定义$weight[]$权重数组,代表每个数字所占的权重,因为要取一个数字的点数,那么必然是获得所有的这个数字;
- 然后就可以转化为是打家劫舍问题,不能偷相邻元素;

```java
class Solution {
    private static final int len = 10005;
    public int deleteAndEarn(int[] nums) {
        int[] weight = new int[len];
        for(int num : nums) {
            weight[num] += num;
        }
        int[] dp = new int[len];
        dp[0] = 0;
        dp[1] = weight[1];
        for(int i = 2 ;i < len; i++) {
            dp[i] = Math.max(dp[i - 1], dp[i - 2] + weight[i]);
        }
        return dp[len - 1];
    }
}
```

