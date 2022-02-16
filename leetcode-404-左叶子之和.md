---
title: leetcode-404-左叶子之和
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: c30336e7
date: 2021-09-03 11:44:22
---

[link](https://leetcode-cn.com/problems/sum-of-left-leaves/)

<hr/>

tags: (dfs)

**题目描述:**

<pre>
计算给定二叉树的所有左叶子之和。
示例：
	3
   / \
  9  20
    /  \
   15   7
在这个二叉树中，有两个左叶子，分别是 9 和 15，所以返回 24
</pre>

**思路:**

<pre>
使用深搜搜索这棵树,但是要注意每次遍历到左子树不为空,但是左子树的子孙为空时,记录该节点
</pre>

**代码:**

```java
class Solution {
    int ans = 0;
    public int sumOfLeftLeaves(TreeNode root) {
        if (root == null) {
            return 0;
        }
        traversal(root);
        return ans;
    }

    private void traversal(TreeNode root) {
        if (root == null) {
            return;
        }
        if (root.left != null && root.left.left == null && root.left.right == null) {
            ans += root.left.val;
        }
        traversal(root.left);
        traversal(root.right);
    }
}
```

![image-20210903114739067](https://gitee.com/cao_ziqiang/img/raw/master/20210903114739.png)

<pre>
一开始想用bfs的,但是不知道怎么用bfs来记录是第一个叶子节点,每一层的关系太难捋清楚了,还是dfs大法好
</pre>

