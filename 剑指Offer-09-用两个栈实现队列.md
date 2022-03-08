---
title: 剑指Offer-09-用两个栈实现队列
categories: 剑指Offer
tags:
  - 剑指Offer
hidden: true
abbrlink: deb45271
date: 2021-12-10 16:29:28
---

这周起也就开起刷剑指offer了,因为感觉一直刷leetcode也不是初心,主要是学以致用,能够拿下基本的面试算法题以及能够说明白就可以了，💪。

[剑指 Offer 09. 用两个栈实现队列](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)

<hr/>

![image-20211210163104517](https://gitee.com/cao_ziqiang/img/raw/master/20211210163104.png)

<hr/>

思路比较好想到，可以先用一个栈全部存储入队元素，设这个栈为$stack1$，需要出栈时，判断另一个栈是否空，若为空，且$stack1$不为空，则说明可以从刚才入队的元素出队，于是$stack1$的元素转移到$stack2$，并将$stack2$弹栈，这样的过程使得最先入栈的元素，此时又置于栈顶，满足了队列的性质。

```java
class CQueue {

    private Stack<Integer> stack1;
    private Stack<Integer> stack2;
    public CQueue() {
        stack1 = new Stack<>();
        stack2 = new Stack<>();
    }
    
    public void appendTail(int value) {
        stack1.push(value);
    }
    
    public int deleteHead() {
        if(stack2.isEmpty()) {
            while(!stack1.isEmpty()) {
                stack2.push(stack1.pop());
            }
        }
        return stack2.isEmpty() ? -1 : stack2.pop();
    }
}

/**
 * Your CQueue object will be instantiated and called as such:
 * CQueue obj = new CQueue();
 * obj.appendTail(value);
 * int param_2 = obj.deleteHead();
 */
```

