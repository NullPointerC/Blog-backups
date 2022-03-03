---
title: leetcode-258-各位相加
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: d5102e07
date: 2022-03-03 10:47:05
---

[258. 各位相加](https://leetcode-cn.com/problems/add-digits/)

![image-20220303104740832](https://gitee.com/cao_ziqiang/img/raw/master/20220303104740.png)

<hr/>

根据题意递归模拟即可。

```java
class Solution {
    public int addDigits(int num) {
        return dfs(num);
    }

    private int dfs(int n) {
        if(n < 10) {
            return n;
        }
        int tmp = 0;
        while(n != 0) {
            tmp += n % 10;
            n /= 10;
        }
        return dfs(tmp);
    }
}
```

这个`O(1)`时间复杂度的解有点难想啊😂。

假设$x$是一个三位数

$x=100\times a + 10 \times b + 1 \times c$

$x' = a + b + c$

二者差值为$99\times a + 9 \times b = 9 \times(11\times a + b)$

所以可以得知$a+b+c=x\%9$，所以当$a+b+c \le 9$时，可以直接返回，其他情况，不断对$9$取模即可。

```java
class Solution {
    public int addDigits(int num) {
        if(num > 9) {
            num = num % 9;
            if(num == 0) {
                return 9;
            }
        }
        return num;
    }
}
```

