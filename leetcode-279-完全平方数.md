---
title: leetcode-279-完全平方数
date: 2021-10-29 16:28:16
categories: [LeetCode]
tags: [LeetCode,algorithm]
---

[$link$](https://leetcode-cn.com/problems/perfect-squares/)

<hr/>

![image-20211029162907969](https://gitee.com/cao_ziqiang/img/raw/master/20211029162908.png)

<hr/>

又是一个比较典型的背包问题，但是这里没有提前告诉我们背包容量，于是可以先使用静态块把背包构造出来，题目已经说明了$n$的范围在$1\text~1e4$，所以构造这之中的完全平方数组成的背包。

```java
class Solution {
    private static List<Integer> squares = new ArrayList<>();
    static {
        for(int i = 1; i <= 100; i++) {
            squares.add(i * i);
        }
    }
    public int numSquares(int n) {
        // 整数n需要dp[n]个平方数相加
        int[] dp = new int[n + 1];
        Arrays.fill(dp, Integer.MAX_VALUE);
        dp[0] = 0;
        for(int square : squares) {
            for(int i = 1; i <= n; i++) {
                if(i >= square) dp[i] = Math.min(dp[i], dp[i - square] + 1);
            }
        }
        return dp[n];
    }
}
```

但是好像直接设置一层内循环还可以更快，因为背包的容量比较小，遍历一趟也不会花很多时间。

```java
class Solution {
    public int numSquares(int n) {
        // 整数n需要dp[n]个平方数相加
        int[] dp = new int[n + 1];
        Arrays.fill(dp, Integer.MAX_VALUE);
        dp[0] = 0;
        dp[1] = 1;
        for(int i = 0; i <= n; i++) {
            for(int j = 1; j * j <= i; j++) {
                dp[i] = Math.min(dp[i - j * j] + 1, dp[i]);
            }
        }
        return dp[n];
    }
}
```

