---
title: leetcode-265-粉刷房子Ⅱ
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: 840e902e
date: 2021-09-28 18:58:58
---

[link](https://leetcode-cn.com/problems/paint-house-ii/)

<hr/>

![image-20210928190025163](https://gitee.com/cao_ziqiang/img/raw/master/20210928190025.png)

<hr/>

这题比起上一题来说多了要考虑$k$种颜色,但是还是可以枚举出来。

定义$dp[i][j] \Rightarrow$为第$i$间房子刷$j$号颜色的最小花费。

故$dp[i][j] = min(dp[i-1][m]) + costs[i- 1][j]\\ 1 \le i \lt n,0\le j\lt k,0 \le m \lt k ,k ≠ j$

```java
class Solution {
    public int minCostII(int[][] costs) {
        int n = costs.length;
        if(n == 0) {
            return 0;
        }
        int k = costs[0].length;
        if(n == 1) {
            int ans = Integer.MAX_VALUE;
            for(int i = 0; i < k ;i++) {
                ans = Math.min(ans, costs[0][i]);
            }
            return ans;
        }
        // 第i间房子刷j号颜色的花费
        int[][] dp = new int[n + 1][k];
        // 初始化base case
        for(int i = 0; i < k; i++) {
            dp[1][i] = costs[0][i];
        }
        // 状态转移
        for(int i = 2; i <= n; i++) {
            int min = Integer.MAX_VALUE;
            int minIndex = 0;
            for (int j = 0; j < k; j++) {
                if(dp[i - 1][j] < min) {
                    min = dp[i - 1][j];
                    minIndex = j;
                }
            }
            for(int j = 0; j < k; j++) {
                dp[i][j] = (getMin(dp[i - 1], j) + costs[i - 1][j]);
            }
            // for(int j = 0; j < k; j++) {
            //     System.out.print(dp[i][j] + "\t");
            // }
            // System.out.println();
        }
        int ans = Integer.MAX_VALUE;
        for(int i = 0; i < k; i++) {
            if(dp[n][i] < ans) {
                ans = dp[n][i];
            }
        }
        return ans;
    }

    private int getMin(int[] nums, int exceptIndex) {
        int min = Integer.MAX_VALUE;
        int minIndex = 0;
        for(int i = 0; i < nums.length; i++) {
            if(nums[i] < min && i != exceptIndex) {
                min = nums[i];
            }
        }
        return min;
    }
}
```

时间复杂度$O(nkk)$，空间复杂度$O(nk)$。

进阶：

考虑要得到第$i$间房上$j$色的最低价格，那么如果房间$i-1$不是上$j$色，则直接$min + costs[i][j]$即可，如果房间$i-1$上$j$色，则房间$i$上$j$色的最低价格是$secondMin+costs[i][j]$。

```java
class Solution {
    public int minCostII(int[][] costs) {
        int n = costs.length;
        if(n == 0) {
            return 0;
        }
        int k = costs[0].length;
        int min = 0, secondMin = 0, colorIndex = -1;
        for(int i = 0; i < n ;i++) {
            int preColorIndex = colorIndex;
            int preSecondMin = secondMin;
            int preMin = min;
            secondMin = Integer.MAX_VALUE;
            min = Integer.MAX_VALUE;
            secondMin = Integer.MAX_VALUE;
            for(int j = 0; j < k; j++) {
                int price = 0;
                if(j == preColorIndex) {
                    price = preSecondMin + costs[i][j];
                }else{
                    price = preMin + costs[i][j];
                }
                if(price < secondMin) {
                    if(price < min) {
                        secondMin = min;
                        min = price;
                        colorIndex = j;
                    }else{
                        secondMin = price;
                    }
                }
            }
        }
        return min;
    }
}
```

