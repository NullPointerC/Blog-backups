---
title: leetcode-91-解码方法
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-09-11 11:26:29
---

[link](https://leetcode-cn.com/problems/decode-ways/)

<hr/>

tags : $dp$

<hr/>

**题目描述:**

一条包含字母 $A-Z$ 的消息通过以下映射进行了 **编码** ：

```
'A' -> 1
'B' -> 2
...
'Z' -> 26
```

要 解码 已编码的消息，所有数字必须基于上述映射的方法，反向映射回字母（可能有多种方法）。例如，"11106" 可以映射为：

- $"AAJF"$ ，将消息分组为 (1 1 10 6)
- $"KJF" $，将消息分组为 (11 10 6)

注意，消息不能分组为  (1 11 06) ，因为 "06" 不能映射为 $"F"$ ，这是由于 "6" 和 "06" 在映射中并不等价。

给你一个只含数字的 非空 字符串 s ，请计算并返回 解码 方法的 总数 。

题目数据保证答案肯定是一个 32 位 的整数。

<hr/>

**思路:**

对于给定的字符串$str$，设其长度为$n$，其中字符从左到右分别为$s[1],s[2]...s[n]$。

我们设$f_i$为字符串$str$前$i$个字符$s[1,i]$的解码方法数。并在状态转移时，考虑和之前的字符结合解码：

- 使用一个字符，即$s[i]$进行解码，只要$s[i] \ne 0$，$s[i]$就可以被解码，由于已知前$i-1$个字符的解码方法数为$f_{i-1}$，故$f_i=f_{i-1}$。
- 使用$s[i]和s[i-1]$进行编码。此时需要$s[i-1]$不为0，且$s[i-1]+s[i]$组成的数$≤26$即可。故$f_i = f_{i-2}$

```java
class Solution {
    public int numDecodings(String s) {
        int n = s.length();
        int[] f = new int[n + 1];
        f[0] = 1;
        for (int i = 1; i <= n; ++i) {
            if (s.charAt(i - 1) != '0') {
                f[i] += f[i - 1];
            }
            if (i > 1 && s.charAt(i - 2) != '0' && ((s.charAt(i - 2) - '0') * 10 + (s.charAt(i - 1) - '0') <= 26)) {
                f[i] += f[i - 2];
            }
        }
        return f[n];
    }
}
```

也可以发现$f_i$的值只和前两个状态有关，故可以使用常数空间。

```java
class Solution {
    public int numDecodings(String s) {
        int n = s.length();
        // a = f[i-2], b = f[i-1], c=f[i]
        int a = 0, b = 1, c = 0;
        for (int i = 1; i <= n; ++i) {
            c = 0;
            if (s.charAt(i - 1) != '0') {
                c += b;
            }
            if (i > 1 && s.charAt(i - 2) != '0' && ((s.charAt(i - 2) - '0') * 10 + (s.charAt(i - 1) - '0') <= 26)) {
                c += a;
            }
            a = b;
            b = c;
        }
        return c;
    }
}
```

