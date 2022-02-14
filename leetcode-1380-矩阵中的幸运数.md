---
title: leetcode-1380-矩阵中的幸运数
date: 2022-02-15 00:22:16
categories: LeetCode
tags: [LeetCode,algorithm]
---

[1380. 矩阵中的幸运数](https://leetcode-cn.com/problems/lucky-numbers-in-a-matrix/)

![image-20220215002255732](https://gitee.com/cao_ziqiang/img/raw/master/20220215002255.png)

枚举即可。用一个数组min存储每一行的最小值，用另一个数组max存储每一列的最大值。

```java
class Solution {
    public List<Integer> luckyNumbers (int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        List<Integer> res = new ArrayList<>();
        int[] min = new int[m];
        int[] max = new int[n];
        for(int i = 0; i < m; i++) {
            int tmp = Integer.MAX_VALUE;
            for(int j = 0; j < n; j++) {
                if(matrix[i][j] < tmp) tmp = matrix[i][j];
            }
            min[i] = tmp;
        }
        for(int j = 0; j < n; j++) {
            int tmp = Integer.MIN_VALUE;
            for(int i = 0; i < m; i++) {
                if(matrix[i][j] > tmp) tmp = matrix[i][j];
            }
            max[j] = tmp;
        }
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(matrix[i][j] == min[i] && matrix[i][j] == max[j]) {
                    res.add(matrix[i][j]);
                }
            }
        }
        return res;
    }
}
```

这里还看到有用min来存每一行最小值的下标，max存每一列最大值的下标。

```java
class Solution {
    public List<Integer> luckyNumbers (int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        List<Integer> res = new ArrayList<>();
        // 存储每一行的最小值下标以及每一列的最大值下标
        int[] rowMin = new int[m], colMax = new int[n];
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                // 比这一行的最小值还小更新
                if(matrix[i][j] < matrix[i][rowMin[i]]) {
                    rowMin[i] = j;
                }
                // 比这一列的最大值还大更新
                if(matrix[i][j] > matrix[colMax[j]][j]) {
                    colMax[j] = i;
                }
            }
        }
        for(int i = 0; i < m; i++) {
            // 如果当前行等于
            if(i == colMax[rowMin[i]]) res.add(matrix[i][rowMin[i]]);
        }
        return res;
    }
}
```