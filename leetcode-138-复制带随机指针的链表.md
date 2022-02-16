---
title: leetcode-138-复制带随机指针的链表
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: f5e4d426
date: 2021-09-20 13:52:17
---

[$link$](https://leetcode-cn.com/problems/copy-list-with-random-pointer/)

<hr/>

![image-20210920135326570](https://gitee.com/cao_ziqiang/img/raw/master/20210920135326.png)

<hr/>

用$map$存$source$和$clone$的映射，如果还未建立关系，则创建出来并添加进$map$中。

```java
class Solution {
    Map<Node, Node> visted = new HashMap<>();
    public Node copyRandomList(Node head) {
        if(head == null) {
            return null;
        }
        Node newHead = dfs(head);
        return newHead;
    }

    private Node dfs(Node node) {
        if(node == null) {
            return null;
        }
        if(!visted.containsKey(node)) {
            Node newNode = new Node(node.val);
            visted.put(node,newNode);
            newNode.random = dfs(node.random);
            newNode.next = dfs(node.next);
            return newNode;
        }
        return visted.get(node);
    }
}
```

