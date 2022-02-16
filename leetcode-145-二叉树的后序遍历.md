---
title: leetcode-145-二叉树的后序遍历
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: c7341708
date: 2021-07-14 08:07:31
---

[link](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/)

<hr/>

题目描述：

<pre>
给定一个二叉树，返回它的 后序 遍历。
</pre>

实例：

<pre>
输入: [1,null,2,3]  
   1
    \
     2
    /
   3
输出: [3,2,1]
</pre>

思路:

<pre>
依然使用递归，注意后序遍历顺序为:左-右-根即可。
</pre>

```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> list = new LinkedList();
        postOrder(root, list);
        return list;
    }
    private void postOrder(TreeNode root, List<Integer> list) {
        if(root == null) {
            return;
        }
        postOrder(root.left, list);
        postOrder(root.right, list);
        list.add(root.val);
    }
}
```

![image-20210714081028485](https://gitee.com/cao_ziqiang/img/raw/master/20210714081028.png)

