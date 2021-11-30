---
title: leetcode-344-反转字符串
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-09-20 14:02:19
---

[$link$](https://leetcode-cn.com/problems/reverse-string/)

<hr/>

![image-20210920140259465](https://gitee.com/cao_ziqiang/img/raw/master/20210920140259.png)

<hr/>

设置首尾指针$front$和$back$即可，每次交换两个指针的值，直到两个指针相遇。

```java
class Solution {
    public void reverseString(char[] s) {
        int l = s.length;
        if(l == 0) {
            return;
        }
        int front = 0;
        int back = l - 1;
        while(front < back) {
            char temp = s[front];
            s[front] = s[back];
            s[back] = temp;
            front++;
            back--;
        }
    }
}
```

<hr/>

plus:2021-11-3

利用新学的位运算：

```java
class Solution {
    public void reverseString(char[] s) {
        int n = s.length;
        for (int i = 0; i < n / 2; ++i) {
            int j = n - 1 - i;
            s[i] ^= s[j];
            s[j] ^= s[i];
            s[i] ^= s[j];
        }
    }
}
```

第一步的时候$s[i]=s[i] \text {^} s[j]$

第二步的时候$s[j] = s[j] \text{^} s[i] \text{^}s[j] \Rightarrow s[i] $

最后再$s[i] = s[i] \text{^} s[j] \text{^} s[j] \Rightarrow s[i]$