---
title: leetcode-257-二叉树的所有路径
date: 2021-08-30 21:36:42
categories: LeetCode
tags: [algorithm,Java,LeetCode]
---

[link](https://leetcode-cn.com/problems/binary-tree-paths/)

<hr/>

**题目描述:**

<pre>
给你一个二叉树的根节点 root ，按 任意顺序 ，返回所有从根节点到叶子节点的路
径。
叶子节点 是指没有子节点的节点。
</pre>

**示例1:**

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210830213748.jpeg)

<pre>
输入：root = [1,2,3,null,5]
输出：["1->2->5","1->3"]
</pre>

**示例 2：**

<pre>
输入：root = [1]
输出：["1"]
</pre>

**思路:**

<pre>
前序遍历加回溯即可,遇到空节点直接return,遇到叶子节点,用字符串加上当前值并
且加入到list中;
</pre>

**代码:**

```java
class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> ans = new LinkedList<>();
        traversal(root, ans, "");
        return ans;
    }

    private void traversal(TreeNode root, List<String> list, String str) {
        if (root == null) {
            return;
        }
        if (root.left == null && root.right == null) {
            str += root.val;
            list.add(str);
            return;
        } else {
            str += root.val + "->";
        }
        traversal(root.left, list, str);
        traversal(root.right, list, str);
    }
}
```

![image-20210830215155468](https://gitee.com/cao_ziqiang/img/raw/master/20210830215155.png)

<pre>
也开始使用StringBuilder来加快字符串的拼接,因为直接用加号拼是O(n)的复杂
度,确实比较慢;
</pre>

