---
title: leetcode-144-二叉树的前序遍历
date: 2021-07-14 07:57:41
categories: LeetCode
tags: [algorithm,Java,LeetCode]
---

[link](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)

<hr/>

题目描述:

<pre>
给你二叉树的根节点 root ，返回它节点值的 前序 遍历。
</pre>

示例1:

<pre>
输入：root = [1,null,2,3]
输出：[1,2,3]
</pre>

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210714075842.jpeg)

示例2:

<pre>
输入：root = []
输出：[]
</pre>

示例3:

<pre>
输入：root = [1]
输出：[1]
</pre>

示例4:

<pre>
输入：root = [1,2]
输出：[1,2]
</pre>

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210714075923.jpeg)

示例5:

<pre>
输入：root = [1,null,2]
输出：[1,2]
</pre>

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210714075938.jpeg)

思路:

<pre>
利用树结构天然的递归性质,写一个辅助方法。传入list和根结点root，按照根-左-右的顺序添加值进入list
</pre>

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        if(root == null) {
            return null;
        }
        List<Integer> list = new LinkedList();
        preOrder(root,list);
        return list;
    }
    private void preOrder(TreeNode root, List<Integer> list) {
        if (root == null) {
            return;
        }
        // 以下即为前序遍历的顺序,先根结点,再左子树,再右子树
        list.add(root.val);
        preOrder(root.left, list);
        preOrder(root.right, list);
    }
}
```

![image-20210714080344599](https://gitee.com/cao_ziqiang/img/raw/master/20210714080344.png)

