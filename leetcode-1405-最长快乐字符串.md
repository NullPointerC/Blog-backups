---
title: leetcode-1405-最长快乐字符串
date: 2022-02-07 22:36:36
categories: LeetCode
tags: [LeetCode,algorithm]
---

[1405. 最长快乐字符串](https://leetcode-cn.com/problems/longest-happy-string/ "最长快乐字符串")

![image-20220207223808486](https://gitee.com/cao_ziqiang/img/raw/master/20220207223808.png)

个人感觉有点难哇,没想到通过率这么高,属实是没有想到,看答案其实不难,就是要选出现次数最多的字母,出现次数少的字母用来做分割。

如果加入当前字母会出现了3个连续的时候，就不能再选了。同时，因为相同字母剩的越多，越容易出现字母连续相同，所以要先选掉较多的字母。

吐槽一下Java竟然没有Pair这个类，又不想手写这里就用的int[]二元组。

```java
class Solution {
    public String longestDiverseString(int a, int b, int c) {
        StringBuilder res = new StringBuilder();
        List<int[]> choices = new ArrayList<>();
        choices.add(new int[]{a, 'a' - '0'});
        choices.add(new int[]{b, 'b' - '0'});
        choices.add(new int[]{c, 'c' - '0'});
        while(true) {
            // 按照出现的频率降序排序
            Collections.sort(choices, (x, y) -> {return y[0] - x[0];});
            int preSize = res.length();
            // 枚举所有的二元组
            for(var choice : choices) {
                // 没有了或者是加入后会使出现3个重复的跳过
                if(choice[0] == 0 || res.length() >= 2 && (choice[1] + '0') == res.charAt(res.length() - 1) && (choice[1] + '0') == res.charAt(res.length() - 2)) {
                    continue;
                }
                // 选了出现次数最多的那个二元组
                res.append((char)(choice[1] + '0'));
                choice[0]--;
                break;
            }
            if(preSize == res.length()) break;
        }
        return res.toString();
    }
}
```

