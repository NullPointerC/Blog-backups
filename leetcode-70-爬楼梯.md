---
title: leetcode-70-爬楼梯
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-09-26 18:08:44
---

[$link$](https://leetcode-cn.com/problems/climbing-stairs/)

<hr/>

![image-20210926181200014](https://gitee.com/cao_ziqiang/img/raw/master/20210926181200.png)

<hr/>

很明显这又是动态规划类型的题目,当前问题可以分解为更小的子问题,当前问题就是要求$n$级楼梯有多少种走法,可以得到,当前$n$级楼梯是从更低的楼梯上来的,只要先知道$n-1$和$n-2$级楼梯怎么走,就可以退出$n$级楼梯怎么走。

$f[i] = f[i-1]+f[i-2]$

```java
class Solution {
    public int climbStairs(int n) {
        if(n == 1 || n == 2) {
            return n;
        }
        int prev = 1;
        int curr = 2;
        for(int i = 3; i <= n; i++) {
            int temp = prev + curr;
            prev = curr;
                        curr = temp;
        }
        return curr;
    }
}
```

