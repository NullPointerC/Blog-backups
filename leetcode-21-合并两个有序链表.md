---
title: leetcode-21-合并两个有序链表
date: 2021-07-12 08:24:19
categories: LeetCode
tags: [algorithm,Java,LeetCode]
---

[link](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

<hr/>

题目描述:

<pre>
将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 
</pre>

示例 1：

<pre>
输入：l1 = [1,2,4], l2 = [1,3,4]
输出：[1,1,2,3,4,4]
</pre>

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210712082532.jpeg)

示例 2：

<pre>
输入：l1 = [], l2 = []
输出：[]
</pre>

示例 3：

<pre>
输入：l1 = [], l2 = [0]
输出：[0]
</pre>

思路:

<pre>
比较常见的归并排序,注意好数组边界即可,以及遍历完后还需要添加没有遍历完的链表结点
</pre>

代码:

```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode dummyHead = new ListNode(0);
        ListNode tail = dummyHead;
        while (l1 != null && l2 != null) {
            if (l1.val <= l2.val) {
                tail.next = new ListNode(l1.val);
                tail = tail.next;
                l1 = l1.next;
            } else {
                tail.next = new ListNode(l2.val);
                tail = tail.next;
                l2 = l2.next;
            }
        }
        while (l1 != null) {
            tail.next = new ListNode(l1.val);
            tail = tail.next;
            l1 = l1.next;
        }
        while (l2 != null) {
            tail.next = new ListNode(l2.val);
            tail = tail.next;
            l2 = l2.next;
        }
        return dummyHead.next;
    }
}
```

看到一种递归解法也感觉很有意思,在这里把它也贴上来

![image-20210712083031585](https://gitee.com/cao_ziqiang/img/raw/master/20210712083031.png)

```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if (l1 == null) {
            return l2;
        } else if (l2 == null) {
            return l1;
        } else if (l1.val < l2.val) {
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        } else {
            l2.next = mergeTwoLists(l1, l2.next);
            return l2;
        }

    }
}
```

![image-20210712083144034](https://gitee.com/cao_ziqiang/img/raw/master/20210712083144.png)

