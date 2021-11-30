---
title: leetcode-784-字母大小写全排列
date: 2021-11-10 16:24:35
categories: LeetCode
tags: [LeetCode,algorithm]
---

[$link$](https://leetcode-cn.com/problems/letter-case-permutation/)

<hr/>

![image-20211110162519622](https://gitee.com/cao_ziqiang/img/raw/master/20211110162519.png)

<hr/>

也是比较常见的回溯，但是要注意当前位置是否为字母，如果是非字母，直接正常回溯就可。如果是字母，要进行大小写的回溯。

```java
class Solution {
    List<String> ans = new ArrayList<>();
    StringBuilder path = new StringBuilder();
    public List<String> letterCasePermutation(String s) {
        dfs(s, path, 0);
        return ans;
    }
    private void dfs(String s, StringBuilder path, int u) {
        if(u == s.length()) {
            ans.add(path.toString());
            return ;
        }
        if(isLowerLetter(s.charAt(u)) || isUpperLetter(s.charAt(u))) {
            path.append((char)(s.charAt(u) ^ ' '));
            dfs(s, path, u + 1);
            path.deleteCharAt(path.length() - 1);
            path.append(s.charAt(u));
            dfs(s, path, u + 1);
            path.deleteCharAt(path.length() - 1);
        } else {
            path.append(s.charAt(u));
            dfs(s, path, u + 1);
            path.deleteCharAt(path.length() - 1);
        }
    }
    private boolean isUpperLetter(char ch) {
        return (ch >= 'A' && ch <= 'Z');
    }
    private boolean isLowerLetter(char ch) {
        return (ch >= 'a' && ch <= 'z');
    }
}
```

