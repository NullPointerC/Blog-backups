---
title: leetcode-918-环形子数组的最大和
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: 7c04f1ea
date: 2021-10-13 17:53:23
---

[$link$](https://leetcode-cn.com/problems/maximum-sum-circular-subarray/)

![image-20211013175358795](https://gitee.com/cao_ziqiang/img/raw/master/20211013175358.png)

<hr/>

最大子数组往往需要用到动态规划，但是这里给出的数组可以支持环形，这时就有两种可能的情况，一是在数组中间取得了最大子数组，二是在数组的两端取得了最大子数组。

![img](https://gitee.com/cao_ziqiang/img/raw/master/20211013175611.png)

<hr/>

考虑第一种情况，如果是在中间取得了子数组，则只需要考虑最大即可。

而对于第二种来说，在两端取得了最大的子数组，其和就等于数组的和减去最小的子数组和。这里也存在一种特殊的情况就是数组的元素全为负数，那么这时要取元素中的最大值即可，因为此时元素全为负，累加也只会使结果变小。

```java
class Solution {
    public int maxSubarraySumCircular(int[] nums) {
        int n = nums.length;
        if(n < 2) {
            return nums[0];
        }
        int max = nums[0];
        int min = nums[0];
        int currMax = nums[0];
        int currMin = nums[0];
        int sum = nums[0];
        for(int i = 1; i < n; i++) {
            sum += nums[i];
            currMax = Math.max(currMax + nums[i], nums[i]);
            currMin = Math.min(currMin + nums[i], nums[i]);
            max = Math.max(max, currMax);
            min = Math.min(min, currMin);
        }
        if(max < 0) {
            return max;
        }
        return Math.max(max, sum - min);

    }
}
```

