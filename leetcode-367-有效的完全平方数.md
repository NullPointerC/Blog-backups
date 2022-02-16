---
title: leetcode-367-有效的完全平方数
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: 1609f186
date: 2021-11-04 08:33:25
---

[$link$](https://leetcode-cn.com/problems/valid-perfect-square/)

<hr/>

![image-20211104083417277](https://gitee.com/cao_ziqiang/img/raw/master/20211104083417.png)

<hr/>

上实验课就有时间摸鱼🐟了，就来写题了，今天的题还是比较容易的，就是有些细节需要考虑清楚，尤其是范围不要爆了。

一个比较朴素的方法就是倒着找, 从$num$的一半开始往前面找。

```java
class Solution {
    public boolean isPerfectSquare(int num) {
        int mid = num / 2;
        for(int i = mid + 1 ; i >= 1; i--) {
            if (num == i * i) {
                return true;
            }
        }
        return false;
    }
}
```

<hr/>

也可以用二分，但是要注意不要使范围爆掉，所以$mid$我们考虑用$long$来装。

```java
class Solution {
    public boolean isPerfectSquare(int num) {
        if ( num < 2) {
            return true;
        }
        int low = 1, high = num;
        while(low <= high) {
            long mid = low + ((high - low) >> 1);
            if (num == mid * mid) {
                return true;
            } else if (num > mid * mid) {
                low = (int)mid + 1;
            } else if (num < mid * mid) {
                high = (int)mid - 1;
            }
        }
        return false;
    }
}
```

不然$mid \times mid$很容易就爆$int$了。

