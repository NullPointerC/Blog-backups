---
title: leetcode-300-最长递增子序列
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: 3808c419
date: 2021-09-16 12:05:40
---

[$link$](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

<hr/>

![image-20210916120705728](https://gitee.com/cao_ziqiang/img/raw/master/20210916120705.png)

<hr/>

记$f[i]$表示以$nums[i]$结尾的最长递增子序列长度,当我们每遍历到一个位置时,都和该位置之前的所有子序列相比,找到比当前元素更小的最后一个子序列元素,并将其拼接上子序列,更新$f[i]$

用方程表示则如下:

$f[i] = max(f[j]) + 1$

其中$0≤j<i$且$nums[j]<nums[i]$

$LIS_{length} = max(f[i])$,最后把这个值返回即可.

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int len = nums.length;
        if(len == 0) {
            return 0;
        }
        // f[i]表示以nums[i-1]结尾的递增子序列长度
        int[] f = new int[len + 1];
        Arrays.fill(f,1);
        int ans = Integer.MIN_VALUE;
        for(int i = 0;i<len;i++) {
            for(int j = 0; j < i;j++) {
                if(nums[i] > nums[j]) {
                    f[i] = Math.max(f[i],f[j] + 1);
                }
            }
        }
        for(int i = 0;i<len+1;i++) {
            if(f[i] > ans) {
                ans = f[i];
            }
        }
        return ans;
    }
}
```

