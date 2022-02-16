---
title: leetcode-575-分糖果
categories:
  - LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: d79ccdd9
date: 2021-11-01 09:07:29
---

[$link$](https://leetcode-cn.com/problems/distribute-candies/)

<hr/>

![image-20211101090841887](https://gitee.com/cao_ziqiang/img/raw/master/20211101090841.png)

<hr/>

贪心, 先求出糖果的种类$types$。再判断其是否比数组半长更大，若更大，则说明妹妹可以获得数组半长那么多的种类，因为要均分，否则获取到种类数。

```java
class Solution {
    public int distributeCandies(int[] candyType) {
        Set<Integer> types = new HashSet<>();
        for(int type : candyType) {
            types.add(type);
        }
        return Math.min(candyType.length >> 1, types.size());
    }
}
```

