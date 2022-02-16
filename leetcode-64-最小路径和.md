---
title: leetcode-64-最小路径和
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: aec4f41d
date: 2021-09-10 16:46:05
---

[**link**](https://leetcode-cn.com/problems/minimum-path-sum/submissions/)

<hr/>

tags : 二维 $dp$

**题目描述:**

给定一个包含非负整数的 $m\times n$ 网格 $grid$ ，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

**说明：**每次只能向下或者向右移动一步。

**示例 1：**

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210910164749.jpeg)

```
输入：grid = [[1,3,1],[1,5,1],[4,2,1]]
输出：7
解释：因为路径 1→3→1→1→1 的总和最小。
```

思路:

$dp[i][j] \Rightarrow$从$gird[0][0]$到$grid[i][j]$的最小路径和$

$base case$为第一行和第一列的数值初始化

状态转移方程: $dp[i][j]\Rightarrow min(dp[i-1][j],dp[i][j-1])+grid[i][j]$

$Code$如下:

```java
class Solution {
    public int minPathSum(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        if (n == 0) {
            return 0;
        }
        int[][] dp = new int[m][n];
        dp[0][0] = grid[0][0];
        for (int i = 1; i < m; i++) {
            dp[i][0] += (grid[i][0] + dp[i - 1][0]);
        }
        for (int i = 1; i < n; i++) {
            dp[0][i] += (grid[0][i] + dp[0][i - 1]);
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                dp[i][j] = Math.min(dp[i - 1][j], dp[i][j - 1]) + grid[i][j];
            }
        }
        return dp[m - 1][n - 1];
    }
}
```

