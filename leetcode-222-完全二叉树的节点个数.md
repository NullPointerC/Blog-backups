---
title: leetcode-222-完全二叉树的节点个数
date: 2021-08-29 11:57:46
categories: LeetCode
tags: [algorithm,Java,LeetCode]
---

[link](https://leetcode-cn.com/problems/count-complete-tree-nodes/)

<hr/>

题目描述:

<pre>
给你一棵 完全二叉树 的根节点 root ，求出该树的节点个数。
完全二叉树 的定义如下：在完全二叉树中，除了最底层节点可能没填满外，其余每层
节点数都达到最大值，并且最下面一层的节点都集中在该层最左边的若干位置。若最
底层为第 h 层，则该层包含 1~ 2h 个节点。
</pre>

示例1:

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210829120127.png)

<pre>
输入：root = [1,2,3,4,5,6]
输出：6
</pre>

示例2:

<pre>
输入：root = []
输出：0
</pre>

示例3:

<pre>
输入：root = [1]
输出：1
</pre>

思路:

<pre>
前序遍历数,每次非空结点计数器加1即可
</pre>

代码:

```java
class Solution {
    int count = 0;

    public int countNodes(TreeNode root) {
        traversal(root);
        return count;
    }

    private void traversal(TreeNode root) {
        if (root == null) {
            return;
        }
        count++;
        traversal(root.left);
        traversal(root.right);
    }
}
```

![image-20210829120309549](https://gitee.com/cao_ziqiang/img/raw/master/20210829120309.png)

<pre>
 也可以直接dfs到底
</pre>

```java
class Solution {
	public int countNodes(TreeNode root) {
    if (root == null){
        return 0;
    }
    return countNodes(root.left) + countNodes(root.right) + 1;
	}
}
```

<pre>
这里可以对dfs进行剪枝
</pre>

```java
class Solution {
    public int countNodes(TreeNode root) {
        if(root == null) {
            return 0;
        }
        int leftDepth = getDepth(root.left);
        int rightDepth = getDepth(root.right);
        if (leftDepth == rightDepth) {
            return (1 << leftDepth) + countNodes(root.right);
        } else {// 右子树是满二叉树
            return (1 << rightDepth) + countNodes(root.left);
        }
    }

    private int getDepth(TreeNode root) {
        int depth = 0;
        while (root != null) {
            root = root.left;
            depth++;
        }
        return depth;
    }
}
```

![image-20210829144429079](https://gitee.com/cao_ziqiang/img/raw/master/20210829144429.png)