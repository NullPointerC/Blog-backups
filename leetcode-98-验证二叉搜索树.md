---
title: leetcode-98-验证二叉搜索树
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: dbeb6db1
date: 2021-08-18 11:33:10
---

[link](https://leetcode-cn.com/problems/validate-binary-search-tree/)

<hr/>

题目描述:

<pre>
给定一个二叉树，判断其是否是一个有效的二叉搜索树。
假设一个二叉搜索树具有如下特征：
节点的左子树只包含小于当前节点的数。
节点的右子树只包含大于当前节点的数。
所有左子树和右子树自身必须也是二叉搜索树。
</pre>

示例1:

<pre>
    输入:
    2
   / \
  1   3
输出: true
</pre>



示例2:

<pre>
    输入:
    5
   / \
  1   4
     / \
    3   6
输出: false
解释: 输入为: [5,1,4,null,null,3,6]。
     根节点的值为 5 ，但是其右子节点值为 4 。
</pre>



思路:

<pre>
 利用二叉搜索树的中序遍历结果是从小到大排序的顺序即可
 如果新加入序列中的结点值比结点上一个值更小或相等,说明这不是有效的BST
</pre>

代码:

```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        List<TreeNode> inorder = new ArrayList<>();
        return traversal(root, inorder);

    }

    private boolean traversal(TreeNode root, List<TreeNode> list) {
        if (root == null) {
            return true;
        }
        boolean flag = true;
        flag &= traversal(root.left, list);
        if (list.size() == 0) {
            list.add(root);
        } else {
            if (root.val <= list.get(list.size() - 1).val) {
                flag = false;
            } else {
                list.add(root);
            }
        }
        flag &= traversal(root.right, list);
        return flag;
    }
}
```

![image-20210818113743857](https://gitee.com/cao_ziqiang/img/raw/master/20210818113744.png)

