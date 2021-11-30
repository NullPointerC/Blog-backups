---
title: leetcode-231-2的幂
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-06-19 22:14:11
---

<a href="https://leetcode-cn.com/problems/power-of-two/">传送门</a>

<hr/>

![image-20210619221458730](https://gitee.com/cao_ziqiang/img/raw/master/20210619221505.png)

题目描述:

<pre>
    给你一个整数 n，请你判断该整数是否是 2 的幂次方。如果是，返回 true ；否则，返回 false 。
    如果存在一个整数 x 使得 n == 2x ，则认为 n 是 2 的幂次方。


​    示例1:

<pre>
    输入：n = 1
	输出：true
	解释：20 = 1
</pre>


​	示例2:

<pre>
    输入：n = 16
	输出：true
	解释：24 = 16
</pre>


​	示例3:

<pre>
    输入：n = 3
	输出：false
</pre>


​	示例4:

<pre>
    输入：n = 4
	输出：true
</pre>


​	示例5:

<pre>
    输入：n = 5
	输出：false
</pre>




<hr>


思路:

若一个整数要为2的幂,那么其必然是大于0的,所以所有小于0的直接return false;

再看大于0的情况,不断的取2模2,若是2的幂,则最后返回1,判断是否和1相等

```java
class Solution {
    public boolean isPowerOfTwo(int n) {
        if(n <= 0 ) {
            return false;
        }
        while(n % 2 == 0) {
            n /= 2;
        }
        return n == 1;
    }
}
```

![image-20210619222112301](https://gitee.com/cao_ziqiang/img/raw/master/20210620205816.png)

<hr/>

plus:2021-11-12

今天忽然好想开窍了,想通了之前一直没有看懂的位运算。😂

首先是：若$n$为2的幂，则$n \& (n-1)$，通过这样的方式可以将$n$二进制中的最低位1移除。

若用$(a10...0)_2$来表示$n$的二进制形式，$a$用以表示若干位高位。$n-1$将表示为：$(a01...1)_2$

二者想与，将消去$a$后面的一系列序列，就只得到了$a00...000_2$，这样就可以将最低位的那个1移除。

再接着是$n\& -n$，通过这样的方式可以直接获取到$n$的二进制表示的最低位的1。

由于负数在计算机中是按照补码的规则存储的，所以$-n$的二进制表示即为$n$的二进制表示的每一位取反后再加上1。

$n \Rightarrow (a10...0)_2$

$-n \Rightarrow (\overline {a}01...1)_2+(1)_2\Rightarrow(\overline{a}10...0)_2$

故二者想与，高位和低位都消去了，这样就获得了最低位的1。

```java
class Solution {
    public boolean isPowerOfTwo(int n) {
        return n > 0 && (n & (n - 1)) == 0; 
    }
}
```

或

```java
class Solution {
    public boolean isPowerOfTwo(int n) {
        return n > 0 && (n & -n) == n;
    }
}
```

