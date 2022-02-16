---
title: leetcode-221-最大正方形
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: 78797a86
date: 2021-09-13 21:26:20
---

[$\huge {link}$](https://leetcode-cn.com/problems/maximal-square/)

<hr/>

![image-20210913212939445](https://gitee.com/cao_ziqiang/img/raw/master/20210913212939.png)

<hr/>

记$f[i][j]$为第$i$行第$j$列为右下角所能构成的最大正方形边长

则有:

$f[i][j] = 1 + min(f[i-1][j],f[i][j-1],f[i-1][j-1])$

$\huge {Code}$

```java
class Solution {
    public int maximalSquare(char[][] matrix) {
        int maxSide = 0;
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return maxSide;
        }
        int rows = matrix.length, columns = matrix[0].length;
        int[][] dp = new int[rows][columns];
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < columns; j++) {
                if (matrix[i][j] == '1') {
                    if (i == 0 || j == 0) {
                        dp[i][j] = 1;
                    } else {
                        dp[i][j] = Math.min(Math.min(dp[i - 1][j], dp[i][j - 1]), dp[i - 1][j - 1]) + 1;
                    }
                    maxSide = Math.max(maxSide, dp[i][j]);
                }
            }
        }
        int maxSquare = maxSide * maxSide;
        return maxSquare;
    }
}
```

