---
title: leetcode-946-验证栈序列
categories: LeetCode
tags:
  - LeetCode
  - algortihm
abbrlink: 6b53c8c9
date: 2022-03-02 22:19:16
---

[946. 验证栈序列](https://leetcode-cn.com/problems/validate-stack-sequences/)

![image-20220302221957147](https://gitee.com/cao_ziqiang/img/raw/master/20220302221957.png)

<hr/>

一开始还以为需要暴力地回溯模拟每个入栈出栈的可能性，看是否`popped`是一组·可行解，后来发现，只需要当`st`栈顶的元素和我们枚举到的的出栈序列遍历处的元素相同时，即可让遍历位置`pos`以后移动，同时`st`出栈一个元素，如果`pos`能够走完，说明`popped`是一个合法的出栈序列。

```java
class Solution {
    public boolean validateStackSequences(int[] pushed, int[] popped) {
        Deque<Integer> st = new ArrayDeque<>();
        int pos = 0;
        for(var num : pushed) {
            st.push(num);
            while(!st.isEmpty() && st.peek() == popped[pos]) {
                st.pop();
                pos++;
            }
        }
        return pos == popped.length;
    }
}
```

