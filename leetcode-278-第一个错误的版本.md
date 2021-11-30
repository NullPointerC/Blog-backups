---
title: leetcode-278-第一个错误的版本
date: 2021-10-31 19:18:30
categories: [LeetCode]
tags: [LeetCode,algorithm]
---

[$link$](https://leetcode-cn.com/problems/first-bad-version/)

<hr/>

![image-20211031191923284](https://gitee.com/cao_ziqiang/img/raw/master/20211031191923.png)

<hr/>

比较经典的$lowBoundSearch$。

分析出搜索边界为$[low,high)$，找到它的$lowBound$。

```java
/* The isBadVersion API is defined in the parent class VersionControl.
      boolean isBadVersion(int version); */

public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        int low = 1, high = n;
        while(low < high) {
            int mid = low + ((high - low) >> 1);
            if(isBadVersion(mid)) {
                high = mid;
            }else{
                low = mid + 1;
            }
        }
        return low;
    }
}
```

