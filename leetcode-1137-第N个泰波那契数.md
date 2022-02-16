---
title: leetcode-1137-第N个泰波那契数
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: 47d6597d
date: 2021-09-01 10:08:48
---

[link](https://leetcode-cn.com/problems/n-th-tribonacci-number/)

<hr/>

**题目描述:**

<pre>
泰波那契序列 Tn 定义如下： 
T0 = 0, T1 = 1, T2 = 1, 且在 n >= 0 的条件下 Tn+3 = Tn + Tn+1 + Tn+2
给你整数 n，请返回第 n 个泰波那契数 Tn 的值。
</pre>

**示例1:**

<pre>
输入：n = 4
输出：4
解释：
T_3 = 0 + 1 + 1 = 2
T_4 = 1 + 1 + 2 = 4
</pre>

**示例2:**

<pre>
输入：n = 25
输出：1389537
</pre>

**思路:**

<pre>
利用公式,记忆化即可,或者使用尾递归,直接使用递归会超时
</pre>

**代码:**

```java
class Solution {
    public int tribonacci(int n) {
        if (n <= 0) {
            return 0;
        }
        if (n == 1 || n == 2) {
            return 1;
        }
        int[] dp = new int[n + 1];
        dp[0] = 0;
        dp[1] = 1;
        dp[2] = 1;
        for (int i = 3; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2] + dp[i - 3];
        }
        return dp[n];
    }
}
```

<pre>
其实这题用空间换时间也未尝不可,因为这些数字都是经常要被用到的,没有必要一一枚举
</pre>

```java
class Solution {
    static int[] Ts = {0, 1, 1, 2, 4, 7, 13, 24, 44, 81, 149, 274, 504, 927, 1705, 3136, 5768, 10609, 19513, 35890, 66012, 121415, 223317, 410744, 755476, 1389537, 2555757, 4700770, 8646064, 15902591, 29249425, 53798080, 98950096, 181997601, 334745777, 615693474, 1132436852, 2082876103};
    public int tribonacci(int n) {
        return Ts[n];
    }
}
```

<pre>
矩阵快速幂确实是我没有想到的解法,没想到线性代数的运用可以到这里...
</pre>

贴一个链接吧:[矩阵快速幂](https://leetcode-cn.com/problems/n-th-tribonacci-number/solution/gong-shui-san-xie-yi-ti-si-jie-die-dai-d-m1ie/)

![image-20210901103817291](https://gitee.com/cao_ziqiang/img/raw/master/20210901103817.png)

