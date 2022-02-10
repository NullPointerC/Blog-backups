---
title: leetcode-1984-学生分数的最小差值
date: 2022-02-11 01:22:25
categories: LeetCode
tags: [LeetCode,algorithm]
---

[1984. 学生分数的最小差值](https://leetcode-cn.com/problems/minimum-difference-between-highest-and-lowest-of-k-scores/)

![image-20220211012258859](https://gitee.com/cao_ziqiang/img/raw/master/20220211012258.png)

一个比较简单的办法是我们需要尽量使最大最小值相近，那么我们对数组排序，这样$nums[i]$和$nums[i + k - 1]$就是否接近。

```java
class Solution {
    public int minimumDifference(int[] nums, int k) {
        int n = nums.length;
        if(n == 0) {
            return 0;
        }
        if(k < 2) {
            return 0;
        }
        Arrays.sort(nums);
        int res = Integer.MAX_VALUE;
        for(int i = 0; i < n - k + 1; i++) {
            res = Math.min(nums[i + k - 1] - nums[i], res);
        }
        return res;
    }
}
```

这样枚举最小的即可。

