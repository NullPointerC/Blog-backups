---
title: leetcode-405-数字转换为十六进制数
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-10-02 13:12:11
---

[$link$](https://leetcode-cn.com/problems/convert-a-number-to-hexadecimal/)

<hr/>

![image-20211002131321116](https://gitee.com/cao_ziqiang/img/raw/master/20211002131321.png)

<hr/>

一开始是想自己去求出每个数字的32位二进制,再每4位每4位的取,但是这个过程编码是比较困难的,需要考虑的边界情况比较多也比较复杂，就没有继续算下去。

可以再考虑将给定的数字按每$4$位一组与$0xf$进行与运算,这样就得到了$num$的低$4$位,再将$num$右移4位。

举个栗子：

![img](https://gitee.com/cao_ziqiang/img/raw/master/20211002132645.jpg)



![image-20211002133822587](https://gitee.com/cao_ziqiang/img/raw/master/20211002133822.png)

最后就是返回$"dbaf9acf"$

![image-20211002134028413](https://gitee.com/cao_ziqiang/img/raw/master/20211002134028.png)

```java
class Solution {
    static char[] hexMap = "0123456789abcdef".toCharArray();
    public String toHex(int num) {
        if(num == 0) {
            return String.valueOf("0");
        }
        StringBuilder ans = new StringBuilder();
        while(ans.length() < 8 && num != 0) {
            ans.append(hexMap[num & 0xf]);
            num >>= 4;
        }
        return ans.reverse().toString();
    }
}
```

