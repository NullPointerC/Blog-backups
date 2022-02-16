---
title: leetcode-2000-反转单词的前缀
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: b16949a4
date: 2022-02-02 00:07:19
---

[2000. 反转单词前缀](https://leetcode-cn.com/problems/reverse-prefix-of-word/)

![image-20220202000805246](https://gitee.com/cao_ziqiang/img/raw/master/20220202000805.png)

比较简单的模拟，先找到ch第一次出现的位置`idx`，然后将`word[0...idx]`这一段反转即可。

```java
class Solution {
    public String reversePrefix(String word, char ch) {
        if(word == null || word.length() == 0) {
            return word;
        }
        char[] letters = word.toCharArray();
        int idx = 0, n = letters.length;
        for(; idx < n; idx++) {
            if(letters[idx] == ch) {
                break;
            }
        }
        if(idx == n) {
            return word;
        }
        int begin = 0, end = idx;
        while(begin < end) {
            char tmp = letters[begin];
            letters[begin] = letters[end];
            letters[end] = tmp;
            begin++;
            end--;
        }
        return new String(letters);
    }
}
```

本来还想试试map会不会更快，其实并没有，因为上面算法的时间复杂度为$O(k)$，其中$k$为$ch$在$word$中第一次出现的位置，空间复杂度为$O(n)$。

```java
class Solution {
    public String reversePrefix(String word, char ch) {
        if(word == null || word.length() == 0) {
            return word;
        }
        Map<Character, Integer> map = new HashMap<>();
        for(int i = 0; i < word.length(); i++) {
            if(!map.containsKey(word.charAt(i))) {
                map.put(word.charAt(i), i);
            } 
        }
        if(map.get(ch) == null) {
            return word;
        } else {
            int idx = map.get(ch);
            StringBuilder sb = new StringBuilder();
            for(int i = idx; i >= 0; i--) {
                sb.append(word.charAt(i));
            }
            for(int i = idx + 1; i < word.length(); i++) {
                sb.append(word.charAt(i));
            }
            return sb.toString();
        }
    }
}
```

如果用$map$，就一定要遍历完，时间复杂度为$O(n)$，并且向$StringBuilder$中添加元素的过程其实还是$O(n)$复杂度的一趟遍历，所以并没有更高效。

