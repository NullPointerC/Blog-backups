---
title: leetcode-1342-将数字变为0的操作次数
date: 2022-01-31 00:29:58
categories: LeetCode
tags: [LeetCode,algorithm]
---

[1342. 将数字变成 0 的操作次数](https://leetcode-cn.com/problems/number-of-steps-to-reduce-a-number-to-zero/)

![image-20220131003025196](https://gitee.com/cao_ziqiang/img/raw/master/20220131003025.png)

 根据题意模拟即可,leetcode的除夕福利吧,hhh

```java
class Solution {
    public int numberOfSteps(int num) {
        int ans = 0;
        while(num != 0) {
            if(num % 2 == 0) {
                num /= 2;
            } else {
                num -= 1;
            }
            ans++;
        }
        return ans;
    }
}
```

