---
title: leetcode-187-重复的DNA序列
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: e37c8c9f
date: 2021-10-08 10:12:51
---

[$link$](https://leetcode-cn.com/problems/repeated-dna-sequences/)

<hr/>

![image-20211008101357385](https://gitee.com/cao_ziqiang/img/raw/master/20211008101357.png)

<hr/>

维护一个大小为$10$的窗口,并不断将前沿往字符串末尾移动。每次将窗口内的字符串切下来存入$map$。若$cnt = 2$,则我们将这个字符串存入结果$list$中.

```java
class Solution {
    public List<String> findRepeatedDnaSequences(String s) {
        int n = s.length();
        List<String> ans = new ArrayList<>();
        Map<String, Integer> map = new HashMap<>();
        if(n < 10) {
            return new ArrayList(ans);
        }

        int front = 10, back = 0;
        while(front <= n) {
            String sub = s.substring(back, front);
            map.put(sub, map.getOrDefault(sub, 0) + 1);
            int cnt = map.get(sub);
            if(cnt == 2) {
                ans.add(sub);
            }
            front++;
            back++;
        }
        return ans;
    }
}
```

