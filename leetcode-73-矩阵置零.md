---
title: leetcode-73-矩阵置零
date: 2021-07-10 09:03:36
categories: LeetCode
tags: [algorithm,Java,LeetCode]
---

题目描述:

<pre>
给定一个 m x n 的矩阵，如果一个元素为 0 ，则将其所在行和列的所有元素都设为 0 。请使用 原地 算法。
</pre>

示例1:

<pre>
输入：matrix = [[1,1,1],[1,0,1],[1,1,1]]
输出：[[1,0,1],[0,0,0],[1,0,1]]
</pre>

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210710092421.jpeg)

示例2:

<pre>
输入：matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]
输出：[[0,0,0,0],[0,4,5,0],[0,3,1,0]]
</pre>

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210710092541.png)

<pre>
输入：matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]
输出：[[0,0,0,0],[0,4,5,0],[0,3,1,0]]
</pre>

思路:

<pre>
第一次遍历找出0所在的位置,然后添加进positions数组,然后依次取出position
根据postion / n,postion % n 找出行列row,col
再将row,col所在的行列置为0即可
</pre>

代码:

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        int m = matrix.length;
        int n  = matrix[0].length;
        if(m == 0 || n == 0 || matrix == null) return;
        int[] positions = new int[m * n];
        int index = 0;
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(matrix[i][j] == 0) positions[index++] = i * n + j;
            }
        }
        for(int i = 0; i < index; i++) {
            int position = positions[i];
            int row = position / n;
            int col = position % n;
            for(int j = 0; j < m; j++) matrix[j][col] = 0;
            for(int j = 0; j < n; j++) matrix[row][j] = 0;
        }
    }
}
```

![image-20210710092833254](https://gitee.com/cao_ziqiang/img/raw/master/20210710092833.png)

