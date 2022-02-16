---
title: leetcode-131-分割回文串
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: b4196748
date: 2021-09-11 15:52:25
---

[$\large \textit{link}$](https://leetcode-cn.com/problems/palindrome-partitioning/)

<hr/>

给你一个字符串 $\large s$，请你将 $\large {s}$ 分割成一些子串，使每个子串都是 **回文串** 。返回 $s$ 所有可能的分割方案。

**回文串** 是正着读和反着读都一样的字符串。

<hr/>

由于是需要求出字符串$\large {s}$的所有分割方案，所以我们考虑使用搜索+回溯的方式枚举出所有可能的结果并进行判断。

假设，目前遍历至第$\large {i}$个字符，且$\large {s}[0..i-1]$位置的所有字符已经被分隔成为了回文字串，那么只需从$\large {i}$开始继续遍历至下一个回文串的右边界$\large{j}$，使$\large s[i..j]$是一个回文串即可。

因此，可以从$\large i$开始依次枚举$\large j$。如果$s[i..j]$是一个回文串，那么再以$j+1$为新起点开始搜索，直至搜索完了字符串的最后一个字符。

此处可以使用动态规划来对字符串做预处理，将$s$的每个字串是否是回文串表示出来。设$f(i,j)$表示$s[i..j]$是否为回文串，那么就会有状态转移方程如下：

$f(i,j) =
\begin{cases}
True, & i \geq j\\
f(i+1,j-1) \wedge (s[i]=s[j]), & otherwise
\end{cases}$

预处理完成之后，我们只需要 $O(1)$ 的时间就可以判断任意 $s[i..j]$是否为回文串了。

```java
class Solution {
    boolean[][] f;
    List<List<String>> ret = new ArrayList<List<String>>();
    List<String> ans = new ArrayList<String>();
    int n;

    public List<List<String>> partition(String s) {
        n = s.length();
        f = new boolean[n][n];
        for (int i = 0; i < n; ++i) {
            Arrays.fill(f[i], true);
        }

        for (int i = n - 1; i >= 0; --i) {
            for (int j = i + 1; j < n; ++j) {
                f[i][j] = (s.charAt(i) == s.charAt(j)) && f[i + 1][j - 1];
            }
        }

        dfs(s, 0);
        return ret;
    }

    public void dfs(String s, int i) {
        if (i == n) {
            ret.add(new ArrayList<String>(ans));
            return;
        }
        for (int j = i; j < n; ++j) {
            if (f[i][j]) {
                ans.add(s.substring(i, j + 1));
                dfs(s, j + 1);
                ans.remove(ans.size() - 1);
            }
        }
    }
}
```

