---
title: leetcode-437-路径总和Ⅲ
date: 2021-09-05 10:47:25
categories: LeetCode
tags: [algorithm,Java,LeetCode]

---

[link](https://leetcode-cn.com/problems/path-sum-iii/)

<hr/>

tags : dfs

**题目描述:**

<pre>
给定一个二叉树的根节点 root ，和一个整数 targetSum ，求该二叉树里节点值之和等于 targetSum 的 路径 的数目。
路径 不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。
</pre>

**示例1:**

<pre>
输入：root = [10,5,-3,3,2,null,11,3,-2,null,1], targetSum = 8
输出：3
解释：和等于 8 的路径有 3 条，如图所示。
</pre>

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210905104845.jpeg)

**示例2:**

<pre>
输入：root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
输出：3
</pre>

**思路:**

<pre>
这题和前面几题的不同在于,没有要求路径从根结点出发,也没有要求路径终止于叶子节点;
所以我们需要对每个结点都以它为根开始遍历所有的可能取值
</pre>

**代码:**

```java
class Solution {

    int ans = 0;
    
    public int pathSum(TreeNode root, int targetSum) {
        dfs(root, targetSum);
        return ans;
    }

    private void dfs(TreeNode root, int targetSum) {
        if (root == null) {
            return;
        }
        traversal(root, targetSum);
        dfs(root.left, targetSum);
        dfs(root.right, targetSum);
    }

    private void traversal(TreeNode root, int targetSum) {
        if (root == null) {
            return;
        }
        if (targetSum == root.val) {
            ans++;
        }
        traversal(root.left, targetSum - root.val);
        traversal(root.right, targetSum - root.val);
    }
}
```

