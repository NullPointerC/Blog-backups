---
title: leetcode-1332-删除回文子序列
date: 2022-01-22 11:30:41
categories: LeetCode
tags: [LeetCode,algorithm]
---

[1332. 删除回文子序列](https://leetcode-cn.com/problems/remove-palindromic-subsequences/)

<hr/>

![image-20220122113125090](https://gitee.com/cao_ziqiang/img/raw/master/20220122113125.png)

纯纯的牛马题目,dp了半天,结果是思维题。

要注意到以下条件：

①.子序列，子序列就不要求连续，如果要删除，我们可以贪心的考虑一次性把一种字母全部删掉；

②.只有2种字母，意为只能构成2类回文串；

所以对于给定的s,如果s本身为空，那么无需删除了，如果s本身就是回文串，那么可以一次到位，如果s本身并不是回文串，可以分两次删除，第一次把把`a`全部删掉，再把`b`全部删掉，共计2步。

```java
class Solution {
    public int removePalindromeSub(String s) {
        int n = s.length();
        if(n == 0) return 0;
        else if(check(s)) return 1;
        else return 2;
    }
    private boolean check(String str) {
        int s = 0, e = str.length() - 1;
        while(s < e) {
            if(str.charAt(s) != str.charAt(e)) {
                return false;
            }
            s++;
            e--;
        }
        return true;
    }
}
```

