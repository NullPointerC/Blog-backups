---
title: leetcode-344-反转字符串
date: 2021-09-20 14:02:19
categories: LeetCode
tags: [algorithm,Java,LeetCode]

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

