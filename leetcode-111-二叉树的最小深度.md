---
title: leetcode-111-二叉树的最小深度
date: 2021-07-01 13:04:36
categories: LeetCode
tags: [algorithm,Java,LeetCode]
---

[<font color='blue'>传送门</font>](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/submissions/)

<hr/>

好的,我不想复习,又来刷题了![img](https://gitee.com/cao_ziqiang/img/raw/master/20210701130631.jpg)

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210701130644.jpg)

<hr/>

题目描述:

<pre>
给定一个二叉树，找出其最小深度。
最小深度是从根节点到最近叶子节点的最短路径上的节点数量。
说明：叶子节点是指没有子节点的节点。
</pre>

示例1:

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210701130806.jpeg)

<pre>
输入：root = [3,9,20,null,null,15,7]
输出：2
</pre>

示例2:

<pre>
输入：root = [2,null,3,null,4,null,5,null,6]
输出：5
</pre>

思路:

<pre>
这题和求最大深度是一样的意思,不过就是在递归之前需要看这个二叉树是没有左子树还是没有右子树
因为是求最小深度,但是也要递归到最后一个叶子节点,所以如果左子树为空,那么遍历右子树即可
如果右子树为空,那么遍历左子树即可,如果左右子树都为空,那么同时遍历左子树和右子树,取更小的那个即可
</pre>

代码:

```java
class Solution {
    public int minDepth(TreeNode root) {
        return dfs(root);
    }
    private int dfs(TreeNode root) {
        int ret = 0;
        if(root == null) {
            /*为空则是0*/
            return ret;
        }
        if(root.left != null && root.right != null) {
            ret  = Math.min(dfs(root.left),dfs(root.right));
        }

        if(root.left == null) {
            ret = dfs(root.right);
        }else if(root.right == null){
            ret = dfs(root.left);
        }else {
            ret = Math.min(dfs(root.left),dfs(root.right));
        }
        return 1 + ret;
    }
}
```

![image-20210701131121022](https://gitee.com/cao_ziqiang/img/raw/master/20210701131121.png)

