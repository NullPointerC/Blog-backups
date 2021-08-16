---
title: leetcode-99-恢复二叉搜索树
date: 2021-08-15 12:55:56
categories: algorithm
tags: [algorithm,Java,LeetCode]
---

[link](https://leetcode-cn.com/problems/recover-binary-search-tree/)

<hr/>

题目描述：

<pre>
给你二叉搜索树的根节点 root ，该树中的两个节点被错误地交换。请在不改变其结构的情况下，恢复这棵树。
进阶：使用 O(n) 空间复杂度的解法很容易实现。你能想出一个只使用常数空间的解决方案吗？
</pre>




**示例 1：**

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210815131929.jpeg)

<pre>
输入：root = [1,3,null,null,2]
输出：[3,1,null,null,2]
解释：3 不能是 1 左孩子，因为 3 > 1 。交换 1 和 3 使二叉搜索树有效。
</pre>

示例2：

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210815132017.jpeg)

<pre>
<pre>
输入：root = [3,1,4,null,null,2]
输出：[2,1,4,null,null,3]
解释：2 不能在 3 的右子树中，因为 2 < 3 。交换 2 和 3 使二叉搜索树有效。
</pre>

思路:

<pre>
对于一棵二叉搜索树(BST),它的中序遍历结果就是从大小的结果。所以在中序遍历过程中，我们记录错误两个错误排序节点，最后进行交换。并且还需要记录prev上一个遍历的节点
</pre>

代码：

```java
class Solution {
    TreeNode prev, s, t;

    public void recoverTree(TreeNode root) {
        traverse(root);
        int tmep = s.val;
        s.val = t.val;
        t.val = tmep;
    }

    private void traverse(TreeNode node) {
        if (node == null) {
            return;
        }

        traverse(node.left);

        // 中序遍历
        if (prev != null && node.val < prev.val) {
            s = (s == null) ? prev : s;
            t = node;
        }
        //为下层递归设置前节点
        prev = node;

        traverse(node.right);
    }
}
```

​	![image-20210815133227349](https://gitee.com/cao_ziqiang/img/raw/master/20210815133227.png)

