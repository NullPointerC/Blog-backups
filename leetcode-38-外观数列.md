---
title: leetcode-38-外观数列
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: 9611f784
date: 2021-10-15 09:47:34
---

[$link$](https://leetcode-cn.com/problems/count-and-say/)

<hr/>

![image-20211015094847138](https://gitee.com/cao_ziqiang/img/raw/master/20211015094847.png)

<hr/>

题目的下方已经告诉了我们要如何考虑这个问题。

> 要描述一个数字字符串，首先要将字符串分割为最小数量的组，每个组都由连续的最多相同字符组成。然后对于每个组，先描述字符的数量，然后描述字符，形成一个描述组。要将描述转换为数字字符串，先将每组中的字符数量用数字替换，再将所有描述组连接起来。

```java
class Solution {
    public String countAndSay(int n){
        String ans = String.valueOf(1);
        for (int i = 2; i <= n; i++) {
            StringBuilder temp = new StringBuilder();
            // 寻找上一个结果的规律
            for (int j = 0; j < ans.length(); j++) {
                int count = 1;
                // 判断是否和下一个字符相等
                while (j + 1 < ans.length() && ans.charAt(j) == ans.charAt(j + 1)) {
                    count++;
                    j++;
                }
                temp.append(count).append(ans.charAt(j));
            }
            ans = temp.toString();
        }
        return ans;
    }
}
```

