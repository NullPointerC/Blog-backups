---
title: leetcode-441-排列硬币
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: bc8901c3
date: 2021-10-10 18:12:05
---

[$link$](https://leetcode-cn.com/problems/arranging-coins/)

<hr/>

![image-20211010181313307](https://gitee.com/cao_ziqiang/img/raw/master/20211010181313.png)

<hr/>

一个比较容易想到的办法是递增$level$,且不断尝试当前的硬币数$n-level$。当$n \lt 0$时，即说明此时的$level$不满足条件了，返回上一层即可。

```java
class Solution {
    public int arrangeCoins(int n) {
        int level = 0;
        while(true) {
            if(n >= 0) {
                level++;
                n -= level;
            }else{
                break;
            }
        }
        return level - 1;
    }
}
```

也可以考虑这样的思路，$n \ge \frac{x \times (x + 1)}{2}$。

再通分，移项后，$x^2 + x -2n \le 0$，再使用我们熟悉的一元二次方程的求根公式，即

$x\le\frac{-1\pm\sqrt{1+8n}}{2}$，易知，我们显然要取正根，所以$x\le\frac{-1+\sqrt{1+8n}}{2}$。

```java
class Solution {
    public static int arrangeCoins(int n) {
        return (int) (-1 + Math.sqrt(1 + 8 * (long) n)) / 2;
    }
}
```

