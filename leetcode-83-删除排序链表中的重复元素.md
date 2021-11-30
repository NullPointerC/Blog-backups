---
title: leetcode-83-删除排序链表中的重复元素
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-06-10 21:14:02
---

<a href="https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/" style="color:red;text-decoration:none">传送门</a>

<hr/>

![leetcode-cn.com_problems_remove-duplicates-from-sorted-list_](https://gitee.com/cao_ziqiang/img/raw/master/20210610211539.png)

思路:

由于这个链表是按照升序排列的,所以如果有重复元素,那么他们肯定是在一起排列的,所以我们只需要一趟遍历即可

如果当前结点的值和下一个结点的值相等,我们就直接将下一个结点返回回去当成新结点

如果不相等,则返回当前结点

这里借助一个评论区大佬的题解,大佬用了递归,代码看上去更加简洁明了了.

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
     public ListNode deleteDuplicates(ListNode head) {
        if(head == null || head.next == null){
            return head;
        }
        head.next = deleteDuplicates(head.next);
        if(head.val == head.next.val) head = head.next;
        return head;
    }
}
```

![image-20210610211904804](https://gitee.com/cao_ziqiang/img/raw/master/20210610211904.png)

<hr/>

最后,再写一下自己的渣渣解法

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
    public ListNode deleteDuplicates(ListNode head) {
        if(head == null || head.next == null) {
            return head;
        }
        ListNode node = head;
        while(node.next != null) {
            if(node.val == node.next.val) {
                //删除node.next结点
                node.next = node.next.next;
            } else {
                //否则继续遍历
                node = node.next;
            }
        }
        return head;
    }
}
```

![image-20210610212548065](https://gitee.com/cao_ziqiang/img/raw/master/20210610212548.png)

