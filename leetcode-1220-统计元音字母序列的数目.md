---
title: leetcode-1220-统计元音字母序列的数目
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: 64c8c68a
date: 2022-01-17 21:48:51
---

[1220. 统计元音字母序列的数目](https://leetcode-cn.com/problems/count-vowels-permutation/)

<hr/>

![image-20220117214927395](https://gitee.com/cao_ziqiang/img/raw/master/20220117214927.png)

虽然标注的是困难，但是思路并不难想，我们可以维护上一个元音的数目，对于元音$'a'$，它可以由$'e'$、$'i'$、$'u'$来组成，故而$a = (e + i + u) \% MOD$；

同理，对于元音$'e'$，它可以由$'a'$、$'i'$组成，故而$e=(a + i) \% MOD$；

对于元音$'i'$，它可以由$'e'$、$'o'$组成，故而$i=(e+o)\%MOD$;

对于元音$'o'$，它可以由$'i'$组成，故而$o = i \% MOD$

对于元音$'u'$，它可以由$'i'$、$'o'$组成，故而$u = (i + o)\% MOD$；

更新完之后，记得重新赋值即可。

```java
class Solution {
    private static final int MOD = (int)(1e9 + 7);
    public int countVowelPermutation(int n) {
        long a = 1, e = 1, i = 1, o = 1, u = 1;
        for(int k = 2; k <= n; k++) {
            long currA = (e + i + u) % MOD;
            long currE = (a + i) % MOD;
            long currI = (e + o) % MOD;
            long currO = i % MOD;
            long currU = (i + o) % MOD;
            a = currA;
            e = currE;
            i = currI;
            o = currO;
            u = currU;
        }
        return (int) ((a + e + i + o + u) % MOD);
    }
}
```

