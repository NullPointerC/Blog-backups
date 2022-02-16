---
title: leetcode-733-图像渲染
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: 267b92d0
date: 2021-11-06 12:38:12
---

[$link$](https://leetcode-cn.com/problems/flood-fill/)

<hr/>

![image-20211106123905625](https://gitee.com/cao_ziqiang/img/raw/master/20211106123905.png)

<hr/>

首先想到的是BFS，因为前天接了雨水，![img](https://gitee.com/cao_ziqiang/img/raw/master/20211106123950.jpg)

脑子里一直想的是BFS。

思路也比较简单，定义一个$visted$数组标记是否访问过，添加一个双端队列$queue$。每次先将符合条件的添加到队列中，再依次从队列中取出，遍历这个点的上下左右，若它的上下左右的店也满足条件，再将符合条件的点也添加进队列中去，并标记已被访问过，防止重复访问。

```java
class Solution {
    public int[][] floodFill(int[][] image, int sr, int sc, int newColor) {
        if(image[sr][sc] == newColor){
             return image;
        }
        int m = image.length, n = image[0].length;
        boolean[][] visted = new boolean[m][n];
        int[] dx = new int[] {-1, 0, 1, 0};
        int[] dy = new int[] {0, 1, 0, -1};
        Deque<int[]> queue = new ArrayDeque<>();
        int oldColor = image[sr][sc];
        queue.offer(new int[]{sr, sc});
        visted[sr][sc] = true;
        while (!queue.isEmpty()) {
            int[] point = queue.poll();
            for (int i = 0; i < 4; i++) {
                int x = point[0] + dx[i];
                int y = point[1] + dy[i];
                if (isValidPoint(x, y, m, n)) {
                    if (image[x][y] == oldColor && !visted[x][y]) {
                        queue.offer(new int[]{x, y});
                        visted[x][y] = true;
                    }
                }
            }
            image[point[0]][point[1]] = newColor;
        }
        return image;
    }
    private boolean isValidPoint(int x, int y, int m, int n) {
        if (x >= 0 && x < m && y >= 0 && y < n) {
            return true;
        }
        return false;
    }
}
```

<hr/>

后面看了看评论，应该是dfs更快些。

```java
class Solution {
    int[][] image;
    int oldColor;
    int newColor;
    public int[][] floodFill(int[][] image, int sr, int sc, int newColor) {
        this.image = image;
        this.oldColor = image[sr][sc];
        this.newColor = newColor;
        dfs(sr, sc); 
        return this.image;  
    }

    private void dfs(int r, int c) {
        if(r < 0 || r >= image.length || c < 0 || c >= image[0].length || image[r][c] == newColor || image[r][c] != oldColor) {
            return ;
        }
        image[r][c] = newColor;
        dfs(r + 1, c);
        dfs(r - 1, c);
        dfs(r, c + 1);
        dfs(r, c - 1);
    }
}
```

