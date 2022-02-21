---
title: leetcode-838-推多米诺
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: 4ae908d3
date: 2022-02-21 15:02:41
---

[838. 推多米诺](https://leetcode-cn.com/problems/push-dominoes/)

![image-20220221150340588](https://gitee.com/cao_ziqiang/img/raw/master/20220221150340.png)

![image-20220221150351455](https://gitee.com/cao_ziqiang/img/raw/master/20220221150351.png)

看完题目以为是一道模拟题，所以就需要把各种情况都列出来，首先是碰到`L`多米诺骨牌，肯定会从当前位置一直倒向上一个上一个`R`位置。所以这里就需要记录下上一个`R`的位置`right`。

如果`right`为-1，也就是说当前`L`左侧没有`R`，从上一个`L`到当前`L`所有的牌都会被推倒为`L`。

如下两种情况。

```txt
....L
L...L
```

如果`right`不为-1，那么就需要看下标差，找到中间下标，交换即可。

```txt
R....L
```

如果碰到`R`骨牌，就需要从上一个`R`位置倒过来；

```txt
R....R
```



或者，直到最后

```txt
R....
```

```java
class Solution {
    public String pushDominoes(String dominoes) {
        int n = dominoes.length();
        char[] str = dominoes.toCharArray();
        int right = -1;
        int left = -1;
        for (int i = 0; i < str.length; i++) {
            if (str[i] == 'L'){
                if (right == -1) {
                    for (int j = left + 1; j < i; j++) {
                        str[j] = 'L';
                    }
                }else {
                    for (int j = right + 1; j < right + (i - right + 1) / 2; j++) {
                        str[j] = 'R';
                    }
                    for (int j = i - (i - right - 1) / 2; j < i; j++) {
                        str[j] = 'L';
                    }
                    right = -1;
                }
                left = i;
            }
            if (str[i] == 'R'){
                if (right != -1){
                    for (int j = right + 1; j < i ; j++) {
                        str[j] = 'R';
                    }
                }
                right = i;
            }
        }
        if (right != -1){
            for (int j = right + 1; j < str.length; j++) {
                str[j] = 'R';
            }
        }
        return new String(str);
    }
}
```

