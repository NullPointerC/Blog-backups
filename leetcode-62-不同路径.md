---
title: leetcode-62-不同路径
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: 3d0dbff0
date: 2021-08-28 10:08:43
---

[link](https://leetcode-cn.com/problems/unique-paths/)

<hr/>

题目描述:

<pre>
一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为 “Start” ）。
机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish” ）。
问总共有多少条不同的路径？
</pre>

示例1:

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210828102659.png)

<pre>
输入：m = 3, n = 7
输出：28
</pre>

示例 2：

<pre>
输入：m = 3, n = 2
输出：3
解释：
从左上角开始，总共有 3 条路径可以到达右下角。
1. 向右 -> 向下 -> 向下
2. 向下 -> 向下 -> 向右
3. 向下 -> 向右 -> 向下 
</pre>

示例 3：

<pre>
输入：m = 7, n = 3
输出：28 
</pre>

示例 4：

<pre>
输入：m = 3, n = 3
输出：6
</pre>

思路:

<pre>
求到点(i,j)的路径数需要用到这一点上,左位置的路径数;
设dp[i][j]为到点(i,j)的路径数量
dp[i][j] = dp[i-1][j] + dp[i][j-1]
</pre>

代码:

```java
class Solution {
    public int uniquePaths(int m, int n) {
        // 表示从(0,0)到(m-1,n-1)的路径数量
        int[][] dp = new int[m][n];
        // 初始化base case
        for (int i = 0; i < m; i++) {
            dp[i][0] = 1;
        }
        for (int i = 0; i < n; i++) {
            dp[0][i] = 1;
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }
        return dp[m - 1][n - 1];
    }
}
```

![image-20210828103056896](https://gitee.com/cao_ziqiang/img/raw/master/20210828103057.png)

<pre>
后来看到这也是一个经典的数论题目
C(m+n-2，min(m-1,n-1));
</pre>

