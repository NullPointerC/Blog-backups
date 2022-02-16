---
title: leetcode-1609-奇偶树
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: 3864c21e
date: 2021-12-25 10:45:13
---

祝大伙圣诞快乐。![img](https://gitee.com/cao_ziqiang/img/raw/master/20211225104654.jpg)

[1609. 奇偶树](https://leetcode-cn.com/problems/even-odd-tree/)

![image-20211225104550061](https://gitee.com/cao_ziqiang/img/raw/master/20211225104550.png)

<hr/>

tags(二叉树、bfs)

$bfs$即可。

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
    public boolean isEvenOddTree(TreeNode root) {
        if(root == null) {
            return true;
        }
        Queue<TreeNode> q = new ArrayDeque<>();
        q.offer(root);
        int level = 0;
        if(root.val % 2 == 0) {
            return false;
        }
        while(!q.isEmpty()) {
            TreeNode prev, curr;
            if(level % 2 == 0) {
                prev = new TreeNode(Integer.MIN_VALUE);     
            } else {
                prev = new TreeNode(Integer.MAX_VALUE);
            }
            int size = q.size();
            for(int i = 0; i < size; i++) {
                curr = q.poll();
                if(level % 2 == 0 && (curr.val % 2 == 0 || curr.val <= prev.val)) {
                    return false;
                }
                if(level % 2 == 1 && (curr.val % 2 == 1 || curr.val >= prev.val)) {
                    return false;
                }
                prev = curr;
                if(curr.left != null) {
                    q.offer(curr.left);
                }
                if(curr.right != null) {
                    q.offer(curr.right);
                }
            }
            level++;
        }
        return true;
    }
}
```

