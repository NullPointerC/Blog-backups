---
title: leetcode-116-填充每个节点的下一个右侧节点指针
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: dbe2b3ae
date: 2021-08-27 11:37:31
---

[link](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node/)

<hr/>

题目描述:

<pre>
给定一个 完美二叉树 ，其所有叶子节点都在同一层，每个父节点都有两个子节点。
二叉树定义如下：
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 NULL。
初始状态下，所有 next 指针都被设置为 NULL。
</pre>

示例:

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210827113908.png)

<pre>
输入：root = [1,2,3,4,5,6,7]
输出：[1,#,2,3,#,4,5,6,7,#]
解释：给定二叉树如图 A 所示，你的函数应该填充它的每个 next 指针，以指向其下一个右侧节点，如图 B 所示。序列化的输出按层序遍历排列，同一层节点由 next 指针连接，'#' 标志着每一层的结束。
</pre>

思路:

<pre>
实际上,就是给了我们一颗满二叉树,并且树节点中多了一个指向同层级的
相邻树节点的指针
让我们把每个树节点的next指针填充好
那么我们针对几种特殊情况考虑
如果是一个空节点,那么就无需填充next指针;
如果是同一个节点下正好有两个孩子,就让左孩子的next指针指向右孩子;
对于不在同一个节点下的同层相邻孩子,要先通过孩子的父节点的next指针找到孩子的next指针的父亲,再将孩子的next指针指向找到的父亲的子结点;
这两种情况对应下面的两图;
分析好过程后,对树进行前序遍历即可;
</pre>

![fig1](https://gitee.com/cao_ziqiang/img/raw/master/20210827115803.png)

![fig2](https://gitee.com/cao_ziqiang/img/raw/master/20210827115808.png)

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
        traversal(root);
        return root;
    }
    private void traversal(Node root) {
        if(root == null || root.left == null) {
            return;
        }
        root.left.next = root.right;
        if(root.next != null) {
            root.right.next = root.next.left;
        }
        traversal(root.left);
        traversal(root.right);
    }
}
```

![image-20210827114622649](https://gitee.com/cao_ziqiang/img/raw/master/20210827114622.png)

<pre>
当然也可以使用bfs+队列;
将每一层的节点处理,把正在处理的节点的next指向队头即可;
因为同一层的节点会按序加入到队列中,只要遍历这一层节点个数即可;
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

![image-20210827120005456](https://gitee.com/cao_ziqiang/img/raw/master/20210827120005.png)

<pre>
同样也可以对第一种解法做非递归形式的调用,减少函数调用栈
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
        while (mostLeft.left != null) {
            // 根据mostLeft.left进入到下一层的起始,当下一层有结点时
            Node head = mostLeft;
            // 遍历这一层的next链表,并更新下一层的next,head定义为每一层的起始
            while (head != null) {
                head.left.next = head.right;
                if (head.next != null) {
                    head.right.next = head.next.left;
                }
                // 指针向后移动
                head = head.next;
            }
            // 去下一层的开始
            mostLeft = mostLeft.left;
        }
        return root;
    }
}
```

![image-20210827121012904](https://gitee.com/cao_ziqiang/img/raw/master/20210827121013.png)