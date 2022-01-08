---
title: leetcode-1023-驼峰式匹配
date: 2022-01-05 20:29:40
categories: LeetCode
tags: [LeetCode,algorithm]
---

[1023. 驼峰式匹配](https://leetcode-cn.com/problems/camelcase-matching/)

<hr/>

![image-20220105203017564](https://gitee.com/cao_ziqiang/img/raw/master/20220105203017.png)

tags: 双指针

只需判断对于每个$query$，是否可以用$pattern$匹配它即可，每匹配到$pattern$中的一个大写字母，就使得匹配$pattern$的指针往后移动一个单位，若有大写字母且未与$pattern$匹配时，直接返回$false$。最后判断是否完全匹配完了$pattern$中的所有字母，即$j==pattern.length()$。

```java
class Solution {
    public List<Boolean> camelMatch(String[] queries, String pattern) {
        List<Boolean> ans = new ArrayList<>();
        for(var query : queries) {
            ans.add(check(query, pattern));
        }
        return ans;
    }
    private boolean check(String query, String pattern) {
        int j = 0;
        for(int i = 0; i < query.length(); i++) {
            if(j < pattern.length() && query.charAt(i) == pattern.charAt(j)) j++;
            else if(query.charAt(i) < 'a') return false;
        }
        return j == pattern.length();
    }
}
```

