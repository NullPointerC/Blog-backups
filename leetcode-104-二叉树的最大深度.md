---
title: leetcode-104-二叉树的最大深度
date: 2021-07-14 09:21:42
categories: LeetCode
tags: [algorithm,Java,LeetCode]
---

[link](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

<hr/>

题目描述：

<pre>
给定一个二叉树，找出其最大深度。
二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。
说明: 叶子节点是指没有子节点的节点。
示例：
给定二叉树 [3,9,20,null,null,15,7]，
   3
   / \
  9  20
    /  \
   15   7
返回它的最大深度 3 。
</pre> 
思路：

<pre>
根结点为空，则直接返回0，左子树空，遍历右子树，右子树空，遍历左子树。都不空，则比较哪个深度更大。
</pre>

代码：

```java
class Solution {
    public int maxDepth(TreeNode root) {
        return dfs(root);
    }

    private int dfs(TreeNode root) {
        int ret = 0;
        if (root == null) {
            return ret;
        }
        if (root.left == null) {
            ret = dfs(root.right);
        } else if (root.right == null) {
            ret = dfs(root.left);
        } else {
            ret = Math.max(dfs(root.left), dfs(root.right));
        }
        return 1 + ret;
    }
}
```

![image-20210714092510410](https://gitee.com/cao_ziqiang/img/raw/master/20210714092510.png)

