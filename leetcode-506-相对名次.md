---
title: leetcode-506-相对名次
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: 539b7022
date: 2021-12-02 09:08:29
---

[相对名次](https://leetcode-cn.com/problems/relative-ranks/)

<hr/>

![image-20211202090911612](https://gitee.com/cao_ziqiang/img/raw/master/20211202090911.png)

<hr/>

tags(哈希表、排序)

这里用了一种比较容易想到的办法就是先$copy$一份数组，对$copy$后的数组降序排序，然后这就得到了所有数字的顺序，将他们的值和顺序存入$map$中，然后再遍历原数组，就得到了排序后的顺序。

```java
class Solution {
    public String[] findRelativeRanks(int[] score) {
        int n = score.length;
        if (n == 0) {
            return null;
        }
        String[] ans = new String[n];
        Integer[] copy = new Integer[n];
        for(int i = 0; i < n; i++) {
            copy[i] = score[i];
        }
        Arrays.sort(copy, (a, b) -> {return b - a;});
        Map<Integer, String> map = new HashMap<>();
        for(int i = 0; i < n; i++) {
            if (i == 0) {
                map.put(copy[i], "Gold Medal");
            } else if (i == 1) {
                map.put(copy[i], "Silver Medal");
            } else if (i == 2) {
                map.put(copy[i], "Bronze Medal");
            } else {
                map.put(copy[i], String.valueOf(i + 1));
            }
        }
        for(int i = 0; i < n; i++) {
            ans[i] = map.get(score[i]);
        }
        return ans;
    }
}
```

也可以使用不开辟额外空间，使用一个$order$数组来标识所有的元素的排序后的顺序，其中对于$order$的排序规则为：

在score中元素的大小

```java
class Solution {
    public String[] findRelativeRanks(int[] score) {
        int n = score.length;
        if(n < 0) {
            return null;
        }
        Integer[] order = new Integer[n];
        for(int i = 0; i < n;i++) {
            order[i] = i;
        }
        Arrays.sort(order, (a, b) -> {return score[b] - score[a];});
        String[] ans = new String[n];
        for(int i = 0 ; i < n; i++) {
            switch(i) {
                case 0 : ans[order[i]] = "Gold Medal";break;
                case 1 : ans[order[i]] = "Silver Medal";break;
                case 2 : ans[order[i]] = "Bronze Medal";break;
                default : ans[order[i]] = String.valueOf(i + 1);break;
            }
        }
        return ans;
    }
}
```

