---
title: leetcode-487-最大连续1的个数Ⅱ
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-10-01 14:54:57
---

[$link$](https://leetcode-cn.com/problems/max-consecutive-ones-ii/)

<hr/>

![image-20211001145542848](https://gitee.com/cao_ziqiang/img/raw/master/20211001145542.png)

<hr/>

可以先记录前一次最长连续的1个数，$curr$代表当前连续1数组的长度，当遇到0时，再尝试将两个子数组拼接。

```java
class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int prev = -1;   
        int cur = 0;
        int ans = 0;
        for (int num : nums) {
            if (num == 1) {
                cur += 1;
            } else {
                ans = Math.max(ans, prev + cur + 1);
                prev = cur;
                cur = 0;
            }
        }
        return Math.max(ans, cur + prev + 1);
    }
}
```

