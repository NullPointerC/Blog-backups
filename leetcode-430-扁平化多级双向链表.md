---
title: leetcode-430-扁平化多级双向链表
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: d76add41
date: 2021-09-24 12:33:11
---

[$link$](https://leetcode-cn.com/problems/flatten-a-multilevel-doubly-linked-list/)

<hr/>

![image-20210924123658520](https://gitee.com/cao_ziqiang/img/raw/master/20210924123658.png)

<hr/>

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210924123718.png)

扁平化后的链表如下图：

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210924123736.png)

<hr/>

乍一看可能觉得比较麻烦,一开始想一个一个接,后面发现这就是一棵歪过来的二叉树嘛,把$child$看成左孩子,把$next$看成右孩子。再返回前序遍历结果即可，为了使插入过程统一，引入一个$dummy$。但是在返回的时候需要指定$dummy.next.prev = null$，断开和dummy的链才是一个正确的双链表。

```java
class Solution {
    private Node dummy = new Node();
    private Node prev = dummy;
    public Node flatten(Node head) {
        if(head == null) {
            return null;
        }
        dfs(head);
        dummy.next.prev = null;
        return dummy.next;
    }

    private void dfs(Node head) {
        if(head == null) {
            return;
        }
        Node node = new Node();
        node.val = head.val;
        prev.next = node;
        node.prev = prev;
        prev = node;
        dfs(head.child);
        dfs(head.next);   
    }
}
```

也可以是先前序遍历将所有结点先存起来,再成链。

```java
class Solution {
    public Node flatten(Node head) {
        List<Node> list = new ArrayList<>();
        dfs(head, list);
        for(int i = 0; i < list.size(); i++){
            if(i == list.size()-1){
                list.get(i).next = null;
            }else{
                list.get(i).next = list.get(i+1);
                list.get(i+1).prev = list.get(i);
            }
            list.get(i).child = null;
        }
        return list.size()<1?null:list.get(0);
    }
    private void dfs(Node head, List<Node> list){
        if(head == null){
            return;
        }
        list.add(head);
        dfs(head.child, list);
        dfs(head.next, list);       
    }
}
```

