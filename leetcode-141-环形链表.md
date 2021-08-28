---
title: leetcode-141-环形链表
date: 2021-07-12 08:09:20
categories: LeetCode
tags: [algorithm,Java,LeetCode]
---

[link](https://leetcode-cn.com/problems/linked-list-cycle/)

<hr/>

题目描述:

<pre>
给定一个链表，判断链表中是否有环。
如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，我们使
用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意：
pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。
如果链表中存在环，则返回 true 。 否则，返回 false 。
进阶：
你能用 O(1)（即，常量）内存解决此问题吗？
</pre>

示例1:

<pre>
输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。
</pre>

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210712081105.png)

示例2:

<pre>
输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。
</pre>

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210712081126.png)

示例3:

<pre>
输入：head = [1], pos = -1
输出：false
解释：链表中没有环。
</pre>

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210712081142.png)

思路:

<pre>
首先想到的方法是快慢指针法,快指针每次走两步,慢指针每次走一步,如果慢指针能够追得上快指针,那么说明
链表中有环,否则快指针将会更快到达链表尾部
</pre>

代码:

```java
class Solution {
    public boolean hasCycle(ListNode head) {
        if (head == null) {
            return false;
        }
        /*快指针*/
        ListNode fast = head;
        /*慢指针*/
        ListNode slow = head;
        /*即快指针没有遍历到尾部时*/
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            if (fast != slow) {
                continue;
            } else {
                return true;
            }
        }
        return false;
    }
}
```

![image-20210712081852539](https://gitee.com/cao_ziqiang/img/raw/master/20210712081852.png)

