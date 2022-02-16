---
title: leetcode-292-Nim游戏
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: 268e088f
date: 2021-09-18 19:03:36
---

[$link$](https://leetcode-cn.com/problems/nim-game/)

<hr/>

![image-20210918190521426](https://gitee.com/cao_ziqiang/img/raw/master/20210918190521.png)

<hr/>

当出现$4$的倍数时，对手总是可以让你回到$4$的倍数的情况，而$4$刚好是赢不了的情况，所以只要你拿到的数量是$4$的倍数，对手总是可以让你回到$4$的情况。 想赢也是同样的道理，每次给对手留下$4$的倍数的数量即可。

```java
class Solution {
    public boolean canWinNim(int n) {
        return (n & 3) != 0;   
    }
}
```

