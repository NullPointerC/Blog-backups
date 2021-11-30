---
title: leetcode-206-反转链表
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-07-13 07:58:15
---

[link](https://leetcode-cn.com/problems/reverse-linked-list/)

<hr/>

题目描述:

<pre>
给你单链表的头节点 head ，请你反转链表，并返回反转后的链表。
</pre>

示例1:

<pre>
输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]
</pre>

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210713080019.jpeg)

示例 2：

<pre>
输入：head = [1,2]
输出：[2,1]
</pre>

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210713080037.jpeg)

示例3：

<pre>
输入：head = []
输出：[]
</pre>

思路：

<pre>
首先我想到的是使用栈来接收，利用栈先进后出的特性，然后再依次取出栈顶元素
</pre>

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null) {
            return null;
        }
        Stack<ListNode> stack = new Stack<>();
        while (head != null) {
            stack.push(head);
            head = head.next;
        }
        ListNode dummyHead = new ListNode();
        ListNode tail = dummyHead;
        while (!stack.isEmpty()) {
            tail.next = stack.pop();
            tail = tail.next;
            tail.next = null;
        }
        return dummyHead.next;
    }
}
```

<pre>
后来想到，可以原地排序，少占用一些空间，构造Stack栈对象也比较花费时间
</pre>

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null) {
            return null;
        }
        ListNode newHead = new ListNode();
        ListNode prev = null;
        ListNode next = head;
        while (next != null) {
            /*swap head node*/
            newHead = new ListNode(next.val);
            newHead.next = prev;
            /*iteration*/
            next = next.next;
            prev = newHead;
        }
        return newHead;
    }
}
```

![image-20210713082845799](https://gitee.com/cao_ziqiang/img/raw/master/20210713082845.png)

