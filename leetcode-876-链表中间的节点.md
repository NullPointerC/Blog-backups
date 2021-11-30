---
title: leetcode-876-链表中间的节点
date: 2021-11-04 08:44:12
categories: LeetCode
tags: [LeetCode,algorithm]
---

[$link$](https://leetcode-cn.com/problems/middle-of-the-linked-list/)

<hr/>

![image-20211104084449593](https://gitee.com/cao_ziqiang/img/raw/master/20211104084449.png)

<hr/>

思路比较简单，一趟遍历先获取到链表的总长$cnt$，再遍历链表一半长即可。

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode middleNode(ListNode head) {
        ListNode p = head;
        int cnt = 0;
        while(p != null) {
            p = p.next;
            ++cnt;
        }
        p = head;
        int mid = cnt >> 1;
        while(mid-- != 0) {
            p = p.next;
        }
        return p;
    }
}
```

<hr/>

同样可以考虑双指针，快指针每次走两步，满指针走一步。

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode middleNode(ListNode head) {
        ListNode fast = head, slow = head;
        while(fast != null&& fast.next != null ) {
            fast = fast.next.next;
            slow = slow.next;
        }
        return slow;
    }
}
```

