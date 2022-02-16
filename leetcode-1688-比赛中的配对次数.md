---
title: leetcode-1688-比赛中的配对次数
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: 72c62573
date: 2022-01-25 14:21:08
---

[1688. 比赛中的配对次数](https://leetcode-cn.com/problems/count-of-matches-in-tournament/)

![image-20220125142154964](https://gitee.com/cao_ziqiang/img/raw/master/20220125142155.png)

一个比较好的办法枚举,观察规律,可以看出,对于当前如果有k支队伍,一共需要进行$k/2$场比赛,接下来一场将有${k\ /\ 2+k\ \%\ 2}$支队伍,如此循环下去,直到只剩下1支队伍,就无需再配对。

```java
class Solution {
    public int numberOfMatches(int n) {
        int ans = 0;
        // 只剩下1支队伍时可以获胜了
        while(n > 1) {
            // 下一场的队伍数量
            int next = n / 2 + n % 2;
            // 这一场需要配对的次数
            ans += n / 2;
            // 更新队伍数
            n = next;
        }
        return ans;
    }
}
```

还有一种取巧的做法，个人认为在面试过程中估计比较难想出来，每次配对就会有一场比赛，一场比赛双方会产生一名胜者，一名败者，败者无法继续，胜者继续，最后只剩下一名胜者，所以将产生$n-1$名败者，所以有$n-1$场比赛，也就是$n-1$场配对。

```java
class Solution {
    public int numberOfMatches(int n) {
        return n - 1;
    }
}
```

