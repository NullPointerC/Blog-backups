---
title: leetcode-5-最长回文子串
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
hidden: true
abbrlink: 545f106b
date: 2021-08-23 16:56:55
---

[link](https://leetcode-cn.com/problems/longest-palindromic-substring/)

<hr/>

(tags:dp,字符串)

题目描述:

<pre>
给你一个字符串 s，找到 s 中最长的回文子串。
回文的意思是正着念和倒着念一样，如：上海自来水来自海上
</pre>

示例1:

<pre>
输入：s = "babad"
输出："bab"
解释："aba" 同样是符合题意的答案。
</pre>

示例2:

<pre>
输入：s = "cbbd"
输出："bb"
</pre>

示例3:

<pre>
输入：s = "a"
输出："a"
</pre>

示例4:

<pre>
输入：s = "ac"
输出："a"
</pre>

思路:

<pre>
本题我认为题目原意是让我们使用dp解决.所以构造一个二维布尔dp数组
dp[i][j]	=>	下标从i到j的子串是否回文
然后再根据子串是否回文来进行下一步的状态更新
在遍历字符串的过程中,如果是一个字符,那么必定是回文串;
如果是两个字符,就要看这两个字符是否相等;
如果是三个及以上,就要看i位置和j位置的字符是否相等以及去掉首尾的子串是否是回文串
</pre>

代码:

```java
class Solution {
    public String longestPalindrome(String s) {
        int len = s.length();
        if (len == 0 || len == 1) {
            return s;
        }

        // dp[i][j]表示下标从i到j的子串是否回文
        boolean[][] dp = new boolean[len][len];
        char[] str = s.toCharArray();
        int max = 1;
        String res = "";
        for (int i = len - 1; i >= 0; i--) {
            // 反向遍历,因为下文使用到了dp[i+1][j-1]
            for (int j = i; j < len; j++) {
                if (i == j) {
                    // 一个字符必定回文
                    dp[i][j] = true;
                } else if (j == i + 1) {
                    // 两个字符只要判断它们是否相等
                    dp[i][j] = (str[i] == str[j]);
                } else {
                    // 三个及以上看s[i]和s[j]是否相等且去掉头尾的子串是否回文
                    dp[i][j] = (str[i] == str[j]) && dp[i + 1][j - 1];
                }

                if (dp[i][j]) {
                    if (j + 1 - i >= max) {
                        // 更新最长回文子串
                        max = j + 1 - i;
                        res = s.substring(i, j + 1);
                    }
                }
            }
        }
        return res;
    }
}
```

![image-20210823170424460](https://gitee.com/cao_ziqiang/img/raw/master/20210823170424.png)

虽然花了一个小时才把题目做出来,但是比去年只会用暴力遍历却没有遍历出来已经好太多了,继续加油!

<hr/>

在评论区看到了大佬不用dp,利用回文串的特点来解题,思路大致如下:

<pre>
如"aba"是一个回文子串在"babad"中,那么我们将回文串看出一串全是同一字符的字符串,左右部分对称
然后再从中间向两边扩散,记录最大长度
</pre>

代码:

```java
public String longestPalindrome2(String s) {
    if (s == null || s.length() == 0) {
        return "";
    }
    // 保存起始位置，测试了用数组似乎能比全局变量稍快一点
    int[] range = new int[2];
    char[] str = s.toCharArray();
    for (int i = 0; i < s.length(); i++) {
        // 把回文看成中间的部分全是同一字符，左右部分相对称
        // 找到下一个与当前字符不同的字符
        i = findLongest(str, i, range);
    }
    return s.substring(range[0], range[1] + 1);
}

private int findLongest(char[] str, int low, int[] range) {
    // 查找中间部分
    int high = low;
    while (high < str.length - 1 && str[high + 1] == str[low]) {
        high++;
    }
    // 定位中间部分的最后一个字符
    int ans = high;
    // 从中间向左右扩散
    while (low > 0 && high < str.length - 1 && str[low - 1] == str[high + 1]) {
        low--;
        high++;
    }
    // 记录最大长度
    if (high - low > range[1] - range[0]) {
        range[0] = low;
        range[1] = high;
    }
    return ans;
}
```

![image-20210823171331030](https://gitee.com/cao_ziqiang/img/raw/master/20210823171331.png)

从性能来看,比使用dp解法更优,从这也学到了一个回文串的字符串解法就是:从中心点往两边扩散，来寻找回文串，这种方向相当于穷举每一个点为中心点

