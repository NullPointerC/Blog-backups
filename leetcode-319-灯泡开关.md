---
title: leetcode-319-灯泡开关
date: 2021-11-15 10:18:02
categories: LeetCode
tags: [LeetCode,algorithm]
---

[$link$](https://leetcode-cn.com/problems/bulb-switcher/)

<hr/>

tags: (数论)

![image-20211115101855283](https://gitee.com/cao_ziqiang/img/raw/master/20211115101908.png)

<hr/>

![img](https://gitee.com/cao_ziqiang/img/raw/master/20211115101915.jpeg)

<hr/>

![image-20211115101930517](https://gitee.com/cao_ziqiang/img/raw/master/20211115101930.png)

<hr/>

我们将灯泡分别按顺序编号为$[1,2,3,4...n-1,n]$

对于每个灯泡,它的初始值都是暗,记为0。

进行$n$轮实验，对于每一轮$i$，对于$n$个灯泡，每间隔$i$个就调整它的关闭或打开。

那么可以考虑，进行每一轮时，再遍历一趟数组，对于$j\%i=0 $这样的位置，就是间隔$i$的位置，此时调整它的亮灭。

```java
class Solution {
    public int bulbSwitch(int n) {
        int[] bulbs = new int[n + 1];
        for(int i = 1; i <= n; ++i) {
            for(int j = 1; j <= n; ++j) {
                if (j % i == 0) {
                    bulbs[j] ^= 1;
                }
            }
        }
        int ans = 0;
        for(int bulb : bulbs) {
            if (bulb == 1) {
                ans++;
            }
        }
        return ans;
    }
}
```

但是，很不幸，这样的思路是正确的，但是时间复杂度比较大$O(n^2)$，测试用例比较大时过不掉。

![image-20211115103458284](https://gitee.com/cao_ziqiang/img/raw/master/20211115103458.png)

于是考虑每个灯泡的调整次数，若调整次数为偶数次，那么最终灯泡还是灭的，若调整次数为奇数次，那么最终灯泡时亮的。

于是考虑每个灯泡$bulb_i$的调整次数，每个灯泡的调整次数和它的约数的个数相同，若有$m$个约数，就将调整$m$次亮灭。

对于每个完全平方数$num$，只可以它的一个约数$j$，即$j\times j=num$，这样的数只有奇数个因子，那么编号$bulb_{num}$的灯泡一定为亮。

若对于非完全平方数$num$，若它存在一个约数$j$，那么同理存在另一个约数为$\frac{num}{j}$，即有偶数个因子，最终灯泡还是为熄灭状态。

故我们可以转换为统计区间$[1,n]$内有多少个数有奇数个因子的，也就是求这个区间内有多少个完全平方数，即$floor(sqrt(n))$。

```go
func bulbSwitch(n int) int {
    return int(math.Sqrt(float64(n)))
}
```

或使用牛顿迭代法求$sqrt$函数。

```go
func buldSwitch(n int) int {
    ans := 0
    for i := 0; i <= n; {
        ans++
    }
    return ans
}
```

