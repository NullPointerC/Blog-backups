---
title: 剑指Offer-35-复杂链表的复制
categories: Offer
tags:
  - 剑指Offer
abbrlink: 9cd7e152
date: 2021-12-11 23:22:24
---

[剑指 Offer 35. 复杂链表的复制](https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/)

<hr/>

![image-20211211232305325](https://gitee.com/cao_ziqiang/img/raw/master/20211211232305.png)

<hr/>

这里为了防止重复复制，需要用一个$map$存储新老结点之间的关联关系。

```java
/*
// Definition for a Node.
class Node {
    int val;
    Node next;
    Node random;

    public Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}
*/
class Solution {
    private static final Map<Node, Node> visted = new HashMap<>();
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
            visted.put(node, newNode);
            newNode.next = dfs(node.next);
            newNode.random = dfs(node.random);
            return newNode;
        }
        return visted.get(node);
    }
}
```

Golang:

```go
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Next *Node
 *     Random *Node
 * }
 */
var copied map[*Node]*Node = make(map[*Node]*Node)
func dfs(node *Node) *Node {
    if node == nil {
        return nil
    }
    if oldNode, ok := copied[node]; ok {
        return oldNode
    }
    newNode := &Node{Val:node.Val}
    copied[node] = newNode
    newNode.Next = dfs(node.Next)
    newNode.Random = dfs(node.Random)
    return newNode
}

func copyRandomList(head *Node) *Node {
    if head == nil {
        return nil
    }
    return dfs(head)
}
```

或：

```java
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Next *Node
 *     Random *Node
 * }
 */
var copied map[*Node]*Node = make(map[*Node]*Node)
func dfs(node *Node) *Node {
    if node == nil {
        return nil
    }
    if oldNode, ok := copied[node]; ok {
        return oldNode
    }
    newNode := new (Node)
    newNode.Val = node.Val
    copied[node] = newNode
    newNode.Next = dfs(node.Next)
    newNode.Random = dfs(node.Random)
    return newNode
}

func copyRandomList(head *Node) *Node {
    if head == nil {
        return nil
    }
    return dfs(head)
}
```

