---
title: leetcode-407-接雨水Ⅱ
date: 2021-11-03 20:15:48
categories: LeetCode
tags: [LeetCode,algorithm]
---

[$link$](https://leetcode-cn.com/problems/trapping-rain-water-ii/solution/)

<hr/>

![image-20211103201647298](https://gitee.com/cao_ziqiang/img/raw/master/20211103201647.png)

<hr/>

这题确实难啊，菜鸡的我看了好多题解也不懂，还是找了挺多视频看才懂了，在这里记录一下。

首先要了解一些木桶理论，装水量应该是由最短的来决定的。

一个一个格子地去找显然是很花时间的，而且可能因为相邻格子的装水量改变而需要动态地去改变。

所以考虑从外层往内层去找，对于一个格子，它的装水量应该由它上下左右四个格子来决定，如果它的周围的格子都比它小，那么就不能装水，所以应该是比它高的格子中的最小的那个就是最终的高度。

也就是说：从外到内，最外层影响次外层，次外层又变成新的最外层，直到都遍历完。

使用优先队列也就是小顶堆来存储最外层的节点，因为是从外往里使用最小的那个，所以一直将最外层的节点存进优先队列中，并将原最外层移除队列且设置一个$vis$数组代表是否已经访问过该节点。

每个节点有3个属性，分别是$x、y、h$，不断从队列中$poll$出最小的节点，找比它小的节点，往里面灌水。同时标记它的相邻节点已经被访问过了，并将它的相邻节点入队。

```java
class Solution {
    public int trapRainWater(int[][] heightMap) {
        int m = heightMap.length, n = heightMap[0].length;
        PriorityQueue < int[] > pq = new PriorityQueue < > ((a, b) -> a[2] - b[2]);
        boolean[][] vis = new boolean[m][n];
        for(int i = 0; i < n; i++) {
            pq.add(new int[] {0, i, heightMap[0][i]});
            pq.add(new int[] {m - 1, i, heightMap[m - 1][i]});
            vis[0][i] = vis[m - 1][i] = true;
        }
        for(int i = 1; i < m - 1; i++) {
            pq.add(new int[] {i, 0, heightMap[i][0]});
            pq.add(new int[] {i, n - 1, heightMap[i][n - 1]});
            vis[i][0] = vis[i][n - 1] = true;
        }
        int[][] dirs = new int[][] {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
        int ans = 0;
        while(!pq.isEmpty()) {
            int[] poll = pq.poll();
            int x = poll[0], y = poll[1], h = poll[2];
            for(int[] d: dirs) {
                int nx = x + d[0], ny = y + d[1];
                if(nx < 0 || nx >= m || ny < 0 || ny >= n) continue;
                if(vis[nx][ny]) continue;
                if(h > heightMap[nx][ny]) {
                    ans += h - heightMap[nx][ny];
                }
                pq.add(new int[] {nx, ny, Math.max(heightMap[nx][ny], h)});
                vis[nx][ny] = true;
            }
        }
        return ans;
    }
}
```

