---
title: leetcode-1034-边界着色
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: 23dc668
date: 2021-12-07 17:45:10
---

[边界着色](https://leetcode-cn.com/problems/coloring-a-border/)

![image-20211207174558431](https://gitee.com/cao_ziqiang/img/raw/master/20211207174558.png)

<hr/>

tags: (dfs、bfs)

说起来这题还是很坑的,中文的题目可能是直译过来的,不是很好理解。

通过看懂示例数据，可以知道题目提到的边界，有两种边界，一种在连通分量的点且这个点在这个矩阵$grid$的边界，另一种边界就是连通分量的边界。

要找到这个联通分量可以通过$dfs$或者$bfs$都可以，主要的难点在于如何判断是否为连通分量的边界。

这里如果使用$dfs$来遍历，可以使用一个二维的$connected$数组来标识该点是否在联通分量内。然后去深搜这块联通分量，将所有在联通分量内的点都标识为$true$，且将颜色改为$color$。

搜索完成后，再遍历一次矩阵，将完全处于联通分量内的点再改回它原来的颜色即可。判断是否完全处于联通分量内部只需满足$connected[i][j]$并且它的其余四个方向也是在联通分量内即可。

```java
class Solution {
    private static final int[] dx = new int[]{-1, 0, 1, 0};
    private static final int[] dy = new int[]{0, -1, 0, 1};
    private int m, n;
    public int[][] colorBorder(int[][] grid, int row, int col, int color) {
        this.m = grid.length;
        this.n = grid[0].length;
        int original = grid[row][col];
        boolean[][] connected = new boolean[m][n];
        dfs(grid, row, col, original, color, connected);
        for(int i = 1; i < m - 1; i++) {
            for(int j = 1; j < n - 1; j++) {
                if(connected[i][j] && connected[i - 1][j] && connected[i + 1][j] && connected[i][j - 1] && connected[i][j + 1]) {
                    grid[i][j] = original;
                }
            }
        }
        return grid;
    }
    private void dfs(int[][] grid, int x, int y, int original, int color, boolean[][] connected) {
        if (x < 0 || x >= m || y < 0 || y >= n || grid[x][y] != original || connected[x][y]) {
            return ;
        }
        grid[x][y] = color;
        connected[x][y] = true;
        for(int i = 0; i < 4; i++) {
            dfs(grid, x + dx[i], y + dy[i], original, color, connected);
        }
    }
}
```

