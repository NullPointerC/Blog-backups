---
title: leetcode-1576-替换所有的问号
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: f0acaeba
date: 2022-01-05 14:51:53
---

[1576. 替换所有的问号](https://leetcode-cn.com/problems/replace-all-s-to-avoid-consecutive-repeating-characters/)

<hr/>

![image-20220105145357196](https://gitee.com/cao_ziqiang/img/raw/master/20220105145357.png)

题目意思比较简单，一趟遍历时，找到$?$字符的左边和右边，从$'a'-'z'$的字符中选择和左右字符都不等的即可。

为了防止处理特殊情况，这里可以先给$s$加上左右边界，这样就能统一处理边界情况了。

```java
class Solution {
    public String modifyString(String s) {
        if(s == null || s.isBlank()) {
            return null;
        }
        s = "a" + s + "z";
        char[] chs = s.toString().toCharArray();
        int n = chs.length;
        for(int i = 1; i <= n - 1; i++) {
            if(chs[i] != '?') {
                continue;
            }
            char l = chs[i - 1];
            char r = chs[i + 1];
            for(char j = 'a'; j <= 'c'; j++) {
                if(j != l && j != r) {
                    chs[i] = j;
                    break;
                }
            }
        }
        return new String(chs).substring(1, n - 1);
    }
}
```

