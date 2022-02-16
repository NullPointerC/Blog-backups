---
title: leetcode-120-三角形最小路径和
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: d33121a1
date: 2021-09-22 20:16:35
---

[$link$](https://leetcode-cn.com/problems/triangle/)

<hr/>

![image-20210922201738637](https://gitee.com/cao_ziqiang/img/raw/master/20210922201738.png)

<hr/>

和两道杨辉三角一样的思路，这题只需要用$dp[i]$记录下每一层的最大值,然后从底层再回到上层。

```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int depth = triangle.size();
        if(depth == 0) {
            return 0;
        }
        int ans = Integer.MAX_VALUE;
        int[] dp = new int[depth + 1];
        for (int i = triangle.size() - 1; i >= 0; i--) {
            List<Integer> curTr = triangle.get(i);
            for (int j = 0; j < curTr.size(); j++) {
                //这里的dp[j] 使用的时候默认是上一层的，赋值之后变成当前层
                dp[j] = Math.min(dp[j],dp[j+1]) + curTr.get(j);
            }
        }
        return dp[0];
    }
}
```

