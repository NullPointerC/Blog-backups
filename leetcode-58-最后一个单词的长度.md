---
title: leetcode-58-最后一个单词的长度
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: 37d7cb35
date: 2021-09-21 10:09:11
---

[$link$](https://leetcode-cn.com/problems/length-of-last-word/)

![image-20210921100941140](https://gitee.com/cao_ziqiang/img/raw/master/20210921100941.png)

<hr/>

中秋节的福利吧，太简单了。

先用$trim$去除左右两边的空格，使得可以直接从字母开始遍历。

```java
class Solution {
    public int lengthOfLastWord(String s) {
        s = s.trim();
        int l = s.length();
        if(l == 0) {
            return 0;
        }
        int ans = 0;
        for(int i = l - 1;i >= 0;i--) {
            if(s.charAt(i) != ' ') {
                ans++;
            }else{
                break;
            }
        }
        return ans;
    }
}
```

