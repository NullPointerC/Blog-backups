---
title: leetcode-688-骑士在棋盘上的概率
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: ef00582d
date: 2022-02-17 00:26:14
---

[688. 骑士在棋盘上的概率](https://leetcode-cn.com/problems/knight-probability-in-chessboard/)

![image-20220217002656095](https://gitee.com/cao_ziqiang/img/raw/master/20220217002656.png)

<hr/>

![image-20220217002719260](https://gitee.com/cao_ziqiang/img/raw/master/20220217002719.png)

很明显,直接枚举每一个位置,在以每一个位置为这一步的起点继续枚举的$dfs$在$n$等于25的情况下是会超时的。于是需要考虑到,每走到一个位置$(x,y)$,需要记录下由上一步走到这一步且还在棋盘内的概率。

使用$dp[x][y][k]$表示点$(x,y)$走$k$步还在棋盘内的概率。显然,我们需要自底向上的$dp$,当由上一步$(row,col)$走到$(x,y)$的概率为$dp[row][col][k-1]$且$(x,y)$还在棋盘内时，需要更新$dp[x][y][k]=\sum{dp[row][col][k-1]} \text{/} 8$。

这里$(row,col)$是上一步的点，因为上一步有8个选择，所以需要统计所有的可能情况再除上8才是$dp[x][y][k]$。

```java
class Solution {
    private static int[] dx = new int[]{-2, -2, -1, 1, 2, 2, 1, -1};
    private static int[] dy = new int[]{-1, 1, 2, 2, 1, -1, -2, -2};
    public double knightProbability(int n, int k, int row, int column) {
        if(k == 0) {
            return 1.0;
        }
        // (x,y)走k步走不出界的概率
        double[][][] dp = new double[n][n][k + 1];
        // 走0步自然走不出去
        for(int i = 0; i < n; i++) {
            for(int j = 0; j < n; j++) {
                dp[i][j][0] = 1.0;
            }
        }
        for(int i = 1; i <= k; i++) {
            for(int x = 0; x < n; x++) {
                for(int y = 0; y < n; y++) {
                    double tmp = 0;
                    for(int j = 0; j < 8; j++) {
                        int nx = x + dx[j];
                        int ny = y + dy[j];
                        if(nx < 0 || nx >= n || ny < 0 || ny >= n) {
                            continue;
                        } else {
                            tmp += dp[nx][ny][i - 1];
                        }
                    }
                    dp[x][y][i] += tmp / 8;
                }
            }
        }
        return dp[row][column][k];
    }
}
```

