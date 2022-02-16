---
title: leetcode-117-填充每个节点的下一个左侧节点指针Ⅱ
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: d4f9075f
date: 2021-08-27 16:31:20
---

[link](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node-ii/submissions/)

<hr/>

题目描述:

<pre>
给定一个二叉树
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右
侧节点，则将 next 指针设置为 NULL。
初始状态下，所有 next 指针都被设置为 NULL。
</pre>

示例:

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210827163245.png)

<pre>
输入：root = [1,2,3,4,5,null,7]
输出：[1,#,2,3,#,4,5,7,#]
解释：给定二叉树如图 A 所示，你的函数应该填充它的每个 next 指针，以指向其
下一个右侧节点，如图 B 所示。序列化输出按层序遍历顺序（由 next 指针连
接），'#' 表示每层的末尾。
</pre>

<pre>
相比起上一题,少了满二叉树的这个条件,如果继续使用bfs,无疑还是可以正常AC的
因为我们利用bfs的话和树的形状无关,只需要把同层的每个节点遍历过就可。
</pre>

代码:

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;
    public Node next;

    public Node() {}
    
    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, Node _left, Node _right, Node _next) {
        val = _val;
        left = _left;
        right = _right;
        next = _next;
    }
};
*/

class Solution {
    public Node connect(Node root) {
        Queue<Node> queue = new ArrayDeque<>();
        if (root == null) {
            return null;
        }
        queue.offer(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                Node node = queue.poll();
                if (i < size - 1) {
                    node.next = queue.peek();
                }
                if (node.left != null) {
                    queue.offer(node.left);
                }
                if (node.right != null) {
                    queue.offer(node.right);
                }
            }
        }
        return root;
    }
}
```

![image-20210827163445760](https://gitee.com/cao_ziqiang/img/raw/master/20210827163445.png)

<pre>
当然也可以使用递归,先求解子问题
对于一个节点的左孩子,他的next有两种情况:
第一种是父亲的右孩子不为空,那么直接把next链接过右孩子上即可;
第二钟是父亲的右孩子为空,那么next就应该去父亲的next中寻找,
此时存在一种情况时,父亲的next并不一定是相邻的节点,所以可能并不一定是root.next;
所以需要递归的寻找父亲的next;
处理完左孩子,对于右孩子的情况就很好处理,只需要把右孩子直接链接到
父亲的next的第一个孩子中即可;
</pre>

代码如下:

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;
    public Node next;

    public Node() {}
    
    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, Node _left, Node _right, Node _next) {
        val = _val;
        left = _left;
        right = _right;
        next = _next;
    }
};
*/

class Solution {
    public Node connect(Node root) {
        if (root == null) {
            return null;
        }
        traversal(root);
        return root;
    }

    private void traversal(Node root) {
        if (root == null) {
            return;
        }
        if (root.left != null) {
            if (root.right != null) {
                root.left.next = root.right;
            } else {
                root.left.next = getNext(root.next);
            }
        }
        if (root.right != null) {
            root.right.next = getNext(root.next);
        }
        traversal(root.right);
        traversal(root.left);
    }

    private Node getNext(Node node) {
        if (node == null) return null;
        if (node.left != null) return node.left;
        if (node.right != null) return node.right;
        if (node.next != null) return getNext(node.next);
        return null;
    }
}
```

<pre>
也可以使用非递归解法,将树按自顶向下的顺序遍历,类似于bfs,但是不使用队列
</pre>

代码:

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;
    public Node next;

    public Node() {}
    
    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, Node _left, Node _right, Node _next) {
        val = _val;
        left = _left;
        right = _right;
        next = _next;
    }
};
*/

class Solution {
    public Node connect(Node root) {
        if (root == null) {
            return null;
        }
        Node mostLeft = root;
        while (mostLeft != null) {
            // 从当前层开始遍历每一个非空节点,将下一层连接
            Node curr = mostLeft;
            // 标识下一层的虚拟头和尾部
            Node head, tail;
            head = tail = new Node(0);
            while (curr != null) {
                if (curr.left != null) {
                    tail.next = curr.left;
                    tail = tail.next;
                }
                if (curr.right != null) {
                    tail.next = curr.right;
                    tail = tail.next;
                }
                curr = curr.next;
            }
            mostLeft = head.next;
            
        }
        return root;
    }
}
```

![image-20210827175509338](https://gitee.com/cao_ziqiang/img/raw/master/20210827175509.png)

