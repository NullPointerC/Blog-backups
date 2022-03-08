---
title: leetcode-30-串联所有单词的子串
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
hidden: true
abbrlink: a479358a
date: 2021-10-12 10:20:14
---

[$link$](https://leetcode-cn.com/problems/substring-with-concatenation-of-all-words/)

tags : 滑动窗口, 哈希表

![image-20211012102052143](https://gitee.com/cao_ziqiang/img/raw/master/20211012102052.png)

<hr/>

比较典型的滑动窗口+哈希表.控制窗口的大小为$n \times perLength$。其中$n$为$words$的长度，即有多少个单词，$perLenght$为每个单词的长度，题目已说明每个单词的长度相同，所以窗口大小也就等于两者的乘积。

我们把每个窗口内的单词分割下来，并且再按每个单词的长度分割为一个单词数组。先用$map$存储一遍这些单词出现的频率，再遍历一次给出的$words$数组，比较二者出现的频率，这里为了简洁起见，第一次遍历$words$时，先在减去$map$中频率，最后一趟遍历时，如果发现$words$中的单词其频率不为0，则说明该字串不能由$words$中的单词串联而来。

```java
class Solution {
    public List<Integer> findSubstring(String s, String[] words) {
        List<Integer> ans = new ArrayList<>();
        // 单词个数
        int n = words.length;
        // 每个单词的长度
        int perLength = words[0].length();
        // 窗口大小
        int winSize = n * perLength;
        // 如果s的长度比窗口还小
        int len = s.length();
        if(len < winSize) {
            return ans;
        }
        // 滑动窗口
        int front = winSize;
        int back = 0;
        while(front <= len) {
            Map<String, Integer> map = new HashMap<>();
            String subStr = s.substring(back, front);
            // System.out.println(subStr);
            // 依次将子串拆分加入map中
            for(int i = 0; i < subStr.length(); i += perLength) {
                String word = subStr.substring(i, i + perLength);
                map.put(word, map.getOrDefault(word, 0) + 1);
            }
            // 再比较给出words中的单词
            for(String word : words) {
                map.put(word, map.getOrDefault(word, 0) - 1);
            }
            boolean flag = true;
            for(String word : words) {
                if(map.get(word) < 0) {
                    flag = false;
                    break;
                }
            }

            if(flag) {
                ans.add(back);
            }
            back += 1;
            front += 1;
        }
        return ans;
    }
}
```

时间复杂度为$O(m\times n)$，$m$为$words$的长度，$n$为字符串$s$的长度。

