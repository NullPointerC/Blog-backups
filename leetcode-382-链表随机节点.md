---
title: leetcode-382-链表随机节点
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: bf79e14b
date: 2022-01-16 12:30:43
---

[382. 链表随机节点](https://leetcode-cn.com/problems/linked-list-random-node/)

![image-20220116123638472](https://gitee.com/cao_ziqiang/img/raw/master/20220116123638.png)

学习一下蓄水池采样。

一个比较简单也容易想到的做法是在初始化时就将所有的值存入一个数组中，再随机化一个下标，取该下标即可。

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

    private ListNode head;
    private List<Integer> vals;
    public Solution(ListNode head) {
        this.head = head;
        this.vals = new ArrayList<>();
        ListNode p = head;
        while(p != null) {
            vals.add(p.val);
            p = p.next;
        }
    }
    
    public int getRandom() {
        int size = vals.size();
        Random r = new Random();
        int idx = r.nextInt(size);
        return vals.get(idx);
    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(head);
 * int param_1 = obj.getRandom();
 */
```

当然这样肯定不是最优解法，更合适的解法是蓄水池采样。

我们都知道Java中$random.nextInt(int\ bound)$将会产生一个$[0, bound)$之间的随机数，假定链表长为$n$，那么$i=n$时，每个节点被选中的概率为$\frac{1}{n}$；

而假定我们最终选择前$n-1$个点而要求第$n$个一定不被选取时，此时概率就为：$(1 - \frac{1}{n}) \times \frac{1}{n-1} = \frac{1}{n}$。也能够保证每个节点被选中的概率为$\frac{1}{n}$。

故以此类推，每个节点的概率都是$\frac{1}{n}$。

```java
class Solution {
    private ListNode head;
    public Solution(ListNode head) {
       this.head = head;
    }
 
    public int getRandom() {
        int res = head.val;
        ListNode curr = head.next;
        int i = 2;
        Random random = new Random();
        while(curr != null){
            if(random.nextInt(i) == 0){
                res = curr.val;
            }
            i++;
            curr = curr.next;
        }
        return res;
    }
}
```

