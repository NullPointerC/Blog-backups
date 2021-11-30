---
title: leetcode-559-N叉树的最大深度
date: 2021-11-21 09:49:11
categories: LeetCode
tags: [leetCode,algorithm]
---

[N 叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-n-ary-tree/)

<hr/>

![image-20211122095044467](https://gitee.com/cao_ziqiang/img/raw/master/20211122095044.png)

<hr/>

比较简单的深搜, 直接dfs到底即可。就是把二叉树的两个叉改成了遍历一趟节点的所有孩子。

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> children;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, List<Node> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
    public int maxDepth(Node root) {
        return dfs(root);
    }
    private int dfs(Node root) {
        if(root == null) {
            return 0;
        }
        int ans = 1;
        for(Node child : root.children) {
            ans = Math.max(ans, 1 + dfs(child));
        }
        return ans;
    }
}
```

Golang:

```go
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Children []*Node
 * }
 */
func dfs(root *Node) int {
    if root == nil {
        return 0
    }
    ans := 1
    for _, child := range(root.Children) {
        ans = max(ans, 1 + dfs(child))
    }
    return ans
}
func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
func maxDepth(root *Node) int {
    return dfs(root)
}
```

