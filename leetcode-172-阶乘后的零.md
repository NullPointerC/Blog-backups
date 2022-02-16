---
title: leetcode-172-阶乘后的零
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: b41436fe
date: 2021-07-16 21:41:57
---

[Link](https://leetcode-cn.com/problems/factorial-trailing-zeroes/)

<hr/>

## 题目描述:

<PRE>
给定一个整数 n，返回 n! 结果尾数中零的数量。
</pre>

![image-20210717082524926](https://gitee.com/cao_ziqiang/img/raw/master/20210717082524.png)

## 示例1：

<pre>
输入: 3
输出: 0
解释: 3! = 6, 尾数中没有零。
</pre>

## 示例2：

<pre>
输入: 5
输出: 1
解释: 5! = 120, 尾数中有 1 个零.
</pre>

## 思路：

<pre>
最简单也是最容易想到的解法就是求出n的阶乘,然后将其转为字符数组遍历。
但是这样的遍历方式显然对于大数来说太慢了,即使使用BigInteger也会超时。
就需要转变思路:按照数学题来考虑这个。
首先得知道有 2 * 5 这个因式才能得出末尾有 0 的数。
就可以把问题转换成：题目给定的 n！ 中有几个 2 * 5 末尾就有几个 0，而 2 是任何偶数都有的因子，所以只要考虑 n！ 中一共有多少个 5 就可以了
首先是 5 的倍数，它们就包含至少一个因子 5，比如 5、10、15、20，这几个都是 5 的倍数且只包含一个因子 5
除了 5、10、15、20，比如 25 = 5 * 5，包含两个因子 5，所以 25 的倍数 50、75 这些都包含了两个因子 5
还有一个特殊点的数 125 = 5 * 5 * 5，包含了三个因子 5
</pre>

## 代码：

```java
class Solution {
    public int trailingZeroes(int n) {
        int res = 0;
        // 每一次循环都整除 5，这样就能满足 5、25、125
        for (int i = n; i / 5 > 0; i = i / 5) {
            // 得到每一轮因子 5 的个数
            res += i / 5;
        }
        return res;
    }
}
```

![image-20210717082451954](https://gitee.com/cao_ziqiang/img/raw/master/20210717082452.png)

