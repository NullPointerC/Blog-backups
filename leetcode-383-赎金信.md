---
title: leetcode-383-赎金信
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-07-11 08:11:20
---

[link](https://leetcode-cn.com/problems/ransom-note/)

<hr/>

题目描述:

<pre>
给定一个赎金信 (ransom) 字符串和一个杂志(magazine)字符串，判断第一个字符串 ransom 能不能由第二个字符串
magazines 里面的字符构成。如果可以构成，返回 true ；否则返回 false。
(题目说明：为了不暴露赎金信字迹，要从杂志上搜索各个需要的字母，组成单词来表达意思。杂志字符串中的每个字符只
能在赎金信字符串中使用一次。)
</pre>

示例1:

<pre>
输入：ransomNote = "a", magazine = "b"
输出：false
</pre>

示例2:

<pre>
输入：ransomNote = "aa", magazine = "ab"
输出：false
</pre>

示例3:

<pre>
输入：ransomNote = "aa", magazine = "aab"
输出：true
</pre>

思路:

<pre>
题目说到ransom是由magazine组成的,所以我们可以知道ransom肯定比magazine长度小
所以对较小的那个字符串建立起char-int(value-count)的映射关系
再遍历一趟magazine,如果在map中存在key char,则更新char的value减一
最后在对map的keySet进行一趟遍历,如果找到还有value值大于0的,则说明不能全由magazine组成
</pre>

代码:

```java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        if (ransomNote == "" || magazine == "" || ransomNote == null || magazine == null) {
            return false;
        }
        char[] ransom = ransomNote.toCharArray();
        char[] magazineArr = magazine.toCharArray();
        int ransomLength = ransom.length;
        int magazineLength = magazineArr.length;
        if (ransomLength > magazineLength) {
            return false;
        }
        HashMap<Character, Integer> map = new HashMap<>();
        for (int i = 0; i < ransomLength; i++) {
            map.put(ransom[i], map.getOrDefault(ransom[i], 0) + 1);
        }
        for (int i = 0; i < magazineLength; i++) {
            map.put(magazineArr[i], map.getOrDefault(magazineArr[i], 0) - 1);
        }
        for (Character ch : map.keySet()) {
            if (map.get(ch) > 0) {
                return false;
            }
        }
        return true;
    }
}
```

![image-20210711082835259](https://gitee.com/cao_ziqiang/img/raw/master/20210711082835.png)

后来看到评论区也是说可以用数组来提高一些运行速度,本质的思想和hashmap是一样的

```java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        // 由于 ransomNote 和 magazine 都仅仅有小写字母，公用一份 help
        int[] help = new int[26];
        // 杂志加数量
        for(char c : magazine.toCharArray()){
            ++help[c - 'a'];
        }
        // 赎金信减去数量
        for(char c: ransomNote.toCharArray()){
            // 杂志字符数量不够时，返回 false
            if(--help[c - 'a'] < 0) {
                return false;
            }
        }
        return true;
    }
}
```

![image-20210711083022782](https://gitee.com/cao_ziqiang/img/raw/master/20210711083022.png)

