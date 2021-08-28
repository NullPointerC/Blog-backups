---
title: leetcode-114-二叉树展开为链表
date: 2021-08-22 10:46:30
categories: LeetCode
tags: [algorithm,Java,LeetCode]
---

[link](https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list/submissions/)

<hr/>

（tag：前序遍历、递归）

题目描述：

<pre>
给你二叉树的根结点 root ，请你将它展开为一个单链表：
展开后的单链表应该同样使用 TreeNode ，其中 right 子指针指向链表中下一个结点，而左子指针始终为 null 。
展开后的单链表应该与二叉树 先序遍历 顺序相同。
</pre>

示例1：
![img](https://gitee.com/cao_ziqiang/img/raw/master/20210822104750.jpeg)

<pre>
输入：root = [1,2,5,3,4,null,6]
输出：[1,null,2,null,3,null,4,null,5,null,6]
</pre>

示例2：

<pre>
输入：root = []
输出：[]
</pre>

示例3：

<pre>
输入：root = [0]
输出：[0]
</pre>

思路：

<pre>
题目讲到展开后的单链表要和二叉树的先序遍历顺序相同，所以我们采用先序遍历该树。
每次使用一个cur指针指向上一个节点，找到非空节点时，将cur指针的right指向当前结点，再将cur指针置当前根结点，再将当前的左子树置为空。
</pre>

代码：

```java
class Solution {
    TreeNode cur = new TreeNode();

    public void flatten(TreeNode root) {
        if (root == null) {
            return;
        }
        
        TreeNode left = root.left;
        TreeNode right = root.right;
        
        cur.right = root;
        cur = root;
        root.left = null;
        
        flatten(left);
        flatten(right);
    }
}
```

![image-20210822105227298](https://gitee.com/cao_ziqiang/img/raw/master/20210822105227.png)

