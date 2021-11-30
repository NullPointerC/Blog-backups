---
title: leetcode-108-将有序数组转换为二叉搜索树
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-06-30 10:23:19
---

[传送门](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)

<hr/>

题目思想：分治，递归，构造

题目描述:

<pre>
给你一个整数数组 nums ，其中元素已经按 升序 排列，请你将其转换为一棵 高度平衡 二叉搜索树。
高度平衡 二叉树是一棵满足「每个节点的左右两个子树的高度差的绝对值不超过 1 」的二叉树。
</pre>

示例1:

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210630102452.jpeg)

<pre>
输入：nums = [-10,-3,0,5,9]
输出：[0,-3,9,-10,null,5]
解释：[0,-10,5,null,-3,null,9] 也将被视为正确答案：
</pre>

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210630102536.jpeg)

示例2:

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210630102554.jpeg)

<pre>
输入：nums = [1,3]
输出：[3,1]
解释：[1,3] 和 [3,1] 都是高度平衡二叉搜索树。
</pre>

思路:

<pre>
根据升序数组，恢复一棵高度平衡的BST🌲。
BST的中序遍历是升序的，因此本题等同于根据中序遍历的序列恢复二叉搜索树。因此我们可以以升序序列中的任一个元素作为根节点，以该元素左边的升序序列构建左子树，以该元素右边的升序序列构建右子树，这样得到的树就是一棵二叉搜索树。
又因为本题要求高度平衡，因此我们需要选择升序序列的中间元素作为根节点。
</pre>

代码：

```java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        int l = nums.length;
        if(l == 0 || null == nums) {
            return null;
        }
        /*数组索引最后一个位置为l-1*/
        return build(nums,0,l - 1);
    }
    public TreeNode build(int[] nums, int left, int right) {
        if(left > right) {
            return null;
        }
        /*取升序数组中间元素作为根结点*/
        /*这样取mid防止爆int*/
        int mid = left + (right - left) / 2;
        TreeNode ret = new TreeNode(nums[mid]);
        /*递归构建左子树和右子树*/
        ret.left = build(nums,left,mid - 1);
        ret.right = build(nums, mid + 1,right);
        return ret;
    }
}
```

![image-20210630102740165](https://gitee.com/cao_ziqiang/img/raw/master/20210630102740.png)

