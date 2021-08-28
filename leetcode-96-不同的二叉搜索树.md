---
title: leetcode-96-不同的二叉搜索树
date: 2021-08-16 13:37:36
categories: LeetCode
tags: [algorithm,Java,LeetCode]
---

[link](https://leetcode-cn.com/problems/unique-binary-search-trees/)

<hr/>

题目描述:

<pre>
给你一个整数 n ，求恰由 n 个节点组成且节点值从 1 到 n 互不相同的 二叉搜索树 有多少种？返回满足题意的二叉搜索树的种数。
</pre>



示例1:

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210816133845.jpeg)

<pre>
输入：n = 3
输出：5
</pre>

示例2:

<pre>
输入：n = 1
输出：1
</pre>

思路:

<pre>
因为这是二叉搜索树,所以排序的顺序是唯一的,只是每个数字做根结点时,左右子树
也有不同的排列方式.
假设n个节点存在二叉排序树的个数是G(n)，令f(i)为以i为根的二叉搜索树的个数
即有:G(n) = f(1) + f(2) + f(3) + f(4) + ... + f(n)
n为根节点，当i为根节点时，其左子树节点个数为[1,2,3,...,i-1]，右子树节
点个数为[i+1,i+2,...n]，所以当i为根节点时，其左子树节点个数为i-1个，右
子树节点为n-i，即f(i) = G(i-1)*G(n-i),
上面两式可得:G(n) = G(0)*G(n-1)+G(1)*(n-2)+...+G(n-1)*G(0)
</pre>

代码:

```java
class Solution {
    public int numTrees(int n) {
        int[] dp = new int[n + 1];
        dp[0] = 1;
        dp[1] = 1;
        for(int i = 2;i<=n;i++) {
            for(int j = 1;j<=i;j++) {
                // 状态转移方程 dp[i] = dp[0] * dp[n - 1] + dp[1] * dp[n - 2] + ... + dp[n - 1] * dp[0]
                dp[i] += dp[j - 1] * dp[i-j];
            }
        }
        return dp[n];
    }
}
```

![image-20210816134052327](https://gitee.com/cao_ziqiang/img/raw/master/20210816134052.png)

