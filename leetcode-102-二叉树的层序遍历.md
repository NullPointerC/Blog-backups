---
title: leetcode-102-二叉树的层序遍历
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-07-14 08:38:58
---

[link](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

<hr/>

题目描述：

<pre>
给你一个二叉树，请你返回其按 层序遍历 得到的节点值。 （即逐层地，从左到右访问所有节点）。
示例：
二叉树：[3,9,20,null,null,15,7],
    3
   / \
  9  20
    /  \
   15   7
返回其层序遍历结果：
[
  [3],
  [9,20],
  [15,7]
]
</pre>

思路：

<pre>
结合bfs，可以发现bfs的遍历顺序正是层序遍历的顺序。
但是按照层序遍历的顺序的结果和bfs遍历的结果是不一样的，层序遍历要求分层，返回一个二维数组。
这样我们就可以先在每一层遍历开始之前，先记录下队列中节点的数量n,然后将这n个节点依次处理。
</pre>

代码：

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();

        Queue<TreeNode> queue = new ArrayDeque<>();
        if (root != null) {
            queue.add(root);
        }
        while (!queue.isEmpty()) {
            int n = queue.size();
            List<Integer> level = new ArrayList<>();
            for (int i = 0; i < n; i++) {
                TreeNode node = queue.poll();
                level.add(node.val);
                if (node.left != null) {
                    queue.add(node.left);
                }
                if (node.right != null) {
                    queue.add(node.right);
                }
            }
            res.add(level);
        }
        return res;
    }
}
```

![image-20210714091520124](https://gitee.com/cao_ziqiang/img/raw/master/20210714091520.png)