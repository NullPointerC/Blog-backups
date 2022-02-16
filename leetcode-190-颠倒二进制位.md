---
title: leetcode-190-颠倒二进制位
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: ca41e32e
date: 2021-11-13 23:46:00
---

[$link$](https://leetcode-cn.com/problems/reverse-bits/)

<hr/>

![image-20211114154706264](https://gitee.com/cao_ziqiang/img/raw/master/20211114154706.png)

<hr/>

题目还是比较容易的,还可以让我们回顾一下计数的原理。

循环是比较容易想到的办法，每次把$ans$左移，把$num$二进制末尾数字拼接到$ans$的末尾，然后把$num$左移。

```go
func reverseBits(num uint32) uint32 {
    i := 32
    var ans uint32 = 0
    for i != 0 {
        // 取二进制n的最后一位bit
        bit := num & 1
        // num右移
        num >>= 1
        // ans左移并加上这个进位
        ans = (ans << 1) + bit
        i--
    }
    return ans
}
```

以上是利用循环的，还有一种采用了归并排序中的分治思想。

![190.001.jpeg](https://gitee.com/cao_ziqiang/img/raw/master/20211114160941.jpeg)

```go
func reverseBits(n uint32) uint32 {
    n = (n >> 16) | (n << 16)
    n = ((n & 0xff00ff00) >> 8) | ((n & 0x00ff00ff) << 8)
    n = ((n & 0xf0f0f0f0) >> 4) | ((n & 0x0f0f0f0f) << 4)
    n = ((n & 0xcccccccc) >> 2) | ((n & 0x33333333) << 2)
    n = ((n & 0xaaaaaaaa) >> 1) | ((n & 0x55555555) << 1)
    return n
}
```

