---
title: leetcode-74-搜索二维矩阵
date: 2021-11-14 20:13:22
categories: LeetCode
tags: [LeetCode,algorithm]
---

[$link$](https://leetcode-cn.com/problems/search-a-2d-matrix/)

<hr/>

![image-20211114201407264](https://gitee.com/cao_ziqiang/img/raw/master/20211114201409.png)

<hr/>

一个比较好的办法是，将二维的坐标$(i,j)$一维化，第一个个元素$(0,0)$的坐标一维化即为0，最后一个元素的坐标$(m-1,n-1)$一维化就为3$m\times n-1$。

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int m = matrix.length, n = matrix[0].length;
        int low = 0, high = m * n - 1;
        while (low <= high) {
            int mid = ((high - low) >> 1) + low;
            int row = mid / n;
            int col = mid % n;
            if (matrix[row][col] == target) {
                return true;
            } else if (matrix[row][col] > target){
                high = mid - 1;
            } else {
                low = mid + 1;
            }
        }
        return false;
    }
}
```

```go
func searchMatrix(matrix [][]int, target int) bool {
    m, n := len(matrix), len(matrix[0])
    low, high := 0, (m * n - 1)
    for low <= high {
        mid := ((high - low) >> 1) + low
        row := mid / n
        col := mid % n
        if matrix[row][col] == target {
            return true
        } else if matrix[row][col] > target {
            high = mid - 1
        } else {
            low = mid + 1
        }
    }
    return false
}
```

也可以考虑两次二分，第一轮二分先找到比$target$小的最大行。

再从这一行开始二分，找到$target$。

```go
func searchMatrix(matrix [][]int, target int) bool {
    m, n := len(matrix), len(matrix[0])
    up, down := 0, m - 1
    left, right := 0, n - 1
    rowMid, colMid := -1, -1
    for up <= down {
        rowMid = ((down - up) >> 1) + up
        if matrix[rowMid][0] == target {
            return true
        } else if (matrix[rowMid][0] < target) {
            up = rowMid + 1
        } else {
            down = rowMid - 1
        }
    }
    if down < 0 {
        return false
    }
    for left <= right {
        colMid = ((right - left) >> 1) + left
        if matrix[down][colMid] > target {
            right = colMid - 1
        } else if matrix[down][colMid] < target {
            left = colMid + 1
        } else {
            return true
        }
    }
    return false
}
```

