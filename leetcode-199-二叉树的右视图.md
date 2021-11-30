---
title: leetcode-199-二叉树的右视图
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-08-29 11:14:33
---

[link](https://leetcode-cn.com/problems/binary-tree-right-side-view/)

<hr/>

题目描述:

<pre>
给定一个二叉树的 根节点 root，想象自己站在它的右侧，
按照从顶部到底部的顺序，返回从右侧所能看到的节点值。
</pre>

**示例 1:**

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210829111538.jpeg)

<pre>
输入: [1,2,3,null,5,null,4]
输出: [1,3,4]
</pre>

**示例 2:**

<pre>
输入: [1,null,3]
输出: [1,3]
</pre>

**示例 3:**

<pre>
输入: []
输出: []
</pre>

思路:

<pre>
刚看到的时候以为直接right遍历到底就行,后来发现,会有右子树为空,这时右视图在左子树上的情况;
由于想不到什么好的办法,就搜吧,bfs搜到底,每次取peekLast()
</pre>

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
    public List<Integer> rightSideView(TreeNode root) {
        if (root == null) {
            return new LinkedList<>();
        }
        Deque<TreeNode> deque = new ArrayDeque<>();
        deque.add(root);
        LinkedList<Integer> ans = new LinkedList<>();
        while (!deque.isEmpty()) {
            ans.add(deque.peekLast().val);
            int size = deque.size();
            for (int i = 0; i < size; i++) {
                TreeNode node = deque.poll();
                if (node.left != null) {
                    deque.offer(node.left);
                }
                if (node.right != null) {
                    deque.offer(node.right);
                }
            }
        }
        return ans;
    }
}
```

![image-20210829111813359](https://gitee.com/cao_ziqiang/img/raw/master/20210829111813.png)

<pre>
去评论区看到了大佬写的dfs,跟->右->左,和前序遍历反一下就可以确保每层都找到最右边的结点
</pre>

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
    private LinkedList<Integer> ans = new LinkedList<>();
    public List<Integer> rightSideView(TreeNode root) {
            if (root == null) {
                return ans;
            }
            traversal(root, 0);
            return ans;
        }

        private void traversal(TreeNode root, int depth) {
            if (root == null) {
                return;
            }
            if (depth == ans.size()) {
                ans.add(root.val);
            }
            depth++;
            traversal(root.right, depth);
            traversal(root.left, depth);
        }
}
```

<pre>
用一个depth表示遍历的深度,当到了depth的深度后,将最右边的结点加入即可
</pre>

![image-20210829113024762](https://gitee.com/cao_ziqiang/img/raw/master/20210829113024.png)

