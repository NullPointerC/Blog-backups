---
title: leetcode-124-二叉树中的最大路径和
date: 2021-08-15 11:32:17
categories: LeetCode
tags: [algorithm,Java,LeetCode]
---

[link](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)

<hr/>

题目描述：

<pre>
路径 被定义为一条从树中任意节点出发，沿父节点-子节点连接，达到任意节点的序列。同一个节点在一条路径序列中 至多出现一次 。该路径 至少包含一个 节点，且不一定经过根节点。
路径和 是路径中各节点值的总和。
给你一个二叉树的根节点 root ，返回其 最大路径和 。
</pre>




示例1：

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210815113734.jpeg)



<pre>
输入：root = [1,2,3]
输出：6
解释：最优路径是 2 -> 1 -> 3 ，路径和为 2 + 1 + 3 = 6
</pre>

示例2：

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210815113755.jpeg)

<pre>
输入：root = [-10,9,20,null,null,15,7]
输出：42
解释：最优路径是 15 -> 20 -> 7 ，路径和为 15 + 20 + 7 = 42
</pre>

思路：

<pre>
这道题思路类似于是后序遍历，题目要求是最大路径和，对一个任何一个二叉树节点，先计算出左子树和右子树的最大路径和，然后加上自己的值，就得出新的最大路径和。
</pre>

代码：

```java
class Solution {
    int ans = Integer.MIN_VALUE;

    public int maxPathSum(TreeNode root) {
        oneSideMax(root);
        return ans;
    }

    private int oneSideMax(TreeNode root) {
        if (root == null) {
            return 0;
        }
        // 得到左子树的路径和
        int left = Math.max(0, oneSideMax(root.left));
        // 得到右子树的路径和
        int right = Math.max(0, oneSideMax(root.right));

        // 后序遍历
        // 并更新这条路径上的ans是否能得到更大的和
        ans = Math.max(ans, left + right + root.val);
        // 返回这条路径的最大和
        return Math.max(left, right) + root.val;
    }
}
```

![image-20210815115714184](https://gitee.com/cao_ziqiang/img/raw/master/20210815115714.png)

