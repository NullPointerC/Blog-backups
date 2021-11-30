---
title: leetcode-434-字符串中的单词数
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-10-07 12:03:28
---

[$link$](https://leetcode-cn.com/problems/number-of-segments-in-a-string/)

<hr/>

![image-20211007120754236](https://gitee.com/cao_ziqiang/img/raw/master/20211007120754.png)

<hr/>

题目很短,却没有那么简单.

有一个测试用例太坑了

![image-20211007120828367](https://gitee.com/cao_ziqiang/img/raw/master/20211007120828.png)

<hr/>

因为题目对「单词」的定义为[连续的不是空格的字符]

因为我们可以考虑在字符串的末尾加上一个空格,这就可以统一处理各种情况,可以将空串，空格结尾，字符结尾三种状况归一考虑，减少额外的判断

```java
class Solution {
    public int countSegments(String s) {
        int count = 0;
        String temp = s + " ";
        for (int i = 1; i < temp.length(); i++) {
            if (temp.charAt(i) == ' ' && temp.charAt(i - 1) != ' ') {
                count++;
            }
        }
        return count;
    }
}
```

