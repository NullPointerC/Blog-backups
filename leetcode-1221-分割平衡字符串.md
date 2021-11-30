---
title: leetcode-1221-分割平衡字符串
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-09-07 16:37:41
---

[link](https://leetcode-cn.com/problems/split-a-string-in-balanced-strings/)

<hr/>

**题目描述:**

在一个 平衡字符串 中，$'L'$ 和 $'R'$ 字符的数量是相同的。

给你一个平衡字符串 $s$，请你将它分割成尽可能多的平衡字符串。

注意：分割得到的每个字符串都必须是平衡字符串。

返回可以通过分割得到的平衡字符串的 最大数量 。

**示例1:**

输入：$s$ = $"RLRRLLRLRL"$

输出：4

解释：$s$ 可以分割为 $"RL"$、$"RRLL"$、$"RL"$、$"RL"$ ，每个子字符串中都包含相同数量的 $'L'$ 和 $'R'$ 。

<hr/>

示例 2：

输入：$s$ = $"RLLLLRRRLR"$

输出：3

解释：$s$ 可以分割为 $"RL"$、$"LLLRRR"$、$"LR"$ ，每个子字符串中都包含相同数量的 $'L'$ 和 $'R'$ 。

示例 3：

输入：$s$ = $"LLLLRRRR"$

输出：1

解释：$s$ 只能保持原样 $"LLLLRRRR"$.

示例 4：

输入：$s$ = $"RLRRRLLRLL"$

输出：2

解释：$s$ 可以分割为 $"RL"$、$"RRRLLRLL"$ ，每个子字符串中都包含相同数量的 $'L'$ 和 $'R'$ 。

思路:

很明显,对于一段子串$Str$，只需要确保这其中$L$和$R$的个数是一样的即可，得到了这样的一个字串，就从头开始计数即可；

代码：

```java
class Solution {
    public int balancedStringSplit(String s) {
        if (s == null || s.equals("")) {
            return 0;
        }
        int l = s.length();
        char[] chars = s.toCharArray();
        int balance = 0;
        int ans = 0;
        for (int i = 0; i < l; i++) {
            if (chars[i] == 'L') {
                balance++;
            } else {
                balance--;
            }
            if (balance == 0) {
                ans++;
            }
        }
        return ans;
    }
}
```

