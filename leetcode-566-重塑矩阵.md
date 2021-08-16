---
title: leetcode-566-重塑矩阵
date: 2021-07-05 12:29:25
categories: algorithm
tags: [algorithm,Java,LeetCode]
---

[link](https://leetcode-cn.com/problems/reshape-the-matrix/)

<hr/>

![image-20210705123021233](https://gitee.com/cao_ziqiang/img/raw/master/20210705123021.png)

思路:很常见的模拟题,只需要先将原数组mat的数据都放入一维数组中,然后再根据

ret [i] [j] = matrix[i * c + j]的规律放回即可

```java
class Solution {
    public int[][] matrixReshape(int[][] mat, int r, int c) {
        int rows = mat.length;
        int cols = mat[0].length;
        if(rows * cols != r * c) {
            return mat;
        }
        if(rows == 0 || cols == 0) {
            return mat;
        }
        int[] matrix = new int[rows * cols];
        int index = 0;
        for(int i = 0; i < rows; i++) {
            for(int j = 0; j < cols; j++) {
                matrix[index++] = mat[i][j];
            }
        }
        int[][] ret = new int[r][c];
        for(int i = 0; i < r; i++) {
            for(int j = 0; j < c; j++) {
                ret[i][j] = matrix[i * c + j];
            }
        }
        return ret;
    }
}
```

![image-20210705123449668](https://gitee.com/cao_ziqiang/img/raw/master/20210705123449.png)

