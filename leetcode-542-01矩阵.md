---
title: leetcode-542-01矩阵
date: 2021-11-08 20:45:27
categories: LeetCode
tags: [LeetCode,algorithm]
---

[$link$](https://leetcode-cn.com/problems/01-matrix/)

<hr/>

![image-20211108204605686](https://gitee.com/cao_ziqiang/img/raw/master/20211108204605.png)

<hr/>

本来是不难的，脑抽一下子一直纠结于$dfs$，结果写了半天还是错的$dfs$，因为没有解决重复统计这个问题，如果在更新了一个后一个1之前，更新了前一个1的值，并且还是遍历上下左右，就会陷入递归死循环。。。

那么应该怎么样正确的$dfs$呢？应该在遍历到1的时候，做一步预处理，如果元素0在附近，保留元素值1，如果不再0附近，初始化为一个较大的值。

```java
class Solution {
    private int row;
    private int col;
    private int[][] vector = new int[][]{{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
	public int[][] updateMatrix(int[][] matrix) {
        row = matrix.length;
        col = matrix[0].length;
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                if (matrix[i][j] == 1
                        && !((i > 0 && matrix[i - 1][j] == 0)
                        || (i < row - 1 && matrix[i + 1][j] == 0)
                        || (j > 0 && matrix[i][j - 1] == 0)
                        || (j < col - 1 && matrix[i][j + 1] == 0))) {
                    matrix[i][j] = row + col;
                }
            }
        }
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[0].length; j++) {
                if (matrix[i][j] == 1) {
                    dfs(matrix, i, j);
                }
            }
        }
        return matrix;
    }
    private void dfs(int[][] matrix, int r, int c) {
        // 搜索上下左右四个方向
        for (int[] v : vector) {
            int nr = r + v[0], nc = c + v[1];
            if (nr >= 0 && nr < row
                    && nc >= 0 && nc < col
                    && matrix[nr][nc] > matrix[r][c] + 1) {
                matrix[nr][nc] = matrix[r][c] + 1;
                dfs(matrix, nr, nc);
            }
        }
    }
}
```

同时，看到很多大佬提到，这是一道经典的多源$bfs$，什么是多源$bfs$呢，其实树的$bfs$就是单源$bfs$，因为只要先把root节点入队，再一层一层遍历就好了，图的$bfs$做法其实也是一样的，但是注意到树只有一个root，而图有多个root，所以在初始化的时候，就要把所有的源点入队，树是有向的，所以不需要标识是否被访问过，但是如果是无向图，就需要标识位来标识是否被访问过，并且为了防止某个节点多此入队，在它入队之前就需要把节点设置为已经访问过的状态。

所以，可以首先将所有的0先入队，在从各个0同时开始一圈一圈地向1扩散，

```java
class Solution {
    public int[][] updateMatrix(int[][] matrix) {
    	// 首先将所有的 0 都入队，并且将 1 的位置设置成 -1，表示该位置是 未被访问过的 1
        Queue<int[]> queue = new LinkedList<>();
        int m = matrix.length, n = matrix[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 0) {
                    queue.offer(new int[] {i, j});
                } else {
                    matrix[i][j] = -1;
                } 
            }
        }
        int[] dx = new int[] {-1, 1, 0, 0};
        int[] dy = new int[] {0, 0, -1, 1};
        while (!queue.isEmpty()) {
            int[] point = queue.poll();
            int x = point[0], y = point[1];
            for (int i = 0; i < 4; i++) {
                int newX = x + dx[i];
                int newY = y + dy[i];
                // 如果四邻域的点是 -1，表示这个点是未被访问过的 1
                // 所以这个点到 0 的距离就可以更新成 matrix[x][y] + 1。
                if (newX >= 0 && newX < m && newY >= 0 && newY < n 
                        && matrix[newX][newY] == -1) {
                    matrix[newX][newY] = matrix[x][y] + 1;
                    queue.offer(new int[] {newX, newY});
                }
            }
        }
        return matrix;
    }
}
```

还可以继续考虑使用动态规划,我们发现,如果1周围有1,那么就可以利用这一点去求解距离。

定义$dp[i][j]$来表示该位置距离最近的0的距离，我们发现$dp[i][j]$是由上下左右4个位置来决定，所以考虑先向右下角遍历，再从右下角回到左上角。

![image-20211108211635262](https://gitee.com/cao_ziqiang/img/raw/master/20211108211635.png)

```java
class Solution {
  public int[][] updateMatrix(int[][] matrix) {
    int m = matrix.length, n = matrix[0].length;
    int[][] dp = new int[m][n];
    for (int i = 0; i < m; i++) {
      for (int j = 0; j < n; j++) {
        dp[i][j] = matrix[i][j] == 0 ? 0 : 10000;
      }
    }

    // 从左上角开始
    for (int i = 0; i < m; i++) {
      for (int j = 0; j < n; j++) {
        if (i - 1 >= 0) {
          dp[i][j] = Math.min(dp[i][j], dp[i - 1][j] + 1);
        }
        if (j - 1 >= 0) {
          dp[i][j] = Math.min(dp[i][j], dp[i][j - 1] + 1);
        }
      }
    }
    // 从右下角开始
    for (int i = m - 1; i >= 0; i--) {
      for (int j = n - 1; j >= 0; j--) {
        if (i + 1 < m) {
          dp[i][j] = Math.min(dp[i][j], dp[i + 1][j] + 1);
        }
        if (j + 1 < n) {
          dp[i][j] = Math.min(dp[i][j], dp[i][j + 1] + 1);
        }
      }
    }
    return dp;
  }
}
```

