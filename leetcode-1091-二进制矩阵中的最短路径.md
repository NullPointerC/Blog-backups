---
title: leetcode-1091-二进制矩阵中的最短路径
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: 92bfad95
date: 2021-11-21 19:54:17
---

[二进制矩阵中的最短路径](https://leetcode-cn.com/problems/shortest-path-in-binary-matrix/)

<hr/>

![image-20211122095608446](https://gitee.com/cao_ziqiang/img/raw/master/20211122095608.png)

![image-20211122095619194](https://gitee.com/cao_ziqiang/img/raw/master/20211122095619.png)

<hr/>

这里有个坑要注意的就是，从起点$(0,0)$开始，已经算走了一个格子。

这里不能用$dfs$，会$tle$。所以可以用$bfs$。

关于什么时候用$dfs$或$bfs$，看到一篇博客讲的很好，这里粘贴一下：

1. 如果只是要找到某一个结果是否存在，那么DFS会更高效。因为DFS会首先把一种可能的情况尝试到底，才会回溯去尝试下一种情况，只要找到一种情况，就可以返回了。但是BFS必须所有可能的情况同时尝试，在找到一种满足条件的结果的同时，也尝试了很多不必要的路径；

2. 如果是要找所有可能结果中最短的，那么BFS会更高效。因为DFS是一种一种的尝试，在把所有可能情况尝试完之前，无法确定哪个是最短，所以DFS必须把所有情况都找一遍，才能确定最终答案（DFS的优化就是剪枝，不剪枝很容易超时）。而BFS从一开始就是尝试所有情况，所以只要找到第一个达到的那个点，那就是最短的路径，可以直接返回了，其他情况都可以省略了，所以这种情况下，BFS更高效。





```java
class Solution {
    public int shortestPathBinaryMatrix(int[][] grid) {
        int ans = 0;
        int n = grid.length;
        boolean[][] visted = new boolean[n][n];
        Queue<int[]> queue = new ArrayDeque<>();
        int[][] dirs = new int[][]{{0,1}, {0,-1}, {1,0}, {-1,0}, {1,1}, {1,-1},{-1,1}, {-1,-1}};
        if(grid[0][0] == 0) {
            queue.offer(new int[]{0, 0});
        }
        visted[0][0] = true;
        while(!queue.isEmpty()) {
            int size = queue.size();
            for(int i = 0; i < size; i++) {
                int[] point = queue.poll();
                if (point[0] == n - 1 && point[1] == n - 1) {
                    return ans + 1;
                }
                for(int j = 0; j < 8; j++) {
                    int x = point[0] + dirs[j][0];
                    int y = point[1] + dirs[j][1];
                    if (x < 0 || x >= n || y < 0 || y >= n || grid[x][y] == 1 || visted[x][y]) {
                        continue;
                    } else {
                        queue.offer(new int[]{x, y});
                        visted[x][y] = true;
                    }
                }
            }
            ans++;
        }
        return -1;
    }
}
```

