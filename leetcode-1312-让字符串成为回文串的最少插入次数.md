---
title: leetcode-1312-让字符串成为回文串的最少插入次数
date: 2021-10-17 19:30:48
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
---

[$link$](https://leetcode-cn.com/problems/minimum-insertion-steps-to-make-a-string-palindrome/)

<hr/>

![image-20211017193142137](https://gitee.com/cao_ziqiang/img/raw/master/20211017193142.png)

<hr/>

假设$dp[i][j]$为字符串$s$的字串$s[i...j]$变为回文串的最少操作次数；

故当$s[i]==s[j]$时，此时不需要操作，只需要先知道去掉首尾两端的更短的字串的操作次数即可，故$dp[i][j]=dp[i+1][j-1]$；

若$s[i] \neq s[j]$时，此时有三种可能，第一种是在两端各补齐一个另外一端字母，即$dp[i+1][j-1] + 2$，但是也需要考虑这样的情况，即$dp[i+1][j]$也就是$s[i+1...j]$这一段已经是回文串，所以$dp[i][j]=dp[i+1][j]+1$。同理也需要考虑$dp[i][j-1]$这一段。

```java
class Solution {
    public int minInsertions(String s) {
        final int n = s.length();
        if(n < 2) {
            return 0;
        }
        // 使s[i...j]变为回文串的最少插入次数
        int[][] dp = new int[n][n];
        for(int i = n - 1; i >= 0; i--) {
            for(int j = i + 1; j < n; j++) {
                if(s.charAt(i) == s.charAt(j)) {
                    dp[i][j] = dp[i + 1][j -1];
                }else{
                    dp[i][j] = Math.min(Math.min(dp[i + 1][j] + 1, dp[i][j - 1] + 1), dp[i + 1][j - 1] + 2);
                }
            }
        }
        return dp[0][n - 1];
    }
}
```

<hr/>

同样，也可以这样考虑问题，要将字符串变为回文串，可以考虑将字符串反转，找出原字符串和反转后字符串两者的最长公共子序列，把不相同的那些字母，拿过来插到相同字母的前面或者后面就可以了。

```java
class Solution {
    public int minInsertions(String s) {
        String rs = new StringBuilder(s).reverse().toString();
        int common = longestCommonSubsequence(s, rs);
        return s.length() - common;
    }
    
    private int longestCommonSubsequence(String str1, String str2) {
        final int m = str1.length(), n = str2.length();
        // str1[0...m-1]和str2[0...n-1]的公共序列长度
        int[][] dp = new int[m + 1][n + 1];
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (str1.charAt(i - 1) == str2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                }else{
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[m][n];
    }
}
```

