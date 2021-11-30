---
title: leetcode-387-字符串中的第一个唯一字符
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-07-11 07:58:14
---

[link](https://leetcode-cn.com/problems/first-unique-character-in-a-string/)

<hr/>

![image-20210711080001667](https://gitee.com/cao_ziqiang/img/raw/master/20210711080001.png)

<hr/>

思路:

<pre>
用哈希表建立起value-count的映射关系,第一趟遍历先建立好映射
第二趟遍历再找到字符串中第一个只出现一次的唯一字符
</pre>

代码:

```java
class Solution {
    public int firstUniqChar(String s) {
        if(s == null || s.isEmpty()) {
            return -1;
        }
        int l = s.length();
        HashMap<Character, Integer> map = new HashMap<>();
        for(int i = 0; i < l; i++) {
            char ch = s.charAt(i);
            map.put(ch, map.getOrDefault(ch, 0) + 1);
        }
        int retIndex = -1;
        for(int i = 0; i < l; i++) {
            char ch = s.charAt(i);
            if(map.getOrDefault(ch,0) == 1) {
                retIndex = i;
                break;
            }
        }
        return retIndex;
    }
}
```

用数组可以更快一点,因为哈希表构建很花时间,但是总体下来都是`O(n)`的时间复杂度

