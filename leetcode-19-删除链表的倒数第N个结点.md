---
title: leetcode-19-删除链表的倒数第N个结点
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-09-08 10:25:14
---

[link](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

<hr/>

**题意:**

给你一个链表，删除链表的倒数第 $\rm n$个结点，并且返回链表的头结点。

你能尝试使用一趟扫描实现吗？

**示例 1：**

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210908102821.jpeg)

```
输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]
```

<hr/>

如果没有要求使用一趟遍历,那么是很简单的,先第一趟知道长度,再找出倒数第$N$个结点即可;

我们考虑这样的一种更高效的算法,使用一趟遍历完成,既然从头部数倒数第$N$个不好数,那么我们可以从尾部开始数,但是链表的遍历又是从头部开始的,所以我们借助栈来实现,先依次将节点$push$进$stack$,再从栈中$pop$出$N$个结点即可;

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0, head);
        Deque<ListNode> stack = new LinkedList<ListNode>();
        ListNode cur = dummy;
        while (cur != null) {
            stack.push(cur);
            cur = cur.next;
        }
        for (int i = 0; i < n; ++i) {
            stack.pop();
        }
        ListNode prev = stack.peek();
        prev.next = prev.next.next;
        ListNode ans = dummy.next;
        return ans;
    }
}
```

这样的做法时空复杂度均为$O(n)$,再借助双指针就可以做到常数空间下完成;

设置快慢两个指针,快指针先走$N$步,慢指针而后追赶,当快指针指向$null$时,慢指针就指向了要删除节点的前一个节点,

这时,只需要删除该节点即可;

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        if (head == null || head.next == null) {
            return null;
        }
        ListNode fast = head;
        for (int i = 0; i < n; i++) {
            fast = fast.next;
        }
        if (fast == null) {
            return head.next;
        }
        ListNode slow = head;
        while (fast != null && fast.next != null) {
            fast = fast.next;
            slow = slow.next;
        }
        slow.next = slow.next.next;
        return head;
    }
}
```

再考虑递归解法:

```java
class Solution {
    int curr = 0;
    public ListNode removeNthFromEnd(ListNode head, int n) {
        if(head == null) {
            return null;
        }
        head.next = removeNthFromEnd(head.next,n);
        curr++;
        if(n == curr) return head.next;
        return head;
    }
}
```

