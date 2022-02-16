---
title: leetcode-264-丑数Ⅱ
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: 36d86be4
date: 2021-10-19 21:05:58
---

[$link$](https://leetcode-cn.com/problems/ugly-number-ii/)

![image-20211019210635081](https://gitee.com/cao_ziqiang/img/raw/master/20211019210635.png)

<hr/>

多路归并+动态规划

不难看出，后续的丑数都是基于原有的丑数而来，在原有丑数的基础上，乘上质因数2，3，5即可。

因此，如果我们所有丑数的有序序列为 a1,a2,a3,...,ana1,a2,a3,...,an 的话，序列中的每一个数都必然能够被以下三个序列（中的至少一个）覆盖：

- 由丑数 $\times$ 2 所得的有序序列：$1 \times 2、2\times2、3\times2、4\times2、5\times2、6\times2、8\times2 ...$
- 由丑数 $ \times$ 3 所得的有序序列：$1 \times 3、2\times3、3\times3、4\times3、5\times3、6\times3、8\times3 ...$
- 由丑数 $\times$ 5 所得的有序序列：$1\times5、2 \times 5、3 \times 5、4 \times5、5 \times5、6 \times 5、8 \times 5 ...$

假设我们需要求得 [1, 2, 3, ... , 10, 12][1,2,3,...,10,12] 丑数序列 arrarr 的最后一位，那么该序列可以看作以下三个有序序列归并而来：

- $1 \times 2, 2 \times 2, 3 \times 2, ... , 10 \times 2, 12 \times 2$， 将 2 提出，即 $arr \times 2$
- $1 \times 3, 2 \times 3, 3 \times 3, ... , 10 \times 3, 12 \times 3$ ，将 3 提出， 即$ arr \times 3$
- $1 \times 5, 2 \times 5, 3 \times 5, ... , 10 \times 5, 12 \times 5$ ，将 5 提出， 即 $arr \times 5$

```java
class Solution {
    public int nthUglyNumber(int n) {
        // 丑数数组
        int[] dp = new int[n + 1];
        dp[1] = 1;
        int[] idx = {1, 1, 1};
        for(int i = 2; i <= n; i++) {
            while(dp[idx[0]] * 2 <= dp[i - 1]) {
                idx[0]++;
            }
            while(dp[idx[1]] * 3 <= dp[i - 1]) {
                idx[1]++;
            }
            while(dp[idx[2]] * 5 <= dp[i - 1]) {
                idx[2]++;
            }
            dp[i] = Math.min(dp[idx[0]] * 2, Math.min(dp[idx[1]] * 3, dp[idx[2]] * 5));
        }
        return dp[n];
    }
}
```


