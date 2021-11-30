---
title: leetcode-994-腐烂的橘子
date: 2021-11-08 21:36:09
categories: LeetCode
tags: [LeetCode,algorithm]
---

[$link$](https://leetcode-cn.com/problems/rotting-oranges/)

<hr/>

![image-20211108213658950](https://gitee.com/cao_ziqiang/img/raw/master/20211108213659.png)

<hr/>

这题比较容易想到用$bfs$，但是千万要注意处理好边界值，不然就wa了。

初始化时，先将所有腐烂的橘子入队，然后再去感染周围好的橘子，同时每次增加步骤时，要判断队列是否为空，同时将好的橘子数相应减少。

```java
class Solution {
    public int orangesRotting(int[][] grid) {
        // 个人感觉这种方式比较优雅,即省去了开二维的数组，也省去了开2个一维的数组
        int[] dirs = new int[]{-1, 0, 1, 0, -1};
        // 总的操作数
        int cnt = 0;
        // 好的橘子数
        int cntOfNew = 0;
        int m = grid.length, n = grid[0].length;
        Queue<int[]> queue = new ArrayDeque<>();
        for(int i = 0; i < m; ++i) {
            for(int j = 0; j < n; ++j) {
                if (grid[i][j] == 2) {
                    // 所有腐烂的橘子入队
                    queue.offer(new int[]{i, j});
                } else if (grid[i][j] == 1) {
                    // 好的橘子计数,避免后期重复计算
                    ++cntOfNew;
                }
            }
        }
        while(!queue.isEmpty() && cntOfNew > 0) {
            // 所有坏的橘子出队
            int size = queue.size();
            while(size > 0) {
                int[] point = queue.poll();
                // 遍历上下左右
                for(int i = 1; i <= 4; ++i) {
                    int nx = point[0] + dirs[i - 1];
                    int ny = point[1] + dirs[i];
                    if (nx >= 0 && nx < m && ny >= 0 && ny < n && grid[nx][ny] == 1) {
                        // 好的橘子少了
                        --cntOfNew;
                        // 坏的橘子多了
                        grid[nx][ny] = 2;
                        queue.offer(new int[] {nx, ny});
                    }
                }
                --size;
            }
            if (!queue.isEmpty()) {
                ++cnt;
            }
        }
        return cntOfNew == 0 ? cnt : -1;
    }
}
```

