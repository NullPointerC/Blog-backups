---
title: leetcode-130-被围绕的区域
date: 2021-11-22 19:29:00
categories: LeetCode
tags: [LeetCode,algorithm]
---

[被围绕的区域](https://leetcode-cn.com/problems/surrounded-regions/)

<hr/>

![image-20211122192952223](https://gitee.com/cao_ziqiang/img/raw/master/20211122192952.png)

<hr/>

由于题目说到边界的不算被围绕的区间，因此，我们可以考虑从边界开始搜索，先将和边界相邻的块都标记，再遍历一趟矩阵，若为$'0'$且未和边界相连，就将其修改为$'X'$。

```java
class Solution {
    private static final int[] dx = new int[]{1, -1, 0, 0};
    private static final int[] dy = new int[]{0, 0, 1, -1};
    public void solve(char[][] board) {
        int m = board.length;
        int n = board[0].length;
        Queue<int[]> queue = new ArrayDeque<>();
        boolean[][] visted = new boolean[m][n];
        // 遍历所有行的第一轮和最后一列
        for(int i = 0; i < m; i++) {
            // 遍历所有行的第一列
            if(board[i][0] == 'O' && !visted[i][0]) {
                queue.offer(new int[]{i, 0});
                visted[i][0] = true;
            }
            // 遍历所有行的最后一列
            if(board[i][n - 1] == 'O' && !visted[i][n - 1]) {
                queue.offer(new int[]{i, n - 1});
                visted[i][n - 1] = true;
            }
        }
        // 遍历所有的列
        for(int i = 0; i < n; i++) {
            // 第一行的所有列
            if(board[0][i] == 'O' && !visted[0][i]) {
                queue.offer(new int[]{0, i});
                visted[0][i] = true;
            }
            // 最后一行的所有列
            if(board[m - 1][i] == 'O' && !visted[m - 1][i]) {
                queue.offer(new int[]{m - 1, i});
                visted[m - 1][i] = true;
            }
        }
        
        while(!queue.isEmpty()) {
            int[] cell = queue.poll();
            for(int i = 0; i < 4; i++) {
                int nx = cell[0] + dx[i];
                int ny = cell[1] + dy[i];
                if(nx < 0 || nx >= m || ny < 0 || ny >= n || visted[nx][ny]) {
                    continue;
                }
                if(board[nx][ny] == 'O') {
                    queue.offer(new int[]{nx, ny});
                    visted[nx][ny] = true;
                }
            }
        }
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if (board[i][j] == 'O' && !visted[i][j]) {
                    board[i][j] = 'X';
                }
            }
        }
    }
}
```

当然，使用$dfs$也是可以的。

```java
class Solution {
    private static final int[] dx = new int[]{1, -1, 0, 0};
    private static final int[] dy = new int[]{0, 0, 1, -1};
    private boolean[][] visted;
    private char[][] board;
    public void solve(char[][] board) {
        int m = board.length;
        int n = board[0].length;
        this.board = board;
        this.visted = new boolean[m][n];
        // 遍历所有行的第一轮和最后一列
        for(int i = 0; i < m; i++) {
            // 遍历所有行的第一列
            if(board[i][0] == 'O' && !visted[i][0]) {
                dfs(i, 0);
            }
            // 遍历所有行的最后一列
            if(board[i][n - 1] == 'O' && !visted[i][n - 1]) {
                dfs(i, n - 1);
            }
        }
        
        // 遍历所有的列
        for(int i = 0; i < n; i++) {
            // 第一行的所有列
            if(board[0][i] == 'O' && !visted[0][i]) {
                dfs(0, i);
            }
            // 最后一行的所有列
            if(board[m - 1][i] == 'O' && !visted[m - 1][i]) {
                dfs(m - 1, i);
            }
        }
        // System.out.println(Arrays.deepToString(visted));
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if (board[i][j] == 'O' && !visted[i][j]) {
                    board[i][j] = 'X';
                }
            }
        }
    }
    private void dfs(int i, int j) {
        if (i < 0 || i >= board.length || j < 0 || j >= board[0].length || visted[i][j] || board[i][j] != 'O') {
            return ;
        }
        visted[i][j] = true;
        for(int k = 0; k < 4; k++) {
            dfs(i + dx[k], j + dy[k]);
        }
    }
}
```

