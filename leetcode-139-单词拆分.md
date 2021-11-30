---
title: leetcode-139-单词拆分
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-09-21 11:21:15
---

[$link$](https://leetcode-cn.com/problems/word-break/)

<hr/>

![image-20210921143132676](https://gitee.com/cao_ziqiang/img/raw/master/20210921143132.png)

又是我不会的$dp$。。。

做这种问题一定要仔细分析,看当前问题能否用上前面的子问题。

我们要求的是字符串$s$能否被字典分割，如果字符串的字串可以被分割，那么就说明这个字符串也是能够被分割的。

所以考虑以下问题：

假如我们已知前$j$个字符组成的字符串可以被字典分割，那么再考虑前$i$个字符。($0 \lt j \le i \le s.length$)

这时可以将前前$i$个字符分为前$j$个字符和$s[j..i]$这两部分，对于前$j$个字符，可以用前面的结果，对于$s[j..i]$这一部分，我们可以先用哈希表存储，以便查询这部分是否在字典中。

设$dp[i]$为前$i$个字符能否被分割，初始状态为空串，考虑其已经不可再分，所以设置为$true$。

对于后续：

$dp[i] = dp[j] \text{&&} check(s[j..i])$，$0 \lt j \le i \lt s.length$。

```java
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        Set<String> wordDictSet = new HashSet(wordDict);
        boolean[] dp = new boolean[s.length() + 1];
        dp[0] = true;
        for (int i = 1; i <= s.length(); i++) {
            for (int j = 0; j < i; j++) {
                if (dp[j] && wordDictSet.contains(s.substring(j, i))) {
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[s.length()];
    }
}
```

