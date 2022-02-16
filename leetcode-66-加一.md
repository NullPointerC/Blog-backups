---
title: leetcode-66-加一
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: 9209352a
date: 2021-10-21 16:17:14
---

[$link$](https://leetcode-cn.com/problems/plus-one/)

<hr/>

![image-20211021161932429](https://gitee.com/cao_ziqiang/img/raw/master/20211021161932.png)

<hr/>

个人感觉比较好想到的办法,如果已知$dights[starnt,end]$这一段加一后产生了进位,那么就可以对$dights[start,end-1]$这段再进行加一,直到不产生进位,则可以退出递归。

此外，还需要考虑，若全为9的这种情况，因为会导致全0，需要补齐进位1。

```java
class Solution {
    public int[] plusOne(int[] digits) {
        dfs(digits, 0 , digits.length - 1);
        // 排除全为9的情况
        if(digits[0] == 0) {
            digits = new int[digits.length + 1];
            digits[0] = 1;
        }
        return digits;
    }

    private void dfs(int[] digits, int start ,int end) {
        if(start > end) {
            return;
        }
        if(digits[end] != 9) {
            digits[end]++;
            return;
        }
        digits[end] = 0;
        dfs(digits, start, end - 1);
    }
}
```

<hr/>

也可以直接进行模拟即可。

```java
class Solution {
    public int[] plusOne(int[] digits) {
        int plus = 1;
        for(int i = digits.length - 1; i > -1; i--) {
            digits[i] += plus;
            plus = digits[i] / 10;
            digits[i] %= 10;
        }
        // 补齐进位
        if(plus == 1) {
            digits = new int[digits.length + 1];
            digits[0] = 1;
        }
        return digits;
    }
}
```

