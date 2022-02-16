---
title: leetcode-1765-地图中的最高点
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: 7c92064f
date: 2022-01-29 20:31:37
---

[1765. 地图中的最高点](https://leetcode-cn.com/problems/map-of-highest-peak/)

![image-20220129203242992](https://gitee.com/cao_ziqiang/img/raw/master/20220129203243.png)

我们可以看出只有当有水域时,陆地才会有高度，所以可以考虑从所有的水域开始多源bfs。

先遍历一趟$isWater$矩阵，将所有的水域添加进队列中，然后将所有的水域出队，更新相邻格子的高度，并将其重新入队以便更新剩余的格子。

```java
class Solution {
    private static final int[] dx = new int[]{-1, 0, 1, 0};
    private static final int[] dy = new int[]{0, -1, 0, 1};
    private boolean[][] visted = new boolean[1005][1005];
    public int[][] highestPeak(int[][] isWater) {
        int m = isWater.length, n = isWater[0].length;
        int[][] height = new int[m][n];
        Queue<int[]> q = new ArrayDeque<>();
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(isWater[i][j] == 1) {
                    q.offer(new int[]{i, j});
                    visted[i][j] = true;
                    height[i][j] = 0;
                }
            }
        }
        while(!q.isEmpty()) {
            int[] point = q.poll();
            for(int i = 0; i < 4; i++) {
                int nx = point[0] + dx[i];
                int ny = point[1] + dy[i];
                if(nx < 0 || nx >= m || ny < 0 || ny >= n || isWater[nx][ny] == 1 || visted[nx][ny]) {
                    continue;
                }
                height[nx][ny] = Math.max(height[point[0]][point[1]] + 1, height[nx][ny]);
                visted[nx][ny] = true;
                q.offer(new int[]{nx, ny});
            }
        }
        return height;
    }
}
```

