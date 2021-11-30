---
title: leetcode-617-合并二叉树
date: 2021-11-07 18:50:05
categories: LeetCode
tags: [LeetCode,algorithm]
---

[$link$](https://leetcode-cn.com/problems/merge-two-binary-trees/)

<hr/>

![image-20211107185052651](https://gitee.com/cao_ziqiang/img/raw/master/20211107185052.png)

<hr/>

思路还是比较容易想到，深搜即可，如果两个节点有任意一个不为空，那么就计算相同位置的和，空节点用0替代，都为空则返回$null$。

```java
class Solution {
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        TreeNode root = dfs(root1, root2);
        return root;
    }

    private TreeNode dfs(TreeNode root1, TreeNode root2) {
        TreeNode node = new TreeNode();
        int val1 = root1 == null ? 0 : root1.val;
        int val2 = root2 == null ? 0 : root2.val;
        if (root1 != null || root2 != null) {
            node.val = val1 + val2;
        } else {
            return null;
        }
        if(root1 == null && root2 != null) {
            node.left = dfs(null, root2.left);
            node.right = dfs(null, root2.right);
        }
        if(root1 != null && root2 == null) {
            node.left = dfs(root1.left, null);
            node.right = dfs(root1.right, null);
        }
        if(root1 != null && root2 != null) {
            node.left = dfs(root1.left, root2.left);
            node.right = dfs(root1.right, root2.right); 
        }
        return node;
    }
}
```

看到了评论区大佬的代码，才知道自己的代码多么丑陋。。。

```java
class Solution {
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        TreeNode root = dfs(root1, root2);
        return root;
    }
    private TreeNode dfs(TreeNode root1, TreeNode root2) {
        if (root2 == null) {
            return root1;
        }
        if (root1 == null) {
            return root2;
        }
        TreeNode node = new TreeNode(root1.val + root2.val);
        node.left = dfs(root1.left, root2.left);
        node.right = dfs(root1.right, root2.right);
        return node;
    }
}
```

补一个Go的

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func mergeTrees(root1 *TreeNode, root2 *TreeNode) *TreeNode {
    root := dfs(root1, root2);
    return root
}

func dfs(root1 *TreeNode, root2 *TreeNode) *TreeNode {
    if root1 == nil {
        return root2
    }
    if root2 == nil {
        return root1
    }
    node := &TreeNode{}
    node.Val = root1.Val + root2.Val
    node.Left = dfs(root1.Left, root2.Left)
    node.Right = dfs(root1.Right, root2.Right)
    return node
}
```

