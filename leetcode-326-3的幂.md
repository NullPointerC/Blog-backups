---
title: leetcode-326-3的幂
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-09-23 16:14:04
---

![image-20210923161434382](https://gitee.com/cao_ziqiang/img/raw/master/20210923161434.png)

对于负数和$0$，可以直接剪枝，因为正数的幂次一定大于$0$。对于其他整数，因为$3^n$满足$3\times3\times...\times3$。所以不断除以$3$，并判断是否余数为0，最后如果是3的幂次应该会返回1.

```java
class Solution {
    public boolean isPowerOfThree(int n) {
        if(n <=0 ) {
            return false;
        }
        while(n % 3 == 0) {
            n /= 3;
        }
        return n == 1;
    }
}
```

