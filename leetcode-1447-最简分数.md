---
title: leetcode-1447-最简分数
date: 2022-02-10 00:13:11
categories: LeetCode
tags: [LeetCode,algorithm]
---

[1447. 最简分数](https://leetcode-cn.com/problems/simplified-fractions/)

![image-20220210001342086](https://gitee.com/cao_ziqiang/img/raw/master/20220210001342.png)

我们枚举所有$2$-$n$之间的所有数，先以它们做分母，再枚举所有$1$-$n - 1$的所有数作为分子，如果分子分母的最小公因数为1，说明最简，否则不是最简。

```java
class Solution {
    public List<String> simplifiedFractions(int n) {
        List<String> res = new ArrayList<>();
        for(int i = 2; i <= n; i++) {
            for(int j = 1; j < i; j++) {
                // i和j不能再约分了
                if(gcd(i, j) == 1) {
                    String tmp = j + "/" + i;
                    res.add(tmp);
                }
            }
        }
        return res;
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

