---
title: leetcode-504-七进制数
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: 8c5e0604
date: 2022-03-07 11:24:10
---

[504. 七进制数](https://leetcode-cn.com/problems/base-7/)

![image-20220307112510714](https://gitee.com/cao_ziqiang/img/raw/master/20220307112510.png)

根据二进制的方式按照七进制的方式模拟即可。

```java
class Solution {
    public String convertToBase7(int num) {
        StringBuilder sb = new StringBuilder();
        if(num >= 0) {
            do {
                sb.append(num % 7);
                num /= 7;
            } while(num != 0);
        } else {
            num = 0 - num;
            do {
                sb.append(num % 7);
                num /= 7;
            } while(num != 0);
            sb.append('-');
        }
        return sb.reverse().toString();
    }
}
```

也是看到了Java中Integer类的`toString`。

```java
class Solution {
    public String convertToBase7(int num) {
        return Integer.toString(num, 7);
    }
}
```

