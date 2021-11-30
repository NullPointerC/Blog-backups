---
title: leetcode-63-不同路径Ⅱ
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-09-10 17:25:21
---

[link](https://leetcode-cn.com/problems/unique-paths-ii/)

<hr/>

tags: (二维$dp$)

**题目描述:**

一个机器人位于一个 $m \times n$ 网格的左上角 （起始点在下图中标记为$“Start”$ ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为$“Finish”$）。

现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210910172735.png)

网格中的障碍物和空位置分别用 $1$和 $0$ 来表示。

**示例 1：**

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210910172807.jpeg)

```
输入：obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
输出：2
解释：
3x3 网格的正中间有一个障碍物。
从左上角到右下角一共有 2 条不同的路径：
1. 向右 -> 向右 -> 向下 -> 向下
2. 向下 -> 向下 -> 向右 -> 向右
```

**示例 2：**

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210910172827.jpeg)

```
输入：obstacleGrid = [[0,1],[0,0]]
输出：1
```

**思路:**

我们用 $f(i, j)$ 来表示从坐标 $(0, 0)$ 到坐标 $(i,j)$ 的路径总数，$u(i,j)$ 表示坐标 $(i,j)$ 是否可行，如果坐标 $(i,j)$ 有障碍物，$u(i,j)=0$，否则 $u(i, j) = 1$。

因为「机器人每次只能向下或者向右移动一步」，所以从坐标 $(0,0)$ 到坐标 $(i,j)$ 的路径总数的值只取决于从坐标 $(0, 0)$ 到坐标 $(i−1,j)$ 的路径总数和从坐标 $(0,0)$ 到坐标 $(i,j−1)$ 的路径总数，即 $f(i,j)$ 只能通过 $f(i−1,j)$ 和 $f(i, j - 1)$ 转移得到。

当坐标 $(i,j)$ 本身有障碍的时候，任何路径都到到不了 $f(i,j)$，此时 $f(i, j) = 0$；

下面我们来讨论坐标 $(i, j)$ 没有障碍的情况：如果坐标 $(i−1,j)$ 没有障碍，那么就意味着从坐标 $(i−1,j)$ 可以走到 $(i,j)$，即 $(i−1,j)$ 位置对 $f(i,j)$ 的贡献为 $f(i−1,j)$。

同理，当坐标 $(i,j−1)$ 没有障碍的时候，$(i,j−1)$ 位置对 $f(i,j)$ 的贡献为 $f(i,j−1)$。综上所述，我们可以得到这样的动态规划转移方程：

$ f(i,j) = \begin{cases} 0, & \text{if  }u(i,j)\quad\ne \text {0} \\ f(i-1,j)+f(i,j-1), & \text{if }u(i,j) \quad \text= \quad0 \end{cases}$

$\Huge Code$如下：

```java
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;
        if (n == 0) {
            return 0;
        }
        // 终点不可达, 直接退出
        if (obstacleGrid[m - 1][n - 1] == 1) {
            return 0;
        }
        // 从(0,0)到(m,n)的路径数
        int[][] dp = new int[m][n];
        // 初始化
        for (int i = 0; i < m && obstacleGrid[i][0] != 1; i++) {
            dp[i][0] = 1;
        }

        for (int i = 0; i < n && obstacleGrid[0][i] != 1; i++) {
            dp[0][i] = 1;
        }

        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (obstacleGrid[i][j] == 1) {
                    continue;
                }
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }
        return dp[m - 1][n - 1];
    }
}
```

