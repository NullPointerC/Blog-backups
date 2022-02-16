---
title: leetcode-794-有效的井字游戏
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: cb5d1e0f
date: 2021-12-09 18:12:09
---

[有效的井字游戏](https://leetcode-cn.com/problems/valid-tic-tac-toe-state/)

<hr/>

![image-20211209181301791](https://gitee.com/cao_ziqiang/img/raw/master/20211209181301.png)

**示例 1：**

![img](https://gitee.com/cao_ziqiang/img/raw/master/20211209181335.jpeg)

```
输入：board = ["O  ","   ","   "]
输出：false
解释：玩家 1 总是放字符 "X" 。
```

**示例 2：**

![img](https://gitee.com/cao_ziqiang/img/raw/master/20211209181343.jpeg)

```
输入：board = ["XOX"," X ","   "]
输出：false
解释：玩家应该轮流放字符。
```

**示例 3：**

![img](https://gitee.com/cao_ziqiang/img/raw/master/20211209181351.jpeg)

```
输入：board = ["XXX","   ","OOO"]
输出：false
```

**Example 4:**

![img](https://gitee.com/cao_ziqiang/img/raw/master/20211209181358.jpeg)

```
输入：board = ["XOX","O O","XOX"]
输出：true
```

tags: (字符串)

比较麻烦,主要是判断几种情况。

1. $X$的数量比$O$的数量多一个或者相同；
2. $X$和$O$不能同时获胜；
3. $X$获胜的时候比$O$多一个；
4. $O$获胜的时候比$X$相同；

```java
class Solution {
    public boolean validTicTacToe(String[] board) {
        int cntX = 0, cntO = 0;
        for(int i = 0; i < 3; i++) {
            for(char c : board[i].toCharArray()) {
                if(c == 'X') {
                    cntX++;
                }
                if(c == 'O') {
                    cntO++;
                }
            }
        }
        // System.out.println(cntX + "\t" + cntO);
        if(cntX - cntO != 0 && cntX - cntO != 1) {
            return false;
        }
        if(isWin(board, 'X') && isWin(board, 'O')) {
            return false;
        }
        if(isWin(board, 'X') && (cntX - cntO != 1)) {
            return false;
        }
        if(isWin(board, 'O') && (cntX - cntO != 0)) {
            return false;
        }
        return true;
    }
    private boolean isWin(String[] board, char c) {
        if (board[0].charAt(0) == board[1].charAt(1) && board[1].charAt(1) == board[2].charAt(2) && board[2].charAt(2) == c) {
            return true;
        }
        if (board[0].charAt(2) == board[1].charAt(1) && board[1].charAt(1) == board[2].charAt(0) && board[2].charAt(0) == c) {
            return true;
        }
        for(int i = 0; i < 3; i++){
            if(board[i].charAt(0) == board[i].charAt(1) && board[i].charAt(1) == board[i].charAt(2) && board[i].charAt(2) == c){
                return true;
            }
            if(board[0].charAt(i) == board[1].charAt(i) && board[1].charAt(i) == board[2].charAt(i) && board[2].charAt(i) == c){
                return true;
            }
        }
        return false;
    }
}
```

