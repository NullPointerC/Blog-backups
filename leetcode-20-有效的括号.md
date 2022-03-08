---
title: leetcode-20-有效的括号
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
hidden: true
abbrlink: 87bc76a1
date: 2021-07-13 08:35:57
---

[link](https://leetcode-cn.com/problems/valid-parentheses/)

<hr/>

题目描述：

<pre>
给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。
有效字符串需满足：
	1.左括号必须用相同类型的右括号闭合。
	2.左括号必须以正确的顺序闭合。
</pre>

示例1：

<pre>
输入：s = "()"
输出：true
</pre>

示例2：

<Pre>
输入：s = "()[]{}"
输出：true
</pre>

示例3:

<pre>
输入：s = "(]"
输出：false
</pre>

示例4：

<pre>
输入：s = "([)]"
输出：false
</pre>

示例5：

<pre>
输入：s = "{[]}"
输出：true
</pre>

思路：

<pre>
比较典型的用栈来求解的题目。如果遇到左括号，那么将其入栈。
如果碰到右括号，若栈为空，则说明肯定不是有效的序列。若不为空，则比较栈顶和右括号是否匹配。
匹配则弹栈，不匹配则retrun false。
最后看栈是否空即可。
</pre>

```java
class Solution {
    public boolean isValid(String s) {
        if (s.equals("") || s == null) {
            return true;
        }
        char[] chars = s.toCharArray();
        Stack<Character> stack = new Stack<>();
        for (int i = 0; i < chars.length; i++) {
            char c = chars[i];
            if (c == '[' || c == '(' || c == '{') {
                stack.push(c);
            } else {
                if (stack.isEmpty()) {
                    return false;
                }
                Character peek = stack.peek();
                if (c == ')' && peek == '(' || c == ']' && peek == '[' || c == '}' && peek == '{') {
                    stack.pop();
                } else {
                    return false;
                }
            }
        }
        return stack.isEmpty();
    }
}
```

