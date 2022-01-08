---
title: leetcode-419-甲板上的战舰
date: 2021-12-18 12:07:46
categories: LeetCode
tags: [LeetCode,algorithm]
---

[419. 甲板上的战舰](https://leetcode-cn.com/problems/battleships-in-a-board/)

<hr/>

![image-20211218120825999](https://gitee.com/cao_ziqiang/img/raw/master/20211218120826.png)

**示例 1：**

![img](https://gitee.com/cao_ziqiang/img/raw/master/20211218120835.jpeg)

这里只放一个示例了。主要是要看懂示例的意思，上述示例$1$中的战舰数量是$2$。相信一定很奇怪，但是题目前后描述的战舰不是一个东西。我们很容易误以为是判断一行或者一列最多有多少个$X$。这样的代码也很容易写出来。

```java
class Solution {
    // private static final int[] dx = new int[]{-1, 0, 1, 0};
    // private static final int[] dy = new int[]{0, -1, 0, 1};
    public int countBattleships(char[][] board) {
        int m = board.length, n = board[0].length;
        int ans = 0;
        for(int i = 0; i < m; i++) {
            int temp = 0;
            for(int j = 0; j < n; j++) {
                if(board[i][j] == 'X') {
                    temp++;
                    j++;
                }
            }
            ans = Math.max(ans, temp);
        }
        for(int j = 0; j < n; j++) {
            int temp = 0;
            for(int i = 0; i < m; i++) {
                if(board[i][j] == 'X') {
                    temp++;
                    i++;
                }
            }
            ans = Math.max(ans, temp);
        }
        return ans;
    }
}
```

但是题目说到，“战舰”只能以$1 \times k$或$k \times 1$的形状建造。所以要把一整块$X$是为一个“战舰”。这才是题目要求我们求得的战舰数量。

理解了题意就不难，也就是要求求图中的连通分量个数。比较好写的方式是用$dfs$。

```java
class Solution {
    private static final int[] dx = new int[]{-1, 0, 1, 0};
    private static final int[] dy = new int[]{0, -1, 0, 1};
    public int countBattleships(char[][] board) {
        int m = board.length, n = board[0].length;
        int ans = 0;
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(board[i][j] == 'X') {
                    ans++;
                    dfs(i, j, board);
                }
            }
        }
        return ans;
    }
    private void dfs(int x, int y, char[][] board) {
        board[x][y] = '.';
        for(int i = 0; i < 4; i++) {
            int nx = x + dx[i], ny = y + dy[i];
            if(nx < 0 || nx >= board.length || ny < 0 || ny >= board[0].length) {
                continue;
            }
            if(board[nx][ny] == 'X') {
                dfs(nx, ny, board);
            }
        }
    }
}
```

将一整块$X$视为一个整体，并将相邻$X$都变为$.$。这样就比较容易的求出了连通分量数量。

Golang:

```go
var dx []int = []int{-1, 0, 1, 0}
var dy []int = []int{0, -1, 0, 1}
func countBattleships(board [][]byte) int {
    m, n := len(board), len(board[0])
    ans := 0
    var dfs func(x int, y int, board [][]byte)
    dfs = func(x int, y int, board [][]byte) {
        board[x][y] = '.'
        for i := 0; i < 4; i++ {
            nx, ny := x + dx[i], y + dy[i]
            if nx < 0 || nx >= m || ny < 0 || ny >= n {
                continue;
            }
            if board[nx][ny] == 'X' {
                dfs(nx, ny, board)
            }
        }
    }
    for i := 0; i < m; i ++ {
        for j := 0; j < n; j++ {
            if board[i][j] == 'X' {
                ans++
                dfs(i, j, board)
            }
        }
    }
    return ans
}
```

同样，也可以只求每个“战舰”的左上角，这样也可以只计算一次联通分量。

```go
func countBattleships(board [][]byte) int {
    m, n, ans := len(board), len( board[0]), 0
    for i := 0; i < m; i++ {
        for j := 0; j < n; j++ {
            if board[i][j] == '.' {
                continue
            }
            if i > 0 && board[i - 1][j] == 'X' {
                continue
            }
            if j > 0 && board[i][j - 1] == 'X' {
                continue
            }
            ans++
        }
    }
    return ans
}
```

