---
title: leetcode-109-有序链表转换二叉搜索树
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: a8c1183f
date: 2021-08-20 09:35:13
---

[link](https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree/)

<hr/>

题目描述:

<pre>
给定一个单链表，其中的元素按升序排序，将其转换为高度平衡的二叉搜索树。
本题中，一个高度平衡二叉树是指一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1。
</pre>

示例:

<pre>
给定的有序链表： [-10, -3, 0, 5, 9],
一个可能的答案是：[0, -3, 9, -10, null, 5], 它可以表示下面这个高度平衡二叉搜索树：
      0
     / \
   -3   9
   /   /
 -10  5
</pre>

思路:

<pre>
这题可以参考他的前一题,将有序数组转二叉搜索树,只需要每次将中间元素取出做根结点,左边的元素作为它的左子树
右边的元素作为它的右子树。
但是链表相比起数组麻烦的一点是，链表不能通过索引来取值，所以我们这里通过双指针的方式取中间节点。
每次遍历时，快指针走两步，慢指针走一步，这样快指针到尾部时，慢指针正好走到一半的位置。
</pre>

代码：

```java
class Solution {
    public TreeNode sortedListToBST(ListNode head) {
        return build(head);
    }

    private TreeNode build(ListNode head) {
        if (head == null) {
            return null;
        }
        if (head.next == null) {
            return new TreeNode(head.val);
        }
        ListNode fast = head, slow = head, pre = null;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            pre = slow;
            slow = slow.next;
        }
        // 截断左半部分
        pre.next = null;
        // 截断右半部分
        ListNode rightList = slow.next;
        TreeNode root = new TreeNode(slow.val);
        root.left = build(head);
        root.right = build(rightList);
        return root;
    }
}
```

![image-20210820094013685](https://gitee.com/cao_ziqiang/img/raw/master/20210820094013.png)

