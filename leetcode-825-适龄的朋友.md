---
title: leetcode-825-适龄的朋友
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: cef75914
date: 2021-12-27 10:50:03
---

[825. 适龄的朋友](https://leetcode-cn.com/problems/friends-of-appropriate-ages/)

![image-20211227105040324](https://gitee.com/cao_ziqiang/img/raw/master/20211227105040.png)

tags(排序、双指针)

根据题意可知，条件2是包含了条件3的，因此对于每一个x，满足条件的y就是：

$age[y] > 0.5 \times age[x] + 7$

$age[y] <= age[x]$

即$0.5 \times age[x] + 7 \lt age[y] \le age[x]$

可以求得$age[x] \gt 14$，所以可以从15开始计数。

再对$ages[]$排序，遍历数组，因为$x$是递增的，故$y$也是递增的，所以可以固定左指针$l$，确定满足条件的最小$y$的位置，右指针确定第一个不满足条件$y$的位置，所以每一段区间的长度就为$r-l$。

```java
class Solution {
    public int numFriendRequests(int[] ages) {
        Arrays.sort(ages);
        int l = 0, r = 0, ans = 0, n = ages.length;
        for(int i = 0; i < n; i++) {
            if(ages[i] <= 14) {
                continue;
            }
            while(ages[l] <= 0.5 * ages[i] + 7) {
                l++;
            }
            while(r + 1 < n && ages[r + 1] <= ages[i]) {
                r++;
            }
            ans += (r - l);
        }
        return ans;
    }
}
```

