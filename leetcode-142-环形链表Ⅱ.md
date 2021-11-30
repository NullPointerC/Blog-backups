---
title: leetcode-142-环形链表Ⅱ
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-09-21 14:53:18
---

[$link$](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

<hr/>

![image-20210921145430641](https://gitee.com/cao_ziqiang/img/raw/master/20210921145430.png)

<hr/>

和$leetcode$141一样可以考虑使用哈希表也可以考虑使用双指针。

使用哈希表则是先将每个结点都添加进哈希表中，如果不能添加时，则说明已经添加过，这时将这个发生环形的位置返回即可。

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if(head == null) {
            return null;
        }
        Set<ListNode> set = new HashSet<>();
        ListNode ans = null;
        ListNode p = head;
        while(p != null) {
            if(!set.contains(p)) {
                set.add(p);
            }else{
                ans = p;
                break;
            }
            p = p.next;
        }
        return ans;
    }
}
```

<hr/>

再考虑使用双指针的解法。

![fig1](https://gitee.com/cao_ziqiang/img/raw/master/20210921151704.png)

我们假设链表由非环形部分$a$和环形部分$b+c$组成。设置两个指针分别为$slow$和$fast$。每次快指针走两步，慢指针走一步。这样下去，这两个指针将会在环形位置的某点相遇。

假设相遇位置在环形链表的长度为$b$。故根据两者走过的路程关系，会有如下：

快指针：$a+n(b+c)+b$

慢指针：$a+b$

即：$a+n(b+c)+b=2(a+b)$，得：$a=(n-1)b+nc$

所以我们再让满指针重走快指针的那段长度即可。

这时设置一个新的指针$ptr$，将其设置为$head$，让它和满指针同时出发，它们将会在环形入口相遇。

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if (head == null) {
            return null;
        }
        ListNode slow = head, fast = head;
        while (fast != null) {
            slow = slow.next;
            if (fast.next != null) {
                fast = fast.next.next;
            } else {
                return null;
            }
            if (fast == slow) {
                ListNode ptr = head;
                while (ptr != slow) {
                    ptr = ptr.next;
                    slow = slow.next;
                }
                return ptr;
            }
        }
        return null;
    }
}
```

