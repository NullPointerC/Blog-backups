---
title: leetcode-537-复数乘法
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: '322666e1'
date: 2022-02-25 13:08:36
---

[537. 复数乘法](https://leetcode-cn.com/problems/complex-number-multiplication/)

![image-20220225131025930](https://gitee.com/cao_ziqiang/img/raw/master/20220225131026.png)

简单字符串模拟,因为肯定有加号,首先按加号分割开,得到4个数字,再分别进行实部和虚部的运算即可。

```java
class Solution {
    public String complexNumberMultiply(String num1, String num2) {
        int n1 = 0, n2 = 0, n3 = 0, n4 = 0;
        String[] s1 = num1.split("\\+");
        String[] s2 = num2.split("\\+");
        // System.out.println(Arrays.toString(s1));
        // System.out.println(Arrays.toString(s2));
        n1 = Integer.parseInt(s1[0]);
        n2 = Integer.parseInt(s1[1].substring(0, s1[1].length() - 1));
        n3 = Integer.parseInt(s2[0]);
        n4 = Integer.parseInt(s2[1].substring(0, s2[1].length() - 1));
        // System.out.println(n1 + " " + n2 + " " + n3 + " " + n4);
        int res1 = (n1 * n3) - (n2 * n4);
        int res2 = (n2 * n3) + (n1 * n4);
        return res1  + "+" + res2 + "i";
    }
}
```

