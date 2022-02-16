---
title: leetcode-689-三个无重叠子数组的最大和
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: fc3891d
date: 2021-12-08 19:46:00
---

[三个无重叠子数组的最大和](https://leetcode-cn.com/problems/maximum-sum-of-3-non-overlapping-subarrays/)

<hr/>

![image-20211208194656336](https://gitee.com/cao_ziqiang/img/raw/master/20211208194656.png)

<hr/>

tags：(滑动窗口)

属实是没有往滑动窗口上想。

先考虑原问题的简化形式，找2个这样的数组。如果是两个这样的数组，假设为$[x_1,x_2]$和$[x_3,x_4]$且($x_1\le x_2 \le x_3 \le x_4$)，固定区间$[x_3,x_4]$，那么区间$[x_1,x_2]$就是$[0,x_3-1]$中长度为$k$的所有区间的中的最大值，这个最大值可以在从小到大枚举区间$[x_3,x_4]$可以找出。

现在演化到找3个这样的无重叠数组。同理这3个区间也是没有重叠的区间，假设为$[x_1,x_2]$、$[x_3,x_4]$和$[x_5,x_6]$且满足条件$x_1\le x_2 \le x_3 \le x_4 \le x_5 \le x_6$。我们此时依然固定区间$[x_5,x_6]$，那么问题就转化为在$[0,x_5-1]$这个区间中找出两个区间，使得和最大，回到了上一步中简化的问题。

假设$idx[]$为我们维护的3个区间的起始下标，$sum_1,sum_2,sum_3$分别为这三个区间的和，并分别维护区间1，区间1和区间2，区间1，区间2和区间3的最大和$maxSum_1,maxSum_{12},maxSum_{123}$。

$sum_1+=nums[i-2\times k]$;

$sum_2+=nums[i-k]$;

$sum_3+=nums[i]$;

依次枚举每一个$[x_5,x_6]$区间，分析易知，$x_5$最少要从$2\times k$开始，$x_6$为$3\times k-1$时，三个窗口的值都可以取到，开始维护最大值。

```java
class Solution {
    public int[] maxSumOfThreeSubarrays(int[] nums, int k) {
        int n = nums.length;
        if (3 * k > n) {
            return null;
        }
        int[] idx = new int[4];
        int[] ans = new int[3];
        int sum1 = 0, sum2 = 0, sum3 = 0;
        int maxSum1 = 0, maxSum12 = 0, maxSum123 = 0;
        for(int i = 2 * k; i < n; i++) {
            // 维护3个窗口的各自和
            sum1 += nums[i - 2 * k];
            sum2 += nums[i - k];
            sum3 += nums[i];
            if (i >= 3 * k - 1) {
                // 存在有3个不重叠的窗口了
                if (sum1 > maxSum1) {
                    idx[0] = i - 3 * k + 1;
                    maxSum1 = sum1;
                }
                if (sum2 + maxSum1 > maxSum12) {
                    idx[1] = idx[0];
                    idx[2] = i - 2 * k + 1;
                    maxSum12 = sum2 + maxSum1;
                }
                if (sum3 + maxSum12 > maxSum123) {
                    idx[3] = i - k + 1;
                    maxSum123 = sum3 + maxSum12;
                    ans = new int[]{idx[1], idx[2], idx[3]};
                }
                sum1 -= nums[i - 3 * k + 1];
                sum2 -= nums[i - 2 * k + 1];
                sum3 -= nums[i - k + 1];
            }
        }
        return ans;
    }
}
```

这里有一个细节就是，$idx[1]=idx[0]$这步，因为题目也说到了，我们要返回字典序最小的那个，第一个区间的值发生了变化，如果此时第二个区间的最大值发生了变化，也要记录下此时的第一个区间起点，所以这里需要记录一下。

