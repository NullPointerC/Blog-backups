---
title: leetcode-226-翻转二叉树
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-07-15 21:03:43
---

[Link](https://leetcode-cn.com/problems/invert-binary-tree/)

<hr/>

## 题目描述:

<pre>
翻转一棵二叉树。
</pre>

## 示例:

<pre>
     4
   /   \
  2     7
 / \   / \
1   3 6   9
</pre>

## 输出:

<pre>
     4
   /   \
  7     2
 / \   / \
9   6 3   1
</pre>

## 思路:

<pre>
使用递归无疑是最简单的,对于非空结点,将其左右子树交换即可,然后再对其左右子树执行上述操作即可。
</pre>

## 代码：

```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        return dfs(root);
    }

    private TreeNode dfs(TreeNode root) {
        if (root == null) {
            return null;
        }
        TreeNode tmp = root.left;
        root.left = root.right;
        root.right = tmp;
        dfs(root.left);
        dfs(root.right);
        return root;
    }
}
```

![image-20210715210624096](https://gitee.com/cao_ziqiang/img/raw/master/20210715210624.png)

