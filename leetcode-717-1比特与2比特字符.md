---
title: leetcode-717-1比特与2比特字符
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: 47bd2fb9
date: 2022-02-20 23:30:29
---

[717. 1比特与2比特字符](https://leetcode-cn.com/problems/1-bit-and-2-bit-characters/)

![image-20220220233107944](https://gitee.com/cao_ziqiang/img/raw/master/20220220233108.png)

<hr/>

其实可以发现,如果$bits[i] = 0$时,那么只需要继续往后移动即可,如果$bits[i]=1$时,无论如何都需要移动两位。

而如果是合法的编码方式，那么根据正常的走法，应该正好走到最后一个位置即$n-1$。

```java
class Solution {
    public boolean isOneBitCharacter(int[] bits) {
        int n = bits.length;
        if(bits[n - 1] != 0) {
            return false;
        }
        int idx = 0;
        while(idx < n - 1) {
           if(bits[idx] == 0) {
               idx++;
           } else {
               idx += 2;
           }
        }
        return idx == n - 1;
    }
}
```

