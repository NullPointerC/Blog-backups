---
title: leetcode-1629-按键持续时间最长的键
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: f603bcf7
date: 2022-01-09 10:12:42
---

[1629. 按键持续时间最长的键](https://leetcode-cn.com/problems/slowest-key/)

![image-20220109101324952](https://gitee.com/cao_ziqiang/img/raw/master/20220109101325.png)

tags: 哈希表、字符串模拟

这里第一眼看成了是求总的持续时间,原来是求单次的持续时间,这样就比较简单了。用一个$map$来存储每个按键的按下持续时间即可，始终维护最长的那个字母。

```java
class Solution {
    public char slowestKey(int[] releaseTimes, String keysPressed) {
        if(null == keysPressed || keysPressed.isBlank()) {
            return ' ';
        }
        char[] keys = keysPressed.toCharArray();
        int[] map = new int[26];
        int last = 0, n = releaseTimes.length, max = Integer.MIN_VALUE;
        char ans = 'a';
        for(int i = 0; i < n; i++) {
            map[keys[i] - 'a'] = Math.max(map[keys[i] - 'a'], releaseTimes[i] - last);
            last = releaseTimes[i];
            if(map[keys[i] - 'a'] > max) {
                ans = keys[i];
                max = map[keys[i] - 'a'];
            } else if(map[keys[i] - 'a'] == max && keys[i] > ans) {
                ans = keys[i];
                max = map[keys[i] - 'a'];
            }
        }
        return ans;
    }
}
```

甚至map都可以不要用，只需始终维护最大的即可。

```java
class Solution {
    public char slowestKey(int[] releaseTimes, String keysPressed) {
        if(null == keysPressed || keysPressed.isBlank()) {
            return ' ';
        }
        char ans = 'a';
        int max = Integer.MIN_VALUE, n = releaseTimes.length, last = 0;
        for(int i = 0; i < n; i++) {
            int curr = releaseTimes[i] - last;
            if(curr > max) {
                max = curr;
                ans = keysPressed.charAt(i);
            } else if(curr == max && keysPressed.charAt(i) > ans) {
                ans = keysPressed.charAt(i);
            } 
        }
        return ans;
    }
}
```

