---
title: NC4-判断链表中是否有环
categories: [nowcoder]
tags:
  - algorithm
  - nowcoder
date: 2021-09-26 19:03:14
---

[$link$](https://www.nowcoder.com/practice/650474f313294468a4ded3ce0f7898b9?tpId=188&&tqId=38576&rp=1&ru=/activity/oj&qru=/ta/job-code-high-week/question-ranking)

<hr/>

![image-20210926190443312](https://gitee.com/cao_ziqiang/img/raw/master/20210926190443.png)

<hr/>

很经典的链表题,可以有哈希的思路也可以用双指针。

哈希：

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
import java.util.HashSet;
public class Solution {
    public boolean hasCycle(ListNode head) {
        if(head == null) {
            return false;
        }
        HashSet<ListNode> set = new HashSet<>();
        while(head != null) {
            if(!set.add(head)) {
                return true;
            }
            head = head.next;
        }
        return false;
    }
}
```

判断是否能添加，如果成环，必然会循环到成环的地方，set无法添加时就表示链表有环，直接退出即可。

<hr/>

双指针：让每次快指针走两步，慢指针走一步，如果成环，慢指针一定会追上快指针：

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public boolean hasCycle(ListNode head) {
        if(head == null) {
            return false;
        }
        ListNode fast = head;
        ListNode slow = head;
        while(fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            if(fast == slow) {
                return true;
            }
        }
        return false;
    }
}
```

