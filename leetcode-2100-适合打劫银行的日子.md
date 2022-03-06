---
title: leetcode-2100-适合打劫银行的日子
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: abaa76d5
date: 2022-03-06 12:00:42
---

[2100. 适合打劫银行的日子](https://leetcode-cn.com/problems/find-good-days-to-rob-the-bank/)

![image-20220306120118740](https://gitee.com/cao_ziqiang/img/raw/master/20220306120118.png)

可以根据题意知道适合打劫的日子满足$sec[i-1] \ge sec[i] \le sec[i+1]$。

且对于$i$，前有$time$个递减的数量，后有$time$个递增的数量。

于是分别以$f[i]$和$g[i]$表示$i$天前后各自有多少个相应的递增和递减个数。

再求出所有的$f[i] \ge time$且$g[i] \ge time$的位置即可。

```java
class Solution {
    public List<Integer> goodDaysToRobBank(int[] security, int time) {
        int n = security.length;
        // 表示第i天前面有连续f[i]的递减
        int[] f = new int [n];
        // 表示第i天后面有连续g[i]的递增
        int[] g = new int [n];
        for(int i = 0; i < n; i++) {
            if(i > 0 && security[i] <= security[i - 1]) {
                f[i] = f[i - 1] + 1;
            } else {
                f[i] = 0;
            }
        }
        for(int i = n - 1; i >= 0; i--) {
            if(i < n - 1 && security[i] <= security[i + 1]) {
                g[i] = g[i + 1] + 1;
            } else {
                g[i] = 0;
            }
        }
        List<Integer> res = new ArrayList<>();
        for(int i = 0; i < n; i++) {
            if(f[i] >= time && g[i] >= time) {
                res.add(i);
            }
        }
        return res;
    }
}
```

