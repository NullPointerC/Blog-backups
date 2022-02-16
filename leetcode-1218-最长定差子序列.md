---
title: leetcode-1218-最长定差子序列
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: 448f67e3
date: 2021-11-05 00:19:04
---

[$link$](https://leetcode-cn.com/problems/longest-arithmetic-subsequence-of-given-difference/)

<hr/>

![image-20211105002142494](https://gitee.com/cao_ziqiang/img/raw/master/20211105002142.png)

<hr/>

看到最长xxx之类的就应该往动态规划上想,涉及的最长结构为在每一次之前的差值序列长度后+1。

所以定义一个$map$意为$num -> maxlen$。初始应该为$arr[i] -> 1$，在这题中不用初始化也是可以的。

在循环一趟$arr$，如果存在当前$num - difference$在数组中，则更新当前$num$的长度，并比较与所求的最大长度大小。

```java
class Solution {
    public int longestSubsequence(int[] arr, int difference) {
        int ans = 1;
        Map<Integer, Integer> dp = new HashMap<>();
        for (int num : arr) {
            Integer len = dp.get(num - difference);
            if (len != null) {
                dp.put(num, len + 1);
                ans = Math.max(ans, len + 1);
            } else {
                dp.put(num, 1);
            }
        }
        return ans;
    }
}
```

在补一个Go的

```go
func longestSubsequence(arr []int, difference int) int {
    ans := 1
    dp := make(map[int]int)
    for _, num := range arr {
        v, ok := dp[num - difference]
        if ok {
            dp[num] = v + 1
            if v + 1 > ans {
                ans = v + 1
            }
        } else {
            dp[num] = 1
        }
    }
    return ans;
}
```

