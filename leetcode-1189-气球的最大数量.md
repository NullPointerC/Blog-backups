---
title: leetcode-1189-气球的最大数量
date: 2022-02-13 00:20:22
categories: LeetCode
tags: [LeetCode,algorithm]
---

[1189. “气球” 的最大数量](https://leetcode-cn.com/problems/maximum-number-of-balloons/)

![image-20220213002241294](https://gitee.com/cao_ziqiang/img/raw/master/20220213002241.png)

枚举所有字母的出现次数，统计最小的即可。

```java
class Solution {
    public int maxNumberOfBalloons(String text) {
        int res = Integer.MAX_VALUE;
        if(text == null || text.length() == 0) {
            return 0;
        }
        int[] map = new int[26];
        for(var c : text.toCharArray()) map[c - 'a']++;
        res = Math.min(res, map['a' - 'a']);
        res = Math.min(res, map['b' - 'a']);
        res = Math.min(res, map['l' - 'a'] / 2);
        res = Math.min(res, map['o' - 'a'] / 2);
        res = Math.min(res, map['n' - 'a']);
        return res;
    }
}
```

