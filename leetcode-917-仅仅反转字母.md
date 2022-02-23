---
title: leetcode-917-仅仅反转字母
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: a39b8ae4
date: 2022-02-23 10:49:38
---

[917. 仅仅反转字母](https://leetcode-cn.com/problems/reverse-only-letters/)

![image-20220223105022113](https://gitee.com/cao_ziqiang/img/raw/master/20220223105022.png)

比较典型的双指针类题型，可以设置两个指针`st`和`ed`，分别从字符串的头尾开使遍历，当首尾指针都指向字母时，交换两个指针的字母，直到两指针相遇。

```java
class Solution {
    public String reverseOnlyLetters(String s) {
        char[] str = s.toCharArray();
        int st = 0, ed = str.length - 1;
        while (st < ed) {
            while(st < str.length && !(str[st] >= 'A' && str[st] <= 'Z') && !(str[st] >= 'a' && str[st] <= 'z')) st++;
            while(ed >=0 && !(str[ed] >= 'A' && str[ed] <= 'Z') && !(str[ed] >= 'a' && str[ed] <= 'z')) ed--;
            if(st < ed) {
                char tmp = str[st];
                str[st] = str[ed];
                str[ed] = tmp;
                st++;
                ed--;
            }
        }
        return String.valueOf(str);
    }
}
```

时间复杂度$O(n)$,空间复杂度$O(1)$。

