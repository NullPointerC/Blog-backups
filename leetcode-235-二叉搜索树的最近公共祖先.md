---
title: leetcode-235-二叉搜索树的最近公共祖先
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-08-17 19:33:34
---

**[link](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)**

<hr/>

题目描述:

<pre>
给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。
百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”
例如，给定如下二叉搜索树:  root = [6,2,8,0,4,7,9,null,null,3,5]
</pre>

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210817193447.png)

示例1:

<pre>
输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
输出: 6 
解释: 节点 2 和节点 8 的最近公共祖先是 6。
</pre>

示例2:

<pre>
输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
输出: 2
解释: 节点 2 和节点 4 的最近公共祖先是 2, 因为根据定义最近公共祖先节点可以为节点本身。
</pre>
思路:

<pre>
    这里可以利用二叉搜索树的特性,如果当前根节点比给出的二个结点的值都更大
    说明应该去当前根的左子树中找.
   	如果当前根节点的值比给出的两个结点的值都更小,说明应该去根的右子树中找
   	如果一个在左一个在右,说明root就是最近的公共祖先
</pre>

代码:

```java
class Solution {
    TreeNode ans = null;

    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        traversal(root, p, q);
        return ans;
    }


    private void traversal(TreeNode root, TreeNode p, TreeNode q) {
        // 这里是为了避免判断p,q的大小问题,所以使用乘法
        if ((root.val - p.val) * (root.val - q.val) <= 0) {
            ans = root;
        } else if (root.val < p.val && root.val < q.val) {
            traversal(root.right, p, q);
        } else {
            traversal(root.left, p, q);
        }
    }
}
```

![image-20210817194351456](https://gitee.com/cao_ziqiang/img/raw/master/20210817194351.png)

