---
title: leetcode-107-二叉树的层序遍历Ⅱ
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-08-26 12:00:06
---

[link](https://leetcode-cn.com/problems/binary-tree-level-order-traversal-ii/)

<hr/>

(tags: bfs+queue)

题目描述：

<pre>
给定一个二叉树，返回其节点值自底向上的层序遍历。 （即按从叶子节点所在层到根节点所在的层，逐层从左向右遍历）
例如：给定二叉树 [3,9,20,null,null,15,7],
	3
   / \
  9  20
    /  \
   15   7
返回其自底向上的层序遍历为：
[
  [15,7],
  [9,20],
  [3]
]
</pre>

思路：

<pre>
bfs+队列即可。
</pre>

代码：

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
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();

        Queue<TreeNode> queue = new ArrayDeque<>();
        // 首先加入根结点
        if (root != null) {
            queue.add(root);
        }
        while (!queue.isEmpty()) {
            int n = queue.size();
            // 统计每一层的节点
            List<Integer> level = new ArrayList<>();
            for (int i = 0; i < n; i++) {
                // 正在遍历的节点出队
                TreeNode node = queue.poll();
                level.add(node.val);
                // 其左右子树入队
                if (node.left != null) {
                    queue.offer(node.left);
                }
                if (node.right != null) {
                    queue.offer(node.right);
                }
            }
            res.add(level);
        }
        Collections.reverse(res);
        return res;
    }
}
```

![image-20210826121851017](https://gitee.com/cao_ziqiang/img/raw/master/20210826121851.png)



<pre>
当然，如果认为广搜太慢了，也可以使用深搜+depth来优化，也可以达到bfs的效果
</pre>

```java
class Solution {
    List<List<Integer>> nodePool = new ArrayList<>();
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        if(root == null) return nodePool;
        traverse(root,0);
        Collections.reverse(nodePool);
        return nodePool;
    }
    private void traverse(TreeNode root, int level){
        if(root == null) return;
        if(nodePool.size()-1 < level )
            nodePool.add(new ArrayList<Integer>());
        nodePool.get(level).add(root.val);
        traverse(root.left, level+1);
        traverse(root.right, level+1);
    }
}
```

![image-20210826150524733](https://gitee.com/cao_ziqiang/img/raw/master/20210826150524.png)

