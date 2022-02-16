---
title: leetcode-1716-计算力扣银行的钱
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: 1c75e7a4
date: 2022-01-15 12:27:04
---

[1716. 计算力扣银行的钱](https://leetcode-cn.com/problems/calculate-money-in-leetcode-bank/)

![image-20220115122752796](https://gitee.com/cao_ziqiang/img/raw/master/20220115122752.png)

一个比较简单的方式就是直接打表即可,因为数据范围不大,只有1000以内。再计算每天的前缀和即可。

```java
class Solution {
    private static int[] ans = new int[1010];
    private static int[] sum = new int[1010];
    static {
        ans[1] = 1;
        for(int i = 1; i < 1010; i++) {
            if(i % 7 == 1 && i > 1) {
                ans[i] = ans[i - 7] + 1;
            } else {
                ans[i] = ans[i - 1] + 1;
            }
            sum[i] = sum[i - 1] + ans[i];
        }
    }
    public int totalMoney(int n) {
        return sum[n];
    }
}
```

