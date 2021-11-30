---
title: leetcode-129-求根节点到叶节点数字之和
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-08-25 15:40:08
---



[link](https://leetcode-cn.com/problems/sum-root-to-leaf-numbers/)

<hr/>

给你一个二叉树的根节点 root ，树中每个节点都存放有一个 0 到 9 之间的数字。
每条从根节点到叶节点的路径都代表一个数字：

例如，从根节点到叶节点的路径 1 -> 2 -> 3 表示数字 123 。
计算从根节点到叶节点生成的 所有数字之和 。

叶节点 是指没有子节点的节点。

示例1:

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210825154514.jpeg)

<pre>
输入：root = [1,2,3]
输出：25
解释：
从根到叶子节点路径 1->2 代表数字 12
从根到叶子节点路径 1->3 代表数字 13
因此，数字总和 = 12 + 13 = 25
</pre>

示例2:

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210825154541.jpeg)

<pre>
输入：root = [4,9,0,5,1]
输出：1026
解释：
从根到叶子节点路径 4->9->5 代表数字 495
从根到叶子节点路径 4->9->1 代表数字 491
从根到叶子节点路径 4->0 代表数字 40
因此，数字总和 = 495 + 491 + 40 = 1026
</pre>

思路:

<pre>
深搜就完事了!
搜到叶子节点就回溯
</pre>

代码:

```java
class Solution {
    public int sumNumbers(TreeNode root) {
        List<String> ans = new LinkedList<>();
        StringBuilder sb = new StringBuilder();
        traversal(root, ans, sb);
        //System.out.println(ans);
        int sum = 0;
        for (int i = 0; i < ans.size(); i++) {
            sum += Integer.parseInt(ans.get(i));
        }
        return sum;
    }

    private void traversal(TreeNode root, List<String> list, StringBuilder sb) {
        if (root != null) {
            sb.append(root.val);
        }
        if (root.left == null && root.right == null) {
            list.add(sb.toString());
            return;
        }
        if (root.left != null) {
            traversal(root.left, list, sb);
            sb = sb.deleteCharAt(sb.length() - 1);
        }
        if (root.right != null) {
            traversal(root.right, list, sb);
            sb = sb.deleteCharAt(sb.length() - 1);
        }

    }
}
```

<pre>
 当然也可以使用bfs
</pre>

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
    Queue<TreeNode> queue = new ArrayDeque<>();
    public int sumNumbers(TreeNode root) {
        return bfs(root);
    }

    private int bfs(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int ans = 0;
        queue.offer(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode cur = queue.poll();
                if (cur.left == null && cur.right == null) {
                    // 碰到叶子节点直接计算进ans
                    ans += cur.val;
                }
                if (cur.left != null) {
                    //把原有节点*10加到儿子节点上
                    cur.left.val += cur.val * 10;
                    queue.offer(cur.left);
                }
                if (cur.right != null) {
                    cur.right.val += cur.val * 10;
                    queue.offer(cur.right);
                }
            }
        }
        return ans;
    }
}
```

![image-20210825163338097](https://gitee.com/cao_ziqiang/img/raw/master/20210825163338.png)

