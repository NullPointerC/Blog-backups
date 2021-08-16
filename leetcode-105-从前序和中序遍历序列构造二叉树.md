---
title: leetcode-105-从前序和中序遍历序列构造二叉树
date: 2021-08-15 11:59:49
categories: algorithm
tags: [algorithm,Java,LeetCode]
---

[link](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

<hr/>

题目描述：

<pre>
给定一棵树的前序遍历 preorder 与中序遍历  inorder。请构造二叉树并返回其根节点。
</pre>

示例1：

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210815124840.jpeg)

<pre>
Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
Output: [3,9,20,null,null,15,7]
</pre>

示例2：

<pre>
Input: preorder = [-1], inorder = [-1]
Output: [-1]
</pre>

思路：

<pre>
首先需要知道前序遍历即-》root-》left-》right，所以前序遍历的第一个元素即为根节点，再从中序序列中找到根结点的位置，判断根结点左子树序列的长度，最后再对左右子树分别进行调用即可
</pre>

代码：

```java
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        Map<Integer, Integer> mapping = new HashMap<>();
        for (int i = 0; i < inorder.length; i++) {
            mapping.put(inorder[i], i);
        }
        return buildTree(preorder, 0, preorder.length, inorder, 0, inorder.length, mapping);
    }

    private TreeNode buildTree(int[] preorder, int preStart, int preEnd,
                               int[] inorder, int inStart, int inEnd, Map<Integer, Integer> inMap) {
        if (preStart > preEnd || inStart > inEnd) {
            return null;
        }
        // 前序遍历
        // 前序遍历的第一个节点即为根结点
        TreeNode root = new TreeNode(preorder[preStart]);
        // 找到根结点在中序序列中的索引
        int inRoot = inMap.get(root.val);
        // 左子树序列长度
        int numsLeft = inRoot - inStart;

        root.left = buildTree(preorder, preStart + 1, preStart + numsLeft,
                inorder, inStart, inRoot - 1, inMap);
        root.right = buildTree(preorder, preStart + numsLeft + 1, preEnd,
                inorder, inRoot + 1, inEnd, inMap);
        return root;
    }
}
```

![image-20210815125421105](https://gitee.com/cao_ziqiang/img/raw/master/20210815125421.png)

