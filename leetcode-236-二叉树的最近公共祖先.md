---
title: leetcode-236-二叉树的最近公共祖先
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-08-30 21:20:21
---

[link](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

<hr/>

tags(后序遍历+回溯)

题目描述:

<pre>
给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。
百度百科中最近公共祖先的定义为：“对于有根树 T 的两个节点 p、q，最近公共祖先
表示为一个节点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以
是它自己的祖先）。”
</pre>

**示例 1：**

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210830212208.png)

<pre>
输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出：3
解释：节点 5 和节点 1 的最近公共祖先是节点 3 。
</pre>

**示例 2：**

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210830212230.png)

<pre>
输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出：5
解释：节点 5 和节点 4 的最近公共祖先是节点 5 。因为根据定义最近公共祖先节点可以为节点本身。
</pre>

**示例 3：**

<pre>
输入：root = [1,2], p = 1, q = 2
输出：1
</pre>

**思路:**

<pre>
以为是找祖先结点,所以需要从子结点开始找,这时就需要回溯;
回溯最好的方式就是后续遍历;
当碰到p,q或null时,就把当前结点返回;
返回后根据返回值进行回溯,如果左右子树分别找到了p,q,说明当前结点就是祖先;
如果左子树为空,而右子树不为空,就说明右子树就是最近的祖先;
反正对左子树也是这样;
</pre>

**代码:**

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // LCA 问题
        if (root == null) {
            return root;
        }
        if (root == p || root == q) {
            return root;
        }
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if (left != null && right != null) {
            return root;
        } else if (left != null) {
            return left;
        } else if (right != null) {
            return right;
        }
        return null;
    }
}
```

![image-20210830212749439](https://gitee.com/cao_ziqiang/img/raw/master/20210830212749.png)

