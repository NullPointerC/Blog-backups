---
title: leetcode-598-范围求和Ⅱ
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: 92c0faed
date: 2021-11-07 18:36:11
---

[$link$](https://leetcode-cn.com/problems/range-addition-ii/)

<hr/>

![image-20211107183706704](https://gitee.com/cao_ziqiang/img/raw/master/20211107183706.png)

![image-20211107183718444](https://gitee.com/cao_ziqiang/img/raw/master/20211107183718.png)

思路比较好想到，因为每次都会对$0 \le x \le m$，$0 \le y \le n$这个区域加上一个数，所以考虑维护这样的一个最小的矩阵，最后输出$x \times y$即可。

```go
func maxCount(m int, n int, ops [][]int) int {
    for _, v := range ops {
        if m > v[0] {
            m = v[0]
        }
        if n > v[1] {
            n = v[1]
        }
    }
    return m * n
}
```

