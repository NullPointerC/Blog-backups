---
title: leetcode-500-键盘行
categories:
  - LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: bdb82c3b
date: 2021-10-31 16:29:25
---

[$link$](https://leetcode-cn.com/problems/keyboard-row/)

<hr/>

![image-20211031163149727](https://gitee.com/cao_ziqiang/img/raw/master/20211031163149.png)

<hr/>

首先想到的是比较简单的哈希法,先创建3个$set$存每一行的大小写字符。

然后再遍历到单词的时候，再遍历每个在单词中的字符是否完全出现在这个$set$中，如果是则添加进结果中，否则继续遍历。

这种解法的时间复杂度为$O(nm)$，$n$为给定的字符串数组大小，$m$为每个字符串的长度，因为在$set$中$contains$方法的时间复杂度为$O(1)$，所以综合来看是$O(nm)$的时间复杂度。

```java
class Solution {
    static Set<String> line1 = new HashSet<>();
    static Set<String> line2 = new HashSet<>();
    static Set<String> line3 = new HashSet<>();
    static {
        for(String s : "qwertyuiopQWERTYUIOP".split("")) {
            line1.add(s);
        }
        for(String s : "asdfghjklASDFGHJKL".split("")) {
            line2.add(s);
        }
        for(String s : "zxcvbnmZXCVBNM".split("")) {
            line3.add(s);
        }
    }
    public String[] findWords(String[] words) {
        List<String> ansList = new ArrayList<>();
        for(String word : words) {
            // System.out.println(word);
            if(isContainsInLine(word, line1)) {
                ansList.add(word);
            } else if (isContainsInLine(word, line2)) {
                ansList.add(word);
            } else if (isContainsInLine(word, line3)) {
                ansList.add(word);
            } else {
                continue;
            }
        }
        return ansList.toArray(new String[ansList.size()]);
    }

    private boolean isContainsInLine(String word, Set<String> set) {
        for(String letter : word.split("")) {
            if(!set.contains(letter)) {
                return false;
            }
        }
        return true;
    }
}
```

或者也可以提取打表，将每个字母所在的行数标识出来，这一就可以判断$words$每个单词中的每个字符是否都属于同一行的，若属于同一行的，则加入结果中去。

```java
class Solution {    
    public String[] findWords(String[] words) {
        List<String> list = new ArrayList<String>();
        String rowIdx = "12210111011122000010020202";
        for (String word : words) {
            boolean isValid = true;
            char idx = rowIdx.charAt(Character.toLowerCase(word.charAt(0)) - 'a');
            for (int i = 1; i < word.length(); ++i) {
                if (rowIdx.charAt(Character.toLowerCase(word.charAt(i)) - 'a') != idx) {
                    isValid = false;
                    break;
                }
            }
            if (isValid) {
                list.add(word);
            }
        }
        
        return list.toArray(new String[list.size()]);
    }
}
```

