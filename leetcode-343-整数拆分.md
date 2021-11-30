---
title: leetcode-343-整数拆分
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-09-18 11:22:26
---

[link](https://leetcode-cn.com/problems/integer-break/)

<hr/>

![image-20210918112315484](https://gitee.com/cao_ziqiang/img/raw/master/20210918112315.png)

<hr/>

一看到$n$不小于$2$且不大于$58$。我就知道，打表的机会来了。

```cpp
class Solution {
public:
    int integerBreak(int n) {
    return (vector<int>{ -1, -1, 1, 2, 4, 6, 9, 12, 18, 27, 36, 54, 81, 108, 162, 243, 324, 486, 729, 972, 1458, 2187, 2916, 4374, 6561, 8748, 13122, 19683, 26244, 39366, 59049, 78732, 118098, 177147, 236196, 354294, 531441, 708588, 1062882, 1594323, 2125764, 3188646, 4782969, 6377292, 9565938, 14348907, 19131876, 28697814, 43046721, 57395628, 86093442, 129140163, 172186884, 258280326, 387420489, 516560652, 774840978, 1162261467, 1549681956  })[n];
}
};
```

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210918112423.jpg)

当然这不是正经解法,正经解法应该怎么样呢, 首先我们考虑$f[i]$表示以$i$做整数拆分可得到的最大乘积

再考虑如何得到$f[i]$,它可以是由两个数分割而来,可以是由多个数分割而来

如果是两个数分割而来,那么就应该满足:

$f[i] = max(f[i],(i - j) \times j) \quad (2\le j \le j)$

如果是由多个数分割而来,那么就应该满足:

$f[i] = max(f[i],f[i - j] \times j)\quad(2\le j \le i)$

综上,状态转移方程应该为:

$f[i] = max(f[i],max((i-j)\times j,f[i-j]\times j))$

```java
class Solution {
    public int integerBreak(int n) {
        int[] f = new int[n + 1];
        f[2] = 1;
        for(int i = 3; i<= n; i++) {
            for(int j = 1; j < i - 1; j++) {
                f[i] = Math.max(f[i], Math.max((i - j) * j, f[i - j] * j));
            }
        }
        return f[n];
    }
}
```

