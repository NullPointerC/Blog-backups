---
title: leetcode-2022-将一维数组转变为二维数组
date: 2022-01-01 17:21:08
categories: LeetCode
tags: LeetCode,algorithm
---

![image-20220101172143030](https://gitee.com/cao_ziqiang/img/raw/master/20220101172143.png)

[2022. 将一维数组转变成二维数组](https://leetcode-cn.com/problems/convert-1d-array-into-2d-array/)

2022年的第一题, 争取今年把leetcode全部刷绿。

![QQ图片20220101172315](https://gitee.com/cao_ziqiang/img/raw/master/20220101172429.jpg)

比较简单的模拟，将一位的索引和二维的做一个转换即可。

$x \rightarrow i /n$

$y \rightarrow i \% n$

```java
class Solution {
    public int[][] construct2DArray(int[] original, int m, int n) {
        int len = original.length;
        if(len != m * n) {
            return new int[][]{};
        }
        int[][] ans = new int[m][n];
        for(int i = 0; i < len; i++) {
            // System.out.println((i / n) + "\t" + (i % n));
            ans[i / n][i % n] = original[i];
        }
        return ans;
    }
}
```

