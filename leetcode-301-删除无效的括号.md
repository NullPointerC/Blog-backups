---
title: leetcode-301-删除无效的括号
date: 2021-10-27 20:13:03
categories: [LeetCode]
tags: [LeetCode,algorithm]
---

[link](https://leetcode-cn.com/problems/remove-invalid-parentheses/)

<hr/>

![image-20211027201404390](https://gitee.com/cao_ziqiang/img/raw/master/20211027201404.png)

<hr/>

看到「所有可能」这样的字眼就应该想到回溯地去搜索所有可能的结果。

首先思考所有可能的方案，那么一定是左括号数和右括号数相等。若对于一个字符串$str$而言，出现左括号则选择得分为1，出现右括号则得分为-1。对于合法的方案，那么最终的得分就为0.

同时很显然，在搜索过程中的最大得分：max = min(左括号数，右括号数)

然后我们再枚举搜索过程中出现字符的处理情况：

- 出现普通的字母字符，无须处理，直接添加进结果即可；
- 出现左括号，如果当前的得分不超过max - 1时，可以选择添加，也可以选择不添加；
- 出现右括号，如果当前得分大于0，说明前面已有左括号和其匹配，那么也可以选择添加或不添加；

选择Set进行结果去重，`len`记录搜索过程中的最大字串，然后将所有长度为`len`的字串加入答案。

```java
class Solution {
    Set<String> set = new HashSet<>();
    int n, max, len;
    String s;
    public List<String> removeInvalidParentheses(String _s) {
        s = _s;
        n = s.length();
        int l = 0, r = 0;
        for (char c : s.toCharArray()) {
            if (c == '(') l++;
            else if (c == ')') r++;
        }
        // 预处理得到搜索过程中的最大得分
        max = Math.min(l, r);
        dfs(0, "", 0);
        return new ArrayList<>(set);
    }
    private void dfs(int u, String cur, int score) {
        // 小于0或超过最大值都是不合法的情况,退出回溯即可
        if (score < 0 || score > max) return ;
        // 搜索到字符串尾部
        if (u == n) {
            // 如果更长则清除set重置最大长度
            if (score == 0 && cur.length() >= len) {
                if (cur.length() > len) set.clear();
                len = cur.length();
                set.add(cur);
            }
            return ;
        }
        char c = s.charAt(u);
        if (c == '(') {
            dfs(u + 1, cur + String.valueOf(c), score + 1);
            dfs(u + 1, cur, score);
        } else if (c == ')') {
            dfs(u + 1, cur + String.valueOf(c), score - 1);
            dfs(u + 1, cur, score);
        } else {
            dfs(u + 1, cur + String.valueOf(c), score);
        }
    }
}
```

对于上述代码，我们会因为需要修改最长的合理方案长度而回溯多此，事实我们也可以进行预处理获得最终的合法方案长度进行剪枝，以减少回溯次数。

```java
class Solution {
    Set<String> set = new HashSet<>();
    int n, max, len;
    String s;
    public List<String> removeInvalidParentheses(String _s) {
        s = _s;
        n = s.length();

        int l = 0, r = 0;
        for (char c : s.toCharArray()) {
            if (c == '(') {
                l++;
            } else if (c == ')') {
                // 抵消前面的一个左括号
                if (l != 0) l--;
                else r++;
            }
        }
        System.out.println(l +"\t" + r);
        // 得到事实最终长度
        len = n - l - r;
        
        int c1 = 0, c2 = 0;
        for (char c : s.toCharArray()) {
            if (c == '(') c1++;
            else if (c == ')') c2++;
        }
        max = Math.min(c1, c2);

        dfs(0, "", l, r, 0);
        return new ArrayList<>(set);
    }
    private void dfs(int u, String cur, int l, int r, int score) {
        if (l < 0 || r < 0 || score < 0 || score > max) return ;
        if (l == 0 && r == 0) {
            if (cur.length() == len) set.add(cur);
        }
        if (u == n) return ;
        char c = s.charAt(u);
        if (c == '(') {
            dfs(u + 1, cur + String.valueOf(c), l, r, score + 1);//选择加入结果
            dfs(u + 1, cur, l - 1, r, score);// 不选择
        } else if (c == ')') {
            dfs(u + 1, cur + String.valueOf(c), l, r, score - 1);//选择加入结果
            dfs(u + 1, cur, l, r - 1, score);// 不选择
        } else {
            dfs(u + 1, cur + String.valueOf(c), l, r, score);
        }
    }
}
```

