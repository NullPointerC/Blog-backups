---
title: leetcode-674-最长连续递增子序列
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-09-16 15:30:31
---

[$link$](https://leetcode-cn.com/problems/longest-continuous-increasing-subsequence/)

<hr/>

![image-20210916153124308](https://gitee.com/cao_ziqiang/img/raw/master/20210916153124.png)

<hr/>

这题相比起$leetcode300$,整体容易了不少,不需要考虑非连续的情况,所以只需要考虑连续的时候,一旦不连续,将已递增的序列重置为$1$即可。

$Code$

```java
class Solution {
    public int findLengthOfLCIS(int[] nums) {
        int n = nums.length;
        if(n == 0) {
            return 0;
        }
        if(n == 1) {
            return 1;
        }
        int ans = 1;
        int len = 1;
        for(int i = 1;i < n;i++) {
            if(nums[i] > nums[i-1]) {
                len++;
            }else{
                len = 1;
            }
            ans = Math.max(ans, len);
        }
        return ans;
    }
}
```

