---
title: leetcode-521-最长特殊序列Ⅰ
categories: LeetCode
tags: 'LeetCode,algorithm'
abbrlink: f43071e6
date: 2022-03-05 20:29:00
---

[521. 最长特殊序列 Ⅰ](https://leetcode-cn.com/problems/longest-uncommon-subsequence-i/)

![image-20220305202937715](https://gitee.com/cao_ziqiang/img/raw/master/20220305202937.png)

觉得虽然标的是简单，但是还是对于这种思维题，需要想清楚题目的条件才可以做的出来。

情况其实就分为相等和不相等两种情况，如果相等，则返回`-`，因为无论怎么拆分，都是另一个的子序列，而如果不等，可以直接取较长的作为去比较的子序列，这样就可以得到结果。

```java
class Solution {
    public int findLUSlength(String a, String b) {
        int n1 = a.length(), n2 = b.length();
        if(a.equals(b)) {
            return -1;
        }
        return Math.max(n1, n2);
    }
}
```

