---
title: leetcode-168-Excel表列名称
date: 2021-06-29 09:56:43
categories: LeetCode
tags: [algorithm,Java,LeetCode]
---

<a href="https://leetcode-cn.com/problems/excel-sheet-column-title/">传送门</a>

考试复习周太无聊了，实在是不想复习，还是搞算法，搞web，yyds

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210629101008.jpeg)

<hr/>

题目描述:

<pre>
给定一个正整数，返回它在 Excel 表中相对应的列名称。
	例如:
        1 -> A
    	2 -> B
    	3 -> C
    	...
    	26 -> Z
    	27 -> AA
    	28 -> AB 
    	...
</pre>

示例1:

<pre>
输入: 1
输出: "A"
</pre>

示例2:

<pre>
输入: 28
输出: "AB"
</pre>

实例3:

<pre>
输入: 701
输出: "ZY"
</pre>

<hr/>

思路:

<pre>
    本来以为是一个简单的10进制转26进制,但是这里有个很大的坑,注意在Excel中,没有0的,就是说26是存在的,而0是不存在的,也就是说26要表示成为Z,而不是A0...,所以我们在进行计算的时候,有一个很关键的地方就是需要减1,不能直接模26,直接模会有数组越界。。。所以在模之前需要减1，把这种奇怪的进制转26进制！！！
</pre>

代码:

```java
class Solution {
    public String convertToTitle(int columnNumber) {
        if(columnNumber <= 0) {
            return null;
        }
        /*初始化letters数组*/
        char[] letters = new char[26];
        for(int i = 0;i < 26;i++) {
            letters[i] =(char)('A' + i);
        }
        StringBuilder sb = new StringBuilder();
        while(columnNumber > 0) {
			/*在进行进制转换前需要-1把这种进制转换成真正的26进制*/
            columnNumber--;
            sb.append(letters[columnNumber % 26]);
            columnNumber /= 26;
        }
        /*将得到的stringbuilder反转输出*/
        return sb.reverse().toString();
    }
}
```

<hr/>

![image-20210629100703771](https://gitee.com/cao_ziqiang/img/raw/master/20210629100703.png)

