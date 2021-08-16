---
title: leetcode-242-有效的单词异位词
date: 2021-07-11 08:30:59
categories: algorithm
tags: [algorithm,Java,LeetCode]
---

[link](https://leetcode-cn.com/problems/valid-anagram/)

<hr/>

题目描述:

<pre>
给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。
</pre>

示例1:

<pre>
输入: s = "anagram", t = "nagaram"
输出: true
</pre>

示例2:

<pre>
输入: s = "rat", t = "car"
输出: false
</pre>

思路:

<pre>
建立两个map,对第一个map先++,后一个map--,最后看所有key值对应的value是否为0
</pre>

代码:

```java
class Solution {
    public boolean isAnagram(String s, String t) {
        if (s == null || t == null || s == "" || t == "") {
            return false;
        }
        char[] sArr = s.toCharArray();
        char[] tArr = t.toCharArray();
        int sLength = sArr.length;
        int tLength = tArr.length;
        if (sLength != tLength) {
            return false;
        }
        int[] map = new int[26];
        for (int i = 0; i < sLength; i++) {
            map[sArr[i] - 'a']++;
        }
        for (int i = 0; i < tLength; i++) {
            map[tArr[i] - 'a']--;
        }
        for (int i = 0; i < 26; i++) {
            if (map[i] != 0) {
                return false;
            }
        }
        return true;
    }
}
```

![image-20210711083922611](https://gitee.com/cao_ziqiang/img/raw/master/20210711083922.png)

