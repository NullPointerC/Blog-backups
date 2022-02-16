---
title: leetcode-1446-连续字符
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: 9d08f171
date: 2021-12-01 16:01:30
---

[连续字符](https://leetcode-cn.com/problems/consecutive-characters/)

![image-20211201160228442](https://gitee.com/cao_ziqiang/img/raw/master/20211201160228.png)

<hr/>

tags(模拟、遍历)

每次找到相邻的相等字符，就计算这段字符的长度即可。

```java
class Solution {
    public int maxPower(String s) {
        int n = s.length();
        if(n == 0 || s.isBlank()) {
            return 0;
        }
        int ans = -1;
        char[] chars = s.toCharArray();
        int maxLen = 1;
        for(int i = 1; i < n; i++) {
            if(chars[i] == chars[i - 1]) {
                maxLen++;
                ans = Math.max(ans, maxLen);
            } else {
                maxLen = 1;
            }
        }
        return ans == -1 ? maxLen : ans;
    }
}
```

Golang:

```go
func maxPower(s string) int {
    n, ans := len(s), 0
    if n == 0 {
        return ans
    }
    maxLen := 1
    for i := 1; i < n; i++ {
        if s[i] == s[i - 1] {
            maxLen++
            ans = max(ans, maxLen)
        } else {
            maxLen = 1
        }
    }
    if ans == 0 {
        return maxLen
    } else {
        return ans
    }
}
func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

