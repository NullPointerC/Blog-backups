---
title: leetcode-397-整数替换
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: '3e706957'
date: 2021-11-19 10:02:45
---

[$link$](https://leetcode-cn.com/problems/integer-replacement/)

<hr/>

tags(dfs、记忆化搜索、贪心)

![image-20211119100408670](https://gitee.com/cao_ziqiang/img/raw/master/20211119100408.png)

<hr/>

比较容易想到的是记忆化搜索, 但是最后卡了一个$2^{31}-1$。所以转用long作为$map$的$K-V$。

```java
class Solution {
    Map<Long, Long> map = new HashMap<>();
    public int integerReplacement(int n) {
        return (int)dfs((long)n);
    }
    private long dfs(long n) {
        if (map.containsKey(n)) {
            return map.get(n);
        }
        if (n == 1) {
            return 0;
        }
        if (n % 2 == 0) {
            map.put(n, dfs(n / 2) + 1);
        } else {
            map.put(n, Math.min(dfs(n + 1), dfs(n - 1)) + 1);
        }
        return map.get(n);
    }
}
```

Golang：

这里做了一些小优化，因为奇数+1或-1都是为了转为偶数再除以2，而加1后除以2，可以简化为$n/2 + 1$，而减1之后再除以2，可以直接简化为$n/2$。

```go
var cache map[int]int

func dfs(n int) int{
    if(n == 1){
        return 0
    }
    v := cache[n]
    if v > 0 {
        return v
    }
    ans := 0
    if n % 2 == 0 {
        ans = dfs(n / 2) + 1
    } else {
        ans = min(dfs(n / 2), dfs(n / 2 + 1)) + 2
    }
    cache[n] = ans
    return ans
}

func integerReplacement(n int) int {
    cache = map[int]int{}
    return dfs(n)
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

也可以直接找规律，贪心。

当$n$为偶数时，只有一种唯一的方法，即$n \Rightarrow n /2$。

当$n$为奇数时，它可能有两种情况，一种是$n \% 4=1$或者$n \% 4=3$。

如果余数为1，可以断定要将$n$变为$\frac {n-1}{2}$。可以比较将$n$变为两种不同情况的次数。

如果变为了$\frac{n+1}{2}$，那么这个数就为奇数：

如果下一步$-1$再除以$2$，得到了$\frac{n-1}{4}$，由$n$变到这一步需要经过3步，而假如变为$\frac{n-1}{2}$再到这一步就经过了2步，可见此时$\frac{n-1}{2}$更优；

如果下一步$+1$再除以2，得到了$\frac{n+3}{4}$，由$n$变到这一步需要经过$3$步，而由$\frac{n-1}{2}$到这一步就需要经过除以$2$再$+1$，也是经过$3$步，可见综合而言，$n\%4=1$时，把$n$转为$\frac{n-1}{2}$可以得到更优解。

同理$n\%4=3$时，可以断定要将$n$变为$\frac{n+1}{2}$。

若变为了$\frac{n-1}{2}$，该式为奇数，下一步进行$-1$再除以$2$，得到$\frac{n-3}{4}$，经过了$3$步，由$\frac{n+1}{2}$变到这一步可以除以$2$再-1，也是经过$3$步；

若下一步$+1$再除以$2$，得到了$\frac{n+1}{4}$，经过了3步，而由$\frac{n+1}{2}$变到这一步就需要2步，可见$\frac{n+1}{2}$更优。

```java
class Solution {
    public int integerReplacement(int n) {
        int ans = 0;
        while (n != 1) {
            if (n % 2 == 0) {
                ++ans;
                n /= 2;
            } else if (n % 4 == 1) {
                ans += 2;
                n /= 2;
            } else {
                if (n == 3) {
                    ans += 2;
                    n = 1;
                } else {
                    ans += 2;
                    n = n / 2 + 1;
                }
            }
        }
        return ans;
    }
}
```

