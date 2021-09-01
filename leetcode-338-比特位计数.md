---
title: leetcode-338-比特位计数
date: 2021-08-31 15:04:25
categories: LeetCode
tags: [algorithm,Java,LeetCode]
---

[link](https://leetcode-cn.com/problems/counting-bits/)

<hr/>

**题目描述:**

<pre>
给定一个非负整数 num。对于 0 ≤ i ≤ num 范围中的每个数字 i ，计算其二进制数
中的 1 的数目并将它们作为数组返回。
</pre>

**示例1:**

<pre>
输入: 2
输出: [0,1,1]
</pre>

**示例2:**

<pre>
输入: 5
输出: [0,1,1,2,1,2]
</pre>

**思路:**

<pre>
最简单的办法就是一趟遍历,把当前遍历到的数组转为二进制并统计它含有1个个数;
时间复杂度为O(nm),n为数组长,m为数字的二进制位长度;
</pre>

**代码:**

```java
class Solution {
    public int[] countBits(int n) {
        if (n < 0) {
            return null;
        }
        int[] dp = new int[n + 1];
        dp[0] = 0;
        for (int i = 1; i <= n; i++) {
            String s = numToStr(i);
            int count = 0;
            for (int j = 0; j < s.length(); j++) {
                if (s.charAt(j) == '1') {
                    count++;
                }
            }
            dp[i] = count;
        }
        return dp;
    }

    private String numToStr(int n) {
        StringBuilder sb = new StringBuilder();
        while (n != 0) {
            sb.append(n % 2);
            n /= 2;
        }
        return sb.reverse().toString();
    }
}
```

<pre>
后来列规律发现了,假如使用dp[i]表示到数字i的二进制位1的个数;
如果i为2的幂,那么dp[i]就直接等于1;
如果i不为2的幂,那么dp[i]可以等于距离他最近的2的幂+dp[i-2^x]
时间复杂度为O(nlgn),判断一个数是否为二进制需要lgn,遍历n次;
</pre>

```java
class Solution {
    public int[] countBits(int n) {
        if (n < 0) {
            return null;
        }
        int[] dp = new int[n + 1];
        dp[0] = 0;
        int start = 1;
        for (int i = 1; i <= n; i++) {
            if (isPowerOfTwo(i)) {
                start = i;
                dp[start] = 1;
            } else {
                dp[i] = dp[start] + dp[i - start];
            }
        }
        return dp;
    }

    private boolean isPowerOfTwo(int n) {
        if (n <= 0) {
            return false;
        }
        while (n % 2 == 0) {
            n /= 2;
        }
        return n == 1;
    }
}
```

<pre>
交完之后发现还不是最优解...难道还有更好的解法?
那这种解法肯定是O(n)复杂度以内的,所以只能进行一趟遍历;
然而还是没有思路...我是fw我承认了...
----------------------------------------------
看到大佬的题解才恍然大悟,还可以使用位运算,比如对n>>1,右移1位
如果n的末尾是0,那么它二进制位中1的个数和n>>1一样;
如果n的末尾是1,那么它二进制位中1的各种和n>>1相比还需要加上最后一位的1;
这个n末尾是否为1怎么判断呢?这时可以让n和1进行与运算,得到的结果就可以代表;
</pre>

代码:

```java
class Solution {
    public int[] countBits(int num) {
        int[] res = new int[num + 1];
        for(int i = 0;i<= num;i++){
            res[i] = res[i >> 1] + (i & 1);  //注意i&1需要加括号
        }
        return res;
    }
}
```

![image-20210831155523473](https://gitee.com/cao_ziqiang/img/raw/master/20210831155523.png)

