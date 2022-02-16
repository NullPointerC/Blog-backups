---
title: leetcode-118-杨辉三角
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: ab7c7359
date: 2021-07-05 12:49:08
---

[link](https://leetcode-cn.com/problems/pascals-triangle/)

<hr/>

![image-20210705125010923](https://gitee.com/cao_ziqiang/img/raw/master/20210705125011.png)

思路:

比较常规的dp,dp初值都可以不用设,因为一开始只有一层,所以不用设

后面的dp状态转移方程为:

dp [i] [j] = dp[i-1] [j-1] + dp[i-1] [j]

代码:

```java
class Solution {
    public List<List<Integer>> generate(int numRows) {
        if(numRows <= 0) {
            return null;
        }
        List<List<Integer>> ret = new LinkedList<>();
        List<Integer> dp = null;
        for(int i = 1; i <= numRows; i++) {
            dp = new LinkedList<>();
            for(int j = 1; j <= i; j++) {
                if(j == 1 || j == i) {
                    dp.add(1);
                }else {
                    List<Integer> list = ret.get(i - 2);
                    dp.add(list.get(j - 2) + list.get(j - 1));
                }
            }
            ret.add(dp);
        }
        return ret;
    }
}
```

<hr/>

![image-20210705125534263](https://gitee.com/cao_ziqiang/img/raw/master/20210705125534.png)

![image-20210705125544706](https://gitee.com/cao_ziqiang/img/raw/master/20210705125544.png)

