---
title: leetcode-2055-蜡烛之间的盘子
date: 2022-03-08 22:58:14
categories: LeetCode
tags: [LeetCode,algorithm]
---

[2055. 蜡烛之间的盘子](https://leetcode-cn.com/problems/plates-between-candles/)

![image-20220308225853367](https://gitee.com/cao_ziqiang/img/raw/master/20220308225853.png)

<hr/>

![image-20220308225956515](https://gitee.com/cao_ziqiang/img/raw/master/20220308225956.png)

<hr/>

![image-20220308230011978](https://gitee.com/cao_ziqiang/img/raw/master/20220308230012.png)

<hr/>

题目的意思其实比较简单，对于每个查询query，需要找到这个查询区间里，`l,r`之间被蜡烛`|`包住的最大盘子数量`*`。

可以预处理`s`，先找到距离每个盘子最近的蜡烛在哪里？用`left`数组表示每个盘子距离最近的左边蜡烛在哪里，于是遍历一趟`s`，如果`s[i]='|'`则更新`left[i]=i`，否则和上一次出现的最近蜡烛相同`left[i]=left[i-1]`。

用`right`数组表示每个盘子距离最近的右边蜡烛在哪里，从反向遍历`s`，如果`s[i]='|'`,就更新`right[i]=i`，否则和上一次最右边的蜡烛位置一样也就是`right[i]=right[i+1]`。

再需要对整个字符串`s`预处理，求出字符串`s`的前缀盘子和`preSum`。

求前缀盘子的过程也就是从正向遍历，如果`s[i]='*'`，那么就更新`preSum[i]=preSum[i-1]+1`，如果遇到蜡烛则保持不变。

接下里处理每个查询，`l,r`区间内，这时候需要取左端点的距离最近的蜡烛`right[l]`作为盘子区间的左端点，和取距离右端点的距离最近的左边蜡烛`left[r]`作为盘子区间的右端点。

如果他们满足盘子区间的左端点比查询区间的右端点覆盖或超过或是盘子区间的右端点比查询区间的左端点更小，就返回0，因为没有盘子，否则返回这段区间的差值。

```java
class Solution {
    public int[] platesBetweenCandles(String s, int[][] queries) {
        if(s == null || s.length() == 0) {
            return new int[0];
        }
        int n = s.length();
        char[] chars = s.toCharArray();
        int[] left = new int[n];
        int[] right = new int[n];
        Arrays.fill(left, -1);
        if(chars[0] == '|') {
            left[0] = 0;
        }
        for(int i = 1; i < n; i++) {
            if(chars[i] == '|') {
                left[i] = i;
            } else {
                left[i] = left[i - 1];
            }
        }
        Arrays.fill(right, -1);
        if(chars[n - 1] == '|') {
            right[n - 1] = n - 1;
        }
        for(int i = n - 2; i >= 0; i--) {
            if(chars[i] == '|') {
                right[i] = i;
            } else {
                right[i] = right[i + 1];
            }
        }
        int[] preSum = new int[n + 1];
        preSum[0] = 0;
        for(int i = 0; i < n; i++) {
            if(chars[i] == '*') {
                preSum[i + 1] = preSum[i] + 1;
            } else if(chars[i] == '|') {
                preSum[i + 1] = preSum[i];
            }
        }
        int q = queries.length;
        int[] res = new int[q];
        for(int i = 0; i < q; i++) {
            int l = queries[i][0], r = queries[i][1];
            int lCandle = right[l], rCandle = left[r];
            if(lCandle >= r - 1 || rCandle <= l + 1) {
                res[i] = 0;
            } else {
                res[i] = preSum[rCandle] - preSum[lCandle];
            }
        }
        return res;
    }
}
```

