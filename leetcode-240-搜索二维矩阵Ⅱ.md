---
title: leetcode-240-搜索二维矩阵Ⅱ
date: 2021-10-25 12:27:33
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
---

[$link$](https://leetcode-cn.com/problems/search-a-2d-matrix-ii/)

<hr/>

![image-20211026122820766](https://gitee.com/cao_ziqiang/img/raw/master/20211026122820.png)

![img](https://gitee.com/cao_ziqiang/img/raw/master/20211026122826.jpeg)

<hr/>

- 一个比较朴素的方法是直接搜索，两层循环即可；

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        for (int[] row : matrix) {
            for (int element : row) {
                if (element == target) {
                    return true;
                }
            }
        }
        return false;
    }
}
```

但是这样的做法显然就没有用到每行和每列的规律。

于是考虑如何使用到每一行和每一列的规律。

考虑左上角的每个点，它具有以下规律：

比它大的都在它的同一列下面，比它小的都在它同一行的左边。

于是若以$matrix[i][j]$作为左上角的顶点，当$matrix[i][j] \gt target$，则需要在同一行往左边找，于是$j--$；

当$matrix[i][j] \lt target$时，则说明需要去找更大的，则需要在它的同列的下面找，于是$i++$;

直到达到边界或是找到了则退出。

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int m = matrix.length;
        int n = matrix[0].length;
        int i = 0, j = n - 1;
        while(i < m && n >= 0) {
            if(matrix[i][j] == target) {
                return true;
            }else if (matrix[i][j] > target) {
                j--;
            }else {
                i++;
            }
        }
        return false;
    }
}
```

