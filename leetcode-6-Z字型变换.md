---
title: leetcode-6-Z字型变换
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: a1a2ad8
date: 2022-03-01 11:38:38
---

[6. Z 字形变换](https://leetcode-cn.com/problems/zigzag-conversion/)

![image-20220301113919777](https://gitee.com/cao_ziqiang/img/raw/master/20220301113919.png)

字符串模拟题，比较讨厌哈😂。

定义一个二维的`matrix`存放Z字型的字符串，如果是往下的顺序，那么在同一列上继续排即可，如果不是往下的顺序，那么就需要往右上角的方向移动。

```java
class Solution {
    public String convert(String s, int numRows) {
        if(s == null || s.length() == 0) {
            return "";
        }
        if(numRows == 1) {
            return s;
        }
        char[] chars = s.toCharArray();
        int n = chars.length;
        StringBuilder sb = new StringBuilder();
        char[][] matrix = new char[numRows][n];
        for(int i = 0; i < numRows; i++) {
            Arrays.fill(matrix[i], ' ');
        }
        int x = 0, y = 0;
        boolean down = true;
        for(int i = 0; i < n; i++) {
            matrix[x][y] = chars[i];
            if(x < numRows && down) {
                x++;
            }
            if(x == numRows) {
                x--;
                down = false;
            }
            if(x > 0 && !down) {
                x--;
                y++;
            }
            if(x == 0) {
                down = true;
            }
        }

        for(int i = 0; i < numRows; i++) {
            for(int j = 0; j < n; j++) {
                if(matrix[i][j] != ' ') {
                    sb.append(matrix[i][j]);
                }
            }
        }
        return sb.toString();
    }
}
```

设`numRows`行字符串分别是`s1,s2...sn`，可以发现在遍历`s`时，每个字符`c`在`Z`字型的变换中是先增后减的，模拟这个行索引的变化即可。

可以按顺序遍历`s`，更新行索引`i+=flag`，且到达了转折点时，变换行索引·`falg = -flag`。

```java
class Solution {
	public String convert(String s, int numRows) {
        if(s == null || s.length() == 0) {
            return "";
        }
        if(numRows < 2) {
            return s;
        }
        char[] chars = s.toCharArray();
        List<StringBuilder> rows = new ArrayList<>(numRows);
        for(int i = 0; i < numRows; i++) {
            rows.add(new StringBuilder());
        }
        int curRow = 0, flag = 1;
        for(char c : chars) {
            rows.get(curRow).append(c);
            curRow += flag;
            if(curRow == 0 || curRow == numRows - 1) {
                flag = -flag;
            }
        }
        StringBuilder res = new StringBuilder();
        for(StringBuilder row : rows) {
            res.append(row);
        }
        return res.toString();
    }	
}
```

