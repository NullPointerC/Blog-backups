---
title: leetcode-509-斐波那契数
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-09-25 09:50:38
---

[link](https://leetcode-cn.com/problems/fibonacci-number/)

![image-20210925095126807](https://gitee.com/cao_ziqiang/img/raw/master/20210925095126.png)

<hr/>

<hr/>

> 从这里开始刷一段时间的动态规划，必须把它拿下了！！！

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210925095306.gif)

这题是很经典的动态规划,动态规划在我的理解里就是解决当前问题还需要用到比当前问题规模更小的子问题,所以我们不断将当前问题拆分到我们更好解决的子问题,最后拆分到好解决的子问题,再让程序自底向上的进行规划。

<hr/>

回到问题就是，我们要求$F(n)$，而$F(n)$的值依赖于$F(n-1)$和$F(n-2)$。我们再求$F(n-1)$和$F(n-2)$的时候又可以不自己求解，而是转而化为更小的子问题，直到化到最小的子问题，即题目给出的$n=0$和$n=1$的情况。

再由程序自底向上的归纳。

```java
class Solution {
    public int fib(int n) {
        if(n == 0||n == 1) {
            return n;
        }
        int prev = 0;
        int curr = 1;
        for(int i = 2; i <= n; i++) {
            int next = prev + curr;
            prev = curr;
            curr = next; 
        }
        return curr;
    }
}
```

