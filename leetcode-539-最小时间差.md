---
title: leetcode-539-最小时间差
date: 2022-01-18 22:39:13
categories: LeetCode
tags: [LeetCode,algorithm]
---

[539. 最小时间差](https://leetcode-cn.com/problems/minimum-time-difference/)

![image-20220118224001212](https://gitee.com/cao_ziqiang/img/raw/master/20220118224001.png)

<hr/>

题目要求的是最小时间差,那么最小时间差肯定是在相邻元素之间,对于每个字符串,我们将其转换为数字类型,再对得到的时间数组排序,最小时间差就出现在两个相邻的时间或是首尾两个时间，因此从前往后遍历一趟即可得到最小时间差。

```java
class Solution {
    private static final int BOUND = 24 * 60;
    public int findMinDifference(List<String> timePoints) {
        int n = timePoints.size();
        if(n == 0) {
            return 0;
        }
        // List<Integer> times = new ArrayList<>(n);
        int[] times = new int[n];
        for(int i = 0; i < n; i++) {
            times[i] = str2Int(timePoints.get(i));
        }
        Arrays.sort(times);
        int ans = Integer.MAX_VALUE;
        for(int i = 1; i <= n; i++) ans = Math.min((times[i % n] - times[(i - 1) % n] + BOUND) % BOUND, ans);
        return ans;
    }
    private int str2Int(String time) {
        String[] split = time.split(":");
        return Integer.parseInt(split[0]) * 60 + Integer.parseInt(split[1]);
    }
}
```

