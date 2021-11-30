---
title: leetcode-515-在每个树行中找最大值
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-09-19 19:10:19
---

[$link$](https://leetcode-cn.com/problems/find-largest-value-in-each-tree-row/)

<hr/>

![image-20210919191057773](https://gitee.com/cao_ziqiang/img/raw/master/20210919191057.png)

<hr/>

如果使用$bfs$，那么题目就很简单了。

```java
class Solution {
    public List<Integer> largestValues(TreeNode root) {
        if(root == null) {
            return new ArrayList<Integer>();
        }
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        List<Integer> list = new LinkedList<>();
        while(!q.isEmpty()) {
            int size = q.size();
            List<Integer> level = new ArrayList<>();
            int max = Integer.MIN_VALUE;
            for(int i = 0;i<size;i++) {
                TreeNode node = q.poll();
                max = Math.max(max, node.val);
                if(node.left != null) {
                    q.offer(node.left);
                }
                if(node.right != null) {
                    q.offer(node.right);
                }
            }
            list.add(max);
        }
        return list;
    }
}
```

