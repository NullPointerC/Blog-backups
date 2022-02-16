---
title: leetcode-1020-飞地的数量
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: c77f73c1
date: 2022-02-12 16:37:49
---

[1020. 飞地的数量](https://leetcode-cn.com/problems/number-of-enclaves/ "1020. 飞地的数量")

![image-20220212163842912](https://gitee.com/cao_ziqiang/img/raw/master/20220212163842.png)

一个比较简单的办法就是从边界的1开始，将可以到达的1都标记为0，再遍历一趟$grid$的内部，如果有1，则说明这些1是无法到达的，将这些1统计即可。

```java
class Solution {
    private static final int[] dx = new int[]{-1, 0, 1, 0};
    private static final int[] dy = new int[]{0, -1, 0, 1};
    public int numEnclaves(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        for(int i = 0; i < n; i++) {
            if(grid[0][i] == 1) {
                dfs(0, i, m, n, new boolean[m][n], grid);
            }
            if(grid[m - 1][i] == 1) {
                dfs(m - 1, i, m, n, new boolean[m][n], grid);
            }
        }
        for(int i = 0; i < m; i++) {
            if(grid[i][0] == 1) {
                dfs(i, 0, m, n, new boolean[m][n], grid);
            }
            if(grid[i][n - 1] == 1) {
                dfs(i, n - 1, m, n, new boolean[m][n], grid);
            }
        }
        int cnt = 0;
        for(int i = 1; i < m - 1; i++) {
            for(int j = 1; j < n - 1; j++) {
                if(grid[i][j] == 1) cnt++;
            }
        }
        return cnt;
    }

    private void dfs(int x, int y, int m, int n, boolean[][] visted, int[][] grid) {
        visted[x][y] = true;
        grid[x][y] = 0;
        for(int i = 0; i < 4; i++) {
            int nx = x + dx[i], ny = y + dy[i];
            if(nx < 0 || nx >= m || ny < 0 || ny >= n || visted[nx][ny] || grid[nx][ny] == 0) {
                continue;
            }
            dfs(nx, ny, m, n, visted, grid);
        }
    }
}
```

或者是标记能够达到的数量can，再统计所有的1的数量cnt，cnt-can即为飞地数量。

```java
class Solution {
    private static final int[] dx = new int[]{-1, 0, 1, 0};
    private static final int[] dy = new int[]{0, -1, 0, 1};
    public int numEnclaves(int[][] grid) {
        int m = grid.length, n = grid[0].length, cnt = 0, can = 0;
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(grid[i][j] == 1) {
                    cnt++;
                    if(canGoOut(i, j, m, n, new boolean[m][n], grid)) can++;
                }
            }
        }
        return cnt - can;
    }

    private boolean canGoOut(int x, int y, int m, int n, boolean[][] visted, int[][] grid) {
        if(x == 0 || x == m - 1 || y == 0 || y == n - 1) {
            return true;
        }
        visted[x][y] = true;
        boolean flag = false;
        for(int i = 0; i < 4; i++) {
            int nx = x + dx[i], ny = y + dy[i];
            if(nx < 0 || nx >= m || ny < 0 || ny >= n || visted[nx][ny] || grid[nx][ny] == 0) {
                continue;
            } 
            flag |= canGoOut(nx, ny, m, n, visted, grid);
        }
        return flag;
    }
}
```

