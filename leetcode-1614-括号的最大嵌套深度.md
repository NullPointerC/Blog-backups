---
title: leetcode-1614-括号的最大嵌套深度
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: e7c4f04f
date: 2022-01-07 11:14:10
---

[1614. 括号的最大嵌套深度](https://leetcode-cn.com/problems/maximum-nesting-depth-of-the-parentheses/)

![image-20220107111454881](https://gitee.com/cao_ziqiang/img/raw/master/20220107111454.png)

tags: 栈、字符串

题目已经提到了，给定的是有效的括号字符串，所以我们要求$s$的嵌套深度，只需使用栈，看最多有多少个左括号即可，因为一定是有效的括号字符串，所以不用检测是否合法。

```java
class Solution {
    public int maxDepth(String s) {
        int ans = 0;
        if(null == s || s.isBlank()) {
            return ans;
        }
        Deque<Character> stack = new ArrayDeque<>();
        for(var ch : s.toCharArray()) {
            if(ch == '(') {
                stack.push(ch);
                ans = Math.max(ans, stack.size());
            } else if (ch == ')'){
                stack.pop();
            } 
        }
        return ans;
    }
}
```

当然，不用栈也是可以的。

```java
class Solution {
    public int maxDepth(String s) {
        int ans = 0, sum = 0;
        if(null == s || s.isBlank()) {
            return ans;
        }
        for(var ch : s.toCharArray()) {
            if(ch == '(') {
                sum++;
                ans = Math.max(ans, sum);
            } else if (ch == ')'){
                sum--;
            } 
        }
        return ans;
    }
}
```

这里只有两个因素可以影响到结果，对结果的贡献度分别为-1和1。

