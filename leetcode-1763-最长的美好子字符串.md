---
title: leetcode-1763-最长的美好子字符串
date: 2022-02-01 00:30:37
categories: LeetCode
tags: [LeetCode,algorithm]
---

[1763. 最长的美好子字符串](https://leetcode-cn.com/problems/longest-nice-substring/)

![image-20220201003115958](https://gitee.com/cao_ziqiang/img/raw/master/20220201003116.png)

本来是想用滑动窗口的，但是不好找收缩窗口的条件，所以直接暴力枚举所有字串了。😂

```java
class Solution {
    public String longestNiceSubstring(String s) {
        int n = s.length();
        if(n == 0) {
            return s;
        }
        // int l = 0, r = 0, ans = 0;
        // Map<Character, Integer> map = new HashMap<>();
        String res = "";
        for(int i = 0; i < n; i++) {
            for(int j = i + 1; j <= n; j ++) {
                String str = s.substring(i, j);
                if(check(str) && str.length() > res.length()) {
                    res = str;
                }
            }
        }
        return res;
    }
    private boolean check(String str) {
        boolean[] smallMap = new boolean[26];
        boolean[] bigMap = new boolean[26];
        int n = str.length();
        for(int i = 0; i < n; i++) {
            char c = str.charAt(i);
            if(c >= 'a' && c <= 'z') smallMap[c - 'a'] = true;
            else bigMap[c - 'A'] = true;
        }
        return Arrays.equals(smallMap, bigMap);
    }
}
```

