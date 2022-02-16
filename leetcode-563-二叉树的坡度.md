---
title: leetcode-563-二叉树的坡度
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: c88b9bd0
date: 2021-11-18 16:09:49
---

[$link$](https://leetcode-cn.com/problems/binary-tree-tilt/)

<hr/>

![image-20211118161059891](https://gitee.com/cao_ziqiang/img/raw/master/20211118161059.png)

<hr/>

tags: (dfs、二叉树)

可以比较容易想到的是$dfs$内再套$dfs$，外层的$dfs$遍历每个节点，计算每个节点的坡度。

内层的$dfs$遍历每个节点，计算每个节点的左子树节点之和和右子树节点之和。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    private int ans = 0;
    public int findTilt(TreeNode root) {
        dfs(root);
        return ans;
    }
    private void dfs(TreeNode node) {
        if(node == null) {
            return ;
        }
        this.ans += Math.abs(getSum(node.left) - getSum(node.right));
        dfs(node.left);
        dfs(node.right);
    }
    private int getSum(TreeNode node) {
        if (node == null) {
            return 0;
        }
        return node.val + getSum(node.left) + getSum(node.right);
    }
}
```

也可以在遍历的时候边求和边计算坡度。

Golang:

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func findTilt(root *TreeNode) (ans int) {
    var dfs func(*TreeNode) int
    dfs = func(node *TreeNode) int {
        if node == nil {
            return 0
        }
        sumLeft, sumRight := dfs(node.Left), dfs(node.Right)
        ans += abs(sumLeft - sumRight)
        return sumLeft + sumRight + node.Val
    }
    dfs(root)
    return
}

func abs(x int) int {
    if x < 0 {
        return -x
    }
    return x
}
```

