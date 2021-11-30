---
title: leetcode-495-提莫攻击
date: 2021-11-10 14:51:40
categories: LeetCode
tags: [LeetCode,algorithm]
---

[$link$](https://leetcode-cn.com/problems/teemo-attacking/)

<hr/>

![image-20211110145233029](https://gitee.com/cao_ziqiang/img/raw/master/20211110145233.png)

<hr/>

比较容易想到思路,当相邻元素的差值在$duration$以内时,说明这个持续的时间没有过,又中毒了。所以重置计数器，并且加上这段差值，否则加上这段间隔。

```go
func findPoisonedDuration(timeSeries []int, duration int) int {
    ans := 0
    for i := 0; i < len(timeSeries) - 1; i += 1 {
        if timeSeries[i] + duration < timeSeries[i + 1] {
            ans += duration     
        } else {
            ans += timeSeries[i + 1] - timeSeries[i]
        }
    }
    return ans + duration
}
```

