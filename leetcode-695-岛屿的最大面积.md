---
title: leetcode-695-岛屿的最大面积
date: 2021-11-06 13:28:44
categories: LeetCode
tags: [LeetCode,algorithm]
---

[$link$](https://leetcode-cn.com/problems/max-area-of-island/)

<hr/>

![image-20211106132926272](https://gitee.com/cao_ziqiang/img/raw/master/20211106132926.png)

![img](https://gitee.com/cao_ziqiang/img/raw/master/20211106132934.jpeg)

![image-20211106132944567](https://gitee.com/cao_ziqiang/img/raw/master/20211106132944.png)

<hr/>

经过上一题，这题马上就想到了用深搜，如果遇到1，就对这点进行深搜，深搜过程中记得将遍历到的点设置为0，防止重复计算。

```java
class Solution {
    int[][] grid;
    int ans = 0;
    int curr = 0;
    int m, n;
    public int maxAreaOfIsland(int[][] grid) {
        this.grid = grid;
        this.m = grid.length;
        this.n = grid[0].length;
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if (grid[i][j] == 1) {
                    curr = 0;
                    dfs(i, j);
                }
            }
        }
        return ans;
    }
    private void dfs(int r, int c) {
        if (r < 0 || r >= m || c < 0 || c >= n || grid[r][c] != 1) {
            return ;
        }
        // System.out.println(r + "\t" + c);
        grid[r][c] = 0;
        curr++;
        // System.out.println(curr);
        ans = Math.max(ans, curr);
        dfs(r - 1, c);
        dfs(r + 1, c);
        dfs(r, c - 1);
        dfs(r, c + 1);
    }
}
```

再补一个bfs

```java
class Solution {
    static int[] dx = new int[]{-1, 0, 1, 0};
    static int[] dy = new int[]{0, 1, 0, -1};
    int m, n;
    public int maxAreaOfIsland(int[][] grid) {
        int ans = 0, curr = 0;
        this.m = grid.length;
        this.n = grid[0].length;
        Deque<int[]> queue = new ArrayDeque<>();
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(grid[i][j] == 1) {
                    queue.offer(new int[]{i, j});
                    grid[i][j] = 0;
                    curr++;
                    while(!queue.isEmpty()) {
                        int[] point = queue.poll();
                        for(int k = 0; k < 4; k++) {
                            int x = point[0] + dx[k];
                            int y = point[1] + dy[k];
                            if(isValidPoint(x, y) && grid[x][y] == 1) {
                                queue.offer(new int[]{x, y});
                                grid[x][y] = 0;
                                curr++;
                            }
                        }
                    }
                    ans = Math.max(ans, curr);
                    curr = 0;
                }
            }
        }
        return ans;
    }
    private boolean isValidPoint(int x, int y) {
        return (x >= 0 && x < m && y >= 0 && y < n);
    }
}
```

