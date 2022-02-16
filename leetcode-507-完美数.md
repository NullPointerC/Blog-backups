---
title: leetcode-507-完美数
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: dc10a7e1
date: 2021-12-31 11:58:36
---

[507. 完美数](https://leetcode-cn.com/problems/perfect-number/)

<hr/>

![image-20211231115918167](https://gitee.com/cao_ziqiang/img/raw/master/20211231115918.png)

tags: (数学)

求出所有的因数求和即可。

```java
class Solution {
    public boolean checkPerfectNumber(int num) {
        return num == getAllFactors(num).stream().mapToInt(Integer::new).sum();
    }

    private List<Integer> getAllFactors(int num) {
        List<Integer> factors = new ArrayList<>();
        for(int i = 1; i <= num / 2; i++) {
            if(num % i == 0) {
                factors.add(i);
            }
        }
        return factors;
    }
}
```

并且基于以上的事实，可以得到在测试范围内的完美数只有5个：$6、28、496、8128、33550336$

```java
class Solution {
    public boolean checkPerfectNumber(int num) {
        switch(num) {
            case 6:
            case 28:
            case 496:
            case 8128:
            case 33550336:
                return true;
        }
        return false;
    }
}
```

