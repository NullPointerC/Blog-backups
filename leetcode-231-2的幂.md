---
title: leetcode-231-2的幂
date: 2021-06-19 22:14:11
categories: LeetCode
tags: [algorithm,Java,LeetCode]
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

