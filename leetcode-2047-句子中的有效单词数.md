---
title: leetcode-2047-句子中的有效单词数
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: c0f6ca1
date: 2022-01-27 22:25:55
---

[2047. 句子中的有效单词数](https://leetcode-cn.com/problems/number-of-valid-words-in-a-sentence/)

![image-20220127222629440](https://gitee.com/cao_ziqiang/img/raw/master/20220127222629.png)

最讨厌做的大模拟题。。。

```java
class Solution {
    public int countValidWords(String sentence) {
        int ans=0;
        String pattern1="[a-z]+[-]?[a-z]+[!|.|,]?";//有连字符
        String pattern2="[a-z]*[!|.|,]?";//没有连字符
        String s[]=sentence.split(" ");
        for(int i=0;i<s.length;i++){if(s[i].length()>0&&(s[i].matches(pattern1)||s[i].matches(pattern2))){ans++;}}
        return ans;
    }
}
```

