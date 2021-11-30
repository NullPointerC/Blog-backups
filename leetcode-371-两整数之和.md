---
title: leetcode-371-两整数之和
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-09-26 18:01:14
---

[$link$](https://leetcode-cn.com/problems/sum-of-two-integers/)

<hr/>

![image-20210926180153595](https://gitee.com/cao_ziqiang/img/raw/master/20210926180153.png)

<hr/>

最简单的办法就是直接返回$ a + b$

```java
class Solution {
    public int getSum(int a, int b) {
        return a + b;
    }
}
```

<hr/>

也可以考虑位运算。异或就是不进位的加法,  再用与运算计算得进位结果。

```java
class Solution {
    public int getSum(int a, int b) {
        if(b == 0) {
            return a;
        }
        return getSum(a ^ b, (a & b) << 1);
    }
}
```

