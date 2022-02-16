---
title: leetcode-191-位1的个数
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: 6dc69d9b
date: 2021-11-12 17:34:50
---

[$link$](https://leetcode-cn.com/problems/number-of-1-bits/)

<hr/>

![image-20211112173531674](https://gitee.com/cao_ziqiang/img/raw/master/20211112173531.png)

<hr/>

我们都知道$n \& (n-1)$可以消去最低位的1，所以只要$n$不为0，就使$n \&= (n-1)$。

```
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int cnt = 0;
        while (n != 0) {
            n &= n - 1;
            cnt++;
        }
        return cnt;
    }
}
```

由于测试数据也不是很大，所以也可以直接从0遍历到31；

```go
func hammingWeight(num uint32) (ans int) {
    for i := 0;i < 32;i++ {
        if 1 << i & num > 0 {
            ans++
        }
    }
    return
}
```

