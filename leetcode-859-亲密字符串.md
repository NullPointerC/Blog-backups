---
title: leetcode-859-亲密字符串
date: 2021-11-23 19:20:23
categories: LeetCode
tags: [LeetCode,algorithm]
---

[亲密字符串](https://leetcode-cn.com/problems/buddy-strings/)

<hr/>

![image-20211123192105859](https://gitee.com/cao_ziqiang/img/raw/master/20211123192105.png)

<hr/>

虽然标注的是简单题，但通过率并不高，因为边界情况比较复杂。

因为这里要注意到，只允许交换一组字母，再要交换是不符合题意的了。

所以考虑两字符串是否可以交换一组字母就使得他们相等。

若二者长度不等是肯定不行的，可以直接return掉；

若二者长度相等，如果二者是相等的，就不存在不等位置的字母，这时要看是否存在重复的字母，这时移动重复的字母也算作是交换过了；

若二者不等，则看是否可以只交换一组就使得二者相等。

```java
class Solution {
    public boolean buddyStrings(String s, String goal) {
        if (s.length() != goal.length()) {
            return false;
        }
        
        if (s.equals(goal)) {
            int[] count = new int[26];
            for (int i = 0; i < s.length(); i++) {
                count[s.charAt(i) - 'a']++;
                if (count[s.charAt(i) - 'a'] > 1) {
                    return true;
                }
            }
            return false;
        } else {
            int first = -1, second = -1;
            for (int i = 0; i < goal.length(); i++) {
                if (s.charAt(i) != goal.charAt(i)) {
                    if (first == -1)
                        first = i;
                    else if (second == -1)
                        second = i;
                    else
                        return false;
                }
            }

            return (second != -1 && s.charAt(first) == goal.charAt(second) &&
                    s.charAt(second) == goal.charAt(first));
        }
    }
}
```

