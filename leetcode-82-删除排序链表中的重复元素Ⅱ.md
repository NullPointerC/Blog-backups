---
title: leetcode-82-删除排序链表中的重复元素Ⅱ
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: e7d96814
date: 2021-11-16 20:16:20
---

[$link$](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/)

<hr/>

![image-20211116201704349](https://gitee.com/cao_ziqiang/img/raw/master/20211116201704.png)

示例1：

![image-20211116201732182](https://gitee.com/cao_ziqiang/img/raw/master/20211116201732.png)

示例2：

![image-20211116201800229](https://gitee.com/cao_ziqiang/img/raw/master/20211116201800.png)

<hr/>

tags: (双指针、链表)

我们可以使用双指针来解决这个这题，设置一左指针和一右指针，左指针始终走的比右指针慢一步。

这里为了方便，还新增了一个虚拟头$dummy$。

第一轮开始的时候，$r$和$r.next$不等，两个指针各自走一步

![image-20211116202416276](https://gitee.com/cao_ziqiang/img/raw/master/20211116202416.png)

结束时：

![image-20211116202450821](https://gitee.com/cao_ziqiang/img/raw/master/20211116202450.png)

再次比较，仍然不等，于是继续移动：

![image-20211116202521091](https://gitee.com/cao_ziqiang/img/raw/master/20211116202521.png)

此时，这一轮的$r$和$r.next$相等了，固定左指针$l$，继续移动右指针，直到碰到第一个不等的位置。

![image-20211116202636517](https://gitee.com/cao_ziqiang/img/raw/master/20211116202636.png)

到这个位置时，$r$和$r.next$不等，此时还是不能移动左指针，要确定了$r$和$r.next$不等了才可以移动，因为删除了中间的那些重复节点，要将这部分串起来，于是设置$l.next=r.next$。再接着移动右指针。

![image-20211116203008377](https://gitee.com/cao_ziqiang/img/raw/master/20211116203008.png)

再次比较$r$和$r.next$的关系，又是相等的，于是右移：

![image-20211116203052909](https://gitee.com/cao_ziqiang/img/raw/master/20211116203052.png)

到这里了，不等了，于是改变$l.next$的指向，并继续右移右指针。

![image-20211116203237191](https://gitee.com/cao_ziqiang/img/raw/master/20211116203237.png)

此时$r.next$为$null$，可以说明链表遍历完成，已经没有重复元素了。

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
        ListNode l, r, dummy;
        dummy = new ListNode(-1, head);
        l = dummy;
        r = head;
        while(r != null && r.next != null) {
            if(r.val != r.next.val) {
                l = r;
                r = r.next;    
            } else {
                while(r.next != null && r.next.val == r.val) {
                    r = r.next;
                }
                l.next = r.next;
                r = r.next;
            }
        }
        return dummy.next;
    }
}
```

Golang:

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func deleteDuplicates(head *ListNode) *ListNode {
    dummy := &ListNode{
        Val : -1,
        Next : head,
    }
    l, r := dummy, head
    for r != nil && r.Next != nil {
        if r.Val != r.Next.Val {
            l = r
            r = r.Next
        } else {
            for r.Next !=nil && r.Next.Val == r.Val {
                r = r.Next
            }
            l.Next = r.Next
            r = r.Next
        }
    }
    return dummy.Next
}
```



