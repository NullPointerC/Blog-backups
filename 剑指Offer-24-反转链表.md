---
title: 剑指Offer-24-反转链表
categories: 剑指Offer
tags:
  - 剑指Offer
hidden: true
abbrlink: b263218e
date: 2021-12-11 23:16:55
---

[剑指 Offer 24. 反转链表](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/)

<hr/>

![image-20211211231742671](http://static.codenote.xyz/img/20211211231742.png)

比较好想到的办法应该是使用双指针。

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode dummy = new ListNode(-1);
        while(head != null) {
            ListNode temp = new ListNode(head.val,dummy.next);
            dummy.next = temp;
            head = head.next;
        }
        return dummy.next;
    }
}
```

用栈用递归也是可以的，用双指针比较简洁。

