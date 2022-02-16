---
title: leetcode-725-分隔链表
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: 3caabf83
date: 2021-09-22 19:05:16
---

[$link$](https://leetcode-cn.com/problems/split-linked-list-in-parts/)

<hr/>

![image-20210922190738313](https://gitee.com/cao_ziqiang/img/raw/master/20210922190738.png)

<hr/>

可能是做过最简单的中等题?

题目下方的提示注意看,看完就应该会有大概的思路.

```
If there are N nodes in the list, and k parts, then 
every part has N/k elements, except the first N%k 
parts have an extra one.
```

翻译过来就是说数组的每个部分的长度应该均摊下来是$N / K$。剩下的N % K个元素应该平分个前面的几个数组元素。

用$parts$记录每个数组元素平均的元素个数，用$extra$表示有多少个多余的元素，将这几个按次序分给数组中前几个。

```java
class Solution {
    public ListNode[] splitListToParts(ListNode head, int k) {
        ListNode[] ans = new ListNode[k];
        if (head == null) {
            return ans;
        }
        int len = 0;
        for (ListNode node = head; node != null; node = node.next) {
            len++;
        }

        int parts = len / k;
        int extra = len % k;
        ListNode curr = head;
        for (int i = 0; i < k; i++) {
            ListNode dummyHead = new ListNode(-1, null);
            ListNode tail = dummyHead;
            for (int j = 0; j < parts; j++) {
                ListNode temp = new ListNode(curr.val);
                tail.next = temp;
                tail = temp;
                curr = curr.next;
            }
            if (extra > 0) {
                ListNode temp = new ListNode(curr.val);
                tail.next = temp;
                tail = temp;
                curr = curr.next;
                extra--;
            }
            ans[i] = dummyHead.next;

        }
        return ans;
    }
}
```

