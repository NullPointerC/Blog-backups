---
title: leetcode-583-两个字符串的删除操作
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: ce18fe87
date: 2021-09-25 09:29:49
---

[$link$](https://leetcode-cn.com/problems/delete-operation-for-two-strings/)

![image-20210925093025450](https://gitee.com/cao_ziqiang/img/raw/master/20210925093025.png)

<hr/>

还是对动态规划不太熟练。

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210925093139.gif)

<hr/>

这道题目可以和**「编辑距离」**类似的思路。对于两个字符串的CRUD问题，一般都需要借助双指针和动态规划。

在这里我们假设$dp[i][j]$表示为字符串$word1[0...i -1]$到字符串$word2[0...j -1]$所需要的最小删除次数。

对于base case，情况应该如下：

字符串$word2$为空，则$word1$要变为$word2$，就需要删除$word1$本身长度这么多个字母，对于$word1$为空也是同理。

$dp[i][0] = i$&&$dp[0][j]=j$

对于这个变化过程中的状态转移：

如果$word1[i -1]==word2[j - 1]$：

$dp[i][j]=dp[i-1][j-1]$

如果不等，则需要考虑是删除哪个单词中的最后一个字母：

如果删除$word1$中的最后一个字母，即删除$word[i-1]$，那么$dp[i][j]=dp[i-1][j]+1$，对于删除$word2$也是同理，即$dp[i][j] = dp[i][j-1]+1$。如果考虑两个都删除，那么$dp[i][j] = dp[i-1][j-1]+1$。

最后需要知道的是整个$word1$变为$word2$的次数，所以需要知道$word1[0...word1.length()]$到$word2[0...word2.length()]$。所以返回$dp[l1][l2]$。

```java
class Solution {
    public int minDistance(String word1, String word2) {
        int l1 = word1.length();
        int l2 = word2.length();
        // dp[i][j]表示word1[0...i-1]到word2[0...j-1]所需要的次数
        int[][] dp = new int[l1 + 1][l2 + 1];
        // 初始化
        for(int i = 0; i <= l1; i++) {
            dp[i][0] = i;
        }
        for(int j = 0; j <= l2; j++) {
            dp[0][j] = j;
        }
        for(int i = 1; i <= l1; i++) {
            for(int j = 1; j <= l2 ; j++) {
                if(word1.charAt(i - 1) == word2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1];
                }else{
                    dp[i][j] = Math.min(dp[i - 1][j - 1] + 2,
                                        Math.min(dp[i - 1][j] + 1, dp[i][j - 1] + 1));
                }
            }
        }
        return dp[l1][l2];
    }
}
```

