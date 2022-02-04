---
title: leetcode-1219-黄金矿工
date: 2022-02-05 00:25:03
categories: LeetCode
tags: [LeetCode,algorithm]
---

[1219. 黄金矿工](https://leetcode-cn.com/problems/path-with-maximum-gold/)

![image-20220205002540112](https://gitee.com/cao_ziqiang/img/raw/master/20220205002540.png)

<hr/>

比较容易看出这是回溯的题目,并且这里只需要维护一条可以获得最多黄金的路线即可。

选择这条路线时标记访问且加上当前的黄金，回溯完需恢复现场。

```java
class Solution {
    private static final int[] dx = new int[]{-1, 0, 1, 0};
    private static final int[] dy = new int[]{0, -1, 0, 1};
    int res = 0;
    public int getMaximumGold(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(grid[i][j] != 0) {
                    boolean[][] visted = new boolean[m][n];
                    backtrack(i, j, 0, visted, grid);
                }
            }
        }
        return res;
    }

    private void backtrack(int x, int y, int curGold, boolean[][] visted, int[][] grid) {
        // 尝试选择这条路线
        visted[x][y] = true;
        curGold += grid[x][y];
        res = Math.max(curGold, res);
        for(int i = 0; i < 4; i++) {
            int nx = x + dx[i];
            int ny = y + dy[i];
            // 越界直接退出即可
            if(nx < 0 || nx >= grid.length || ny < 0 || ny >= grid[0].length) {
                continue;
            }
            // 访问过或者是空地也需要退出
            if(visted[nx][ny] || grid[nx][ny] == 0) {
                continue;
            }
            backtrack(nx, ny, curGold, visted, grid);
        }
        // 恢复现场
        visted[x][y] = false;
        curGold -= grid[x][y];
    }
}
```

