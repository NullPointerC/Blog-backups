---
title: leetcode-429-N叉树的层序遍历
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-09-05 09:59:05
---

[link](https://leetcode-cn.com/problems/n-ary-tree-level-order-traversal/)

<hr/>

tags: (bfs)

**题目描述:**

<pre>
给定一个 N 叉树，返回其节点值的层序遍历。（即从左到右，逐层遍历）。
树的序列化输入是用层序遍历，每组子节点都由 null 值分隔（参见示例）。
</pre>

**示例1:**

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210905100004.png)

<pre>
输入：root = [1,null,3,2,4,null,5,6]
输出：[[1],[3,2,4],[5,6]]
</pre>

**示例2:**

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210905100032.png)

<pre>
输入：root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
输出：[[1],[2,3,4,5],[6,7,8,9,10],[11,12,13],[14]]
</pre>

**思路:**

<pre>
比较简单的bfs,将二叉的条件改为每个subNode即可
</pre>

**代码:**

```java
class Solution {
    public List<List<Integer>> levelOrder(Node root) {
        List<List<Integer>> ans = new LinkedList<>();
        if (root == null) {
            return ans;
        }
        Queue<Node> queue = new ArrayDeque<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            List<Integer> level = new LinkedList<>();
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                Node node = queue.poll();
                level.add(node.val);
                for (Node subNode : node.children) {
                    queue.offer(subNode);
                }
            }
            ans.add(level);
        }
        return ans;
    }
}
```

**dfs代码:**

```java
class Solution {

    private List<List<Integer>> ans = new ArrayList<>();
    public List<List<Integer>> levelOrder(Node root) {
        traversal(root, 0);
        return ans;
    }

    private void traversal(Node root, int level) {
        if (root == null) {
            return;
        }
        if (ans.size() == level) {
            List<Integer> item = new LinkedList<>();
            ans.add(item);
        }
        ans.get(level).add(root.val);
        for (Node child : root.children) {
            traversal(child, level + 1);
        }
    }
}
```

![image-20210905102823603](https://gitee.com/cao_ziqiang/img/raw/master/20210905102823.png)

