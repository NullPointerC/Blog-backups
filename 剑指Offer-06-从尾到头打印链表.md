---
title: 剑指Offer-06-从尾到头打印链表
categories: 剑指Offer
tags:
  - 剑指Offer
hidden: true
abbrlink: a0a3db1e
date: 2021-12-11 22:52:29
---

[剑指 Offer 06. 从尾到头打印链表](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

<hr/>

![image-20211211225303352](http://static.codenote.xyz/img/20211211225303.png)

<hr/>

比较好想，可以使用递归，到空时退出即可，否则一直往后递归，在递归出栈时将结点值存回$vals$中。

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
    private List<Integer> vals;
    public int[] reversePrint(ListNode head) {
        vals = new ArrayList<>();
        dfs(head);
        // System.out.println(vals);
        // return null;
        int[] nodes = new int[vals.size()];
        for(int i = 0; i < vals.size(); i++) {
            nodes[i] = vals.get(i);
        }
        return nodes;
    }
    private void dfs(ListNode node) {
        if(node == null) {
            return ;
        }
        if(node != null) {
            dfs(node.next);
            vals.add(node.val);
        }
    }
}
```

也可以借助栈来实现。

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
    public int[] reversePrint(ListNode head) {
        Deque<Integer> stack = new ArrayDeque<>();
        while(head != null) {
            stack.push(head.val);
            head = head.next;
        }
        int[] vals = new int[stack.size()];
        for(int i = 0; i < vals.length; i++) {
            vals[i] = stack.pop();
        }
        return vals;
    }
}
```

当然循环两遍也是可行的解法。

```java
class Solution {
    public static int[] reversePrint(ListNode head) {
        ListNode node = head;
        int count = 0;
        while (node != null) {
            ++count;
            node = node.next;
        }
        int[] nums = new int[count];
        node = head;
        for (int i = count - 1; i >= 0; --i) {
            nums[i] = node.val;
            node = node.next;
        }
        return nums;
    }
}
```

