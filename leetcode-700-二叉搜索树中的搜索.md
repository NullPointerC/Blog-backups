---
title: leetcode-700-二叉搜索树中的搜索
date: 2021-11-26 10:09:16
categories: LeetCode
tags: [LeetCode,algorithm]
---

[二叉搜索树中的搜索](https://leetcode-cn.com/problems/search-in-a-binary-search-tree/)

<hr/>

![image-20211126100954209](https://gitee.com/cao_ziqiang/img/raw/master/20211126100954.png)

<hr/>

比较简单的搜索,利用BST的性质即可。

$root$为空时退出递归搜索，$root.val=val$时说明找到了，$ans$赋值退出即可。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    TreeNode ans = null;
    public TreeNode searchBST(TreeNode root, int val) {
        dfs(root, val);
        return ans;
    }
    private void dfs(TreeNode root, int val) {
        if (root == null) {
            return ;
        }
        if(root.val == val) {
            ans = root;
            return ;
        } else if (root.val < val){
            dfs(root.right,val);
        } else {
            dfs(root.left, val);
        }
    }
}
```

Golang:

```go
 //递归法
func searchBST(root *TreeNode, val int) *TreeNode {
    if root==nil||root.Val==val{
        return root
    }
    if root.Val>val{
        return searchBST(root.Left,val)
    }
    return searchBST(root.Right,val)
}
```

plus(随想：2021-11-26)：

最近也是因为一些小事，接连几天心情都不太好，也不知道怎么排解，题目也好几天没有好好写，以为退任了可以休息，带学弟也是一件很重要的事，而且还有其他小事烦神。。。在这种状态下，不能确定自己的水平到底是哪一个层次，只能埋头刷题，每天都忙着写好几个题，也不知道自己是不是真的学进去了，焦虑的心情也不好和身边的朋友说，等会万一只是我的焦虑，却变成了一群人的焦虑。。。慢慢排解吧，刷题也可以放慢一些，重质量，不要追求数量！

项目也暂时不着急吧，等再把基础巩固一下。