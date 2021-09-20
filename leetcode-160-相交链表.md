---
title: leetcode-160-相交链表
date: 2021-09-07 18:22:41
categories: LeetCode
tags: [algorithm,Java,LeetCode]

---

[link](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)

给你两个单链表的头节点 $\textit{headA}$和 $\textit{headB}$ ，请你找出并返回两个单链表相交的起始节点。如果两个链表没有交点，返回 $\color[rgb]{1,0.56,0.5}\it\large{null}$ 。

图示两个链表在节点 $\bf{c1}$ 开始相交**：**

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210907191042.png)

题目数据 **保证** 整个链式结构中不存在环。

**注意**，函数返回结果后，链表必须 **保持其原始结构** 。

**示例 1：**

[![img](https://gitee.com/cao_ziqiang/img/raw/master/20210907191056.png)](https://gitee.com/cao_ziqiang/img/raw/master/20210907191056.png)

<pre>
输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
输出：Intersected at '8'
解释：相交节点的值为 8 （注意，如果两个链表相交则不能为 0）。
从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。
在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
</pre>

**示例 2：**

[![img](https://gitee.com/cao_ziqiang/img/raw/master/20210907191232.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_2.png)



<pre>
输入：intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
输出：Intersected at '2'
解释：相交节点的值为 2 （注意，如果两个链表相交则不能为 0）。
从各自的表头开始算起，链表 A 为 [0,9,1,2,4]，链表 B 为 [3,2,4]。
在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。
</pre>

**示例 3：**

[![img](https://gitee.com/cao_ziqiang/img/raw/master/20210907191516.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_3.png)

<pre>
输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
输出：null
解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。
由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
这两个链表不相交，因此返回 null 
</pre>

思路：

若相交，链表A： $\mathrm a + \mathrm c$, 链表B : $b+c$. $a+c+b= b+c+a$ 。则会在公共处c起点相遇。若不相交，$a +b = b+a$ 。因此相遇处是NULL。

所以这也是题目所说的双指针

$\huge\it{C}ode$

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode A = headA, B = headB;
        while (A != B) {
            A = A != null ? A.next : headB;
            B = B != null ? B.next : headA;
        }
        return A;
    }
}
```

