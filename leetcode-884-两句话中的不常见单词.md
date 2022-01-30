---
title: leetcode-884-两句话中的不常见单词
date: 2022-01-30 15:31:08
categories: LeetCode
tags: [LeetCode,algorithm]
---

[884. 两句话中的不常见单词](https://leetcode-cn.com/problems/uncommon-words-from-two-sentences/)

![image-20220130153236203](https://gitee.com/cao_ziqiang/img/raw/master/20220130153236.png)

比较容易的可以看出是一个哈希计数类型的题目,分别统计在一个句子中出现1次而不在另一个句子中出现过的单词即可。

```java
class Solution {
    public String[] uncommonFromSentences(String s1, String s2) {
        String[] words1 = s1.split(" ");
        String[] words2 = s2.split(" ");
        Map<String, Integer> map1 = new HashMap<>();
        Map<String, Integer> map2 = new HashMap<>();
        for(var word : words1) {
            map1.put(word, map1.getOrDefault(word, 0) + 1);
        }
        for(var word : words2) {
            map2.put(word, map2.getOrDefault(word, 0) + 1);
        }
        List<String> ansList = new ArrayList<>();
        for(var word : map1.keySet()) {
            if(map1.get(word) == 1 && map2.get(word) == null) {
                ansList.add(word);
            }
        }
        for(var word : map2.keySet()) {
            if(map2.get(word) == 1 && map1.get(word) == null) {
                ansList.add(word);
            }
        }
        String[] ans = new String[ansList.size()];
        for(int i = 0; i < ansList.size(); i++) {
            ans[i] = ansList.get(i);
        }
        return ans;
    }
}
```

这个做法时间复杂度为$O(max(m,n))$，$m$和$n$分别是两句子的长度。

空间复杂度为$O(m)$。

可以考虑将两句子合并，这时就可以统计只出现过一次的单词了。

篇幅有限就不展开了。

