---
title: leetcode-203-删除链表元素
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: 9abfaef7
date: 2021-07-12 09:00:24
---

[link](https://leetcode-cn.com/problems/remove-linked-list-elements/)

<hr/>

题目描述:

<pre>
给你一个链表的头节点 head 和一个整数 val ，请你删除链表中所有满足 Node.val == val 的节点，并返回 新的头节点 。
</pre>

示例1:

<pre>
输入：head = [1,2,6,3,4,5,6], val = 6
输出：[1,2,3,4,5]
</pre>

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210712090119.jpeg)

示例2:

<pre>
输入：head = [], val = 1
输出：[]
</pre>

示例3:

<pre>
输入：head = [7,7,7,7], val = 7
输出：[]
</pre>

思路:

<pre>
这里我是使用了一个比较简单的思路,创建一个新链表头结点,然后遍历原链表,如果值不为val,则添加进
</pre>

代码

```java
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        if (head == null) {
            return head;
        }
        ListNode dummyHead = new ListNode(0);
        ListNode tail = dummyHead;
        while (head != null) {
            if (head.val != val) {
                tail.next = head;
                tail = tail.next;
                tail.next = null;
            }
            head = head.next;
        }
        return dummyHead.next;
    }
}
```

看到官方给的两种题解也很不错,在这里都贴上来

1. 递归

![image-20210712092757363](https://gitee.com/cao_ziqiang/img/raw/master/20210712092757.png)

```java
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        if (head == null) {
            return head;
        }
        head.next = removeElements(head.next, val);
        return head.val == val ? head.next : head;
    }
}
```

2. 迭代

![image-20210712092817296](https://gitee.com/cao_ziqiang/img/raw/master/20210712092817.png)

```java
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        ListNode dummyHead = new ListNode(0);
        dummyHead.next = head;
        ListNode temp = dummyHead;
        while (temp.next != null) {
            if (temp.next.val == val) {
                temp.next = temp.next.next;
            } else {
                temp = temp.next;
            }
        }
        return dummyHead.next;
    }
}
```

![image-20210712093015010](https://gitee.com/cao_ziqiang/img/raw/master/20210712093015.png)

