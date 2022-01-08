---
title: leetcode-748-最短补全词
date: 2021-12-10 16:16:18
categories: LeetCode
tags: [LeetCode,algorithm]
---

[最短补全词](https://leetcode-cn.com/problems/shortest-completing-word/)

<hr/>

![image-20211210161655716](https://gitee.com/cao_ziqiang/img/raw/master/20211210161655.png)

<hr/>

tags (字符串、map)

比较常规的map比较类题目，先记录下$licensePlate$中的词频，再比较$words$中每个$word$的词频即可。

```java
class Solution {
    public String shortestCompletingWord(String licensePlate, String[] words) {
        int[] map = new int[26];
        if (licensePlate.isBlank()) {
            return null;
        } 
        licensePlate = licensePlate.replace(" ", "").toLowerCase();
        for(char c : licensePlate.toCharArray()) {
            if (c >= 'a' && c <= 'z') {
                map[c - 'a']++;
            }
        }
        // System.out.println(Arrays.toString(map));
        int minLength = Integer.MAX_VALUE;
        String ans = null;
        for(String word : words) {
            Map<Character, Integer> temp = new HashMap<>();
            for(char c : word.toCharArray()) {
                temp.put(c, temp.getOrDefault(c, 0) + 1);
            }
            boolean flag = true;
            for(int i = 0; i < 26; i++) {
                if(map[i] != 0 && map[i] > temp.getOrDefault((char)('a' + i), 0)) {
                    flag = false;
                    break;
                }
            }
            if (flag) {
                if (word.length() < minLength) {
                    minLength = word.length();
                    ans = word;
                }
            }
        }
        return ans;
    }
}
```

