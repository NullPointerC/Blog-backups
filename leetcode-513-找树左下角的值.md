---
title: leetcode-513-找树左下角的值
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: 505270f7
date: 2021-09-19 18:47:16
---

[$link$](https://leetcode-cn.com/problems/find-bottom-left-tree-value/)

<hr/>

![image-20210919184816813](https://gitee.com/cao_ziqiang/img/raw/master/20210919184816.png)

<hr/>

想不到更好的办法时候就试试$bfs$吧，然后选中最后一层的第一个结点的值返回即可。

```java
class Solution {
    public int findBottomLeftValue(TreeNode root) {
        Queue<TreeNode> q = new LinkedList<>();
        List<List<TreeNode>> levels = new ArrayList<>();
        int ans;
        q.offer(root);
        while(!q.isEmpty()) {
            int size = q.size();
            List<TreeNode> level = new ArrayList<>();
            for(int i = 0;i<size;i++) {
                TreeNode node = q.poll();
                if(node.left != null) {
                    q.offer(node.left);
                }
                if(node.right != null) {
                    q.offer(node.right);
                }
                level.add(node);
            }
            levels.add(level);
        }
        return levels.get(levels.size() - 1).get(0).val;
    }
}
```

如果嫌弃$bfs$太慢了，也可以用$dfs$

```java
class Solution {
    private int maxDepth = -1;
    private int value = 0;
    public int findBottomLeftValue(TreeNode root) {
        value = root.val;
        findLeftValue(root,0);
        return value;
    }

    private void findLeftValue (TreeNode root,int deep) {
        if (root == null) return;
        // 找到了更深的最左节点
        if (root.left == null && root.right == null) {
            if (deep > maxDepth) {
                value = root.val;
                maxDepth = deep;
            }
        }
        if (root.left != null) findLeftValue(root.left,deep + 1);
        if (root.right != null) findLeftValue(root.right,deep + 1);
    }
}
```

