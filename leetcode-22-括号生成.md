---
title: leetcode-22-括号生成
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
hidden: true
abbrlink: '533e3721'
date: 2021-08-24 09:51:22
---

[link](https://leetcode-cn.com/problems/generate-parentheses/submissions/)

<hr/>

看到tags里面有动态规划,以为是一道普通的dp题,但是做了才发现,middle题还是难的

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210824095251.png)

写了将近1个小时,试过枚举,试过找dp的条件,还是不会做。。。

后来老实看题解，发现这是一道回溯题，好吧，其实回溯也可以算是一种动态规划？

<hr/>

题目描述：

<pre>
数字 n 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 有效的 括号组合。
有效括号组合需满足：左括号必须以正确的顺序闭合。
</pre>

示例1：

<pre>
输入：n = 3
输出：["((()))","(()())","(())()","()(())","()()()"]
</pre>

示例2：

<pre>
输入：n = 1
输出：["()"]
</pre>

思路：

<pre>
本质就是深搜+剪枝，从空字符串开始枚举每一种可能的情况。
可以在左括号大于n，右括号大于n,右大括号比左大括号更多时剪枝。
如果左右大括号数都为n时，说明这时一个可能的情况，再回溯。
回溯后，继续搜索，分别是在末尾加上左括号，和在末尾加上右括号。
</pre>

代码：

```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> ans = new LinkedList<>();
        generate(ans, "", 0, 0, n);
        return ans;
    }

    private void generate(List<String> list, String str, int left, int right, int n) {
        if (left > n || right > n || right > left) {
            return;
        }
        if (left == n && right == n) {
            list.add(str);
        }
        generate(list, str + "(", left + 1, right, n);
        generate(list, str + ")", left, right + 1, n);
    }
}
```

![image-20210824095753762](https://gitee.com/cao_ziqiang/img/raw/master/20210824095753.png)

