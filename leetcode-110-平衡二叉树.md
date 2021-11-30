---
title: leetcode-110-平衡二叉树
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-07-01 12:28:48
---

[<font color="blue">传送门</font>](https://leetcode-cn.com/problems/balanced-binary-tree/submissions/)

<hr/>

题目描述:

<pre>
    给定一个二叉树，判断它是否是高度平衡的二叉树。
    本题中，一棵高度平衡二叉树定义为：
    一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1 。
</pre>

示例1:

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210701123122.jpeg)

<pre>
    输入：root = [3,9,20,null,null,15,7]
	输出：true
</pre>

示例2:

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210701123148.jpeg)

<pre>
    输入：root = [1,2,2,3,3,null,null,4,4]
	输出：false
</pre>

示例3:

<pre>
    输入：root = []
	输出：true
</pre>

思路:

<pre>
    首先计算出每个结点的高度,然后给出根结点的平衡因子,最后递归判断左子树和右子树即可
</pre>

代码:

```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        return dfs(root);
    }
    private boolean dfs(TreeNode root) {
        if(root == null) {
            /*如果是空树,则一定是平衡二叉树*/
            return true;
        }
        //获取当前节点的平衡因子
        int balanceFactor = getBalanceFactor(root);
        if(balanceFactor > 1) {
            return false;
        }
        return isBalanced(root.left) && isBalanced(root.right);
    }
    private int getHeight(TreeNode root) {
        int ret = 0;
        if(root == null) {
            return  ret;
        }
        if(root.left != null || root.right != null) {
            ret = Math.max(getHeight(root.left),getHeight(root.right));
        }
        return 1 + ret;
    }
    private int getBalanceFactor(TreeNode root) {
        if(root == null) {
            return 0;
        }
        return Math.abs(getHeight(root.left) - getHeight(root.right));
    }
}
```

![image-20210701123417895](https://gitee.com/cao_ziqiang/img/raw/master/20210701123417.png)

