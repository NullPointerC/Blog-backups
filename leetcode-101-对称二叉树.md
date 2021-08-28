---
title: leetcode-101-对称二叉树
date: 2021-07-14 09:25:35
categories: LeetCode
tags: [algorithm,Java,LeetCode]
---

[link](https://leetcode-cn.com/problems/symmetric-tree/)

<hr/>

题目描述：

<pre>
给定一个二叉树，检查它是否是镜像对称的。
例如，二叉树 [1,2,2,3,4,4,3] 是对称的。
    1
   / \
  2   2
 / \ / \
3  4 4  3
但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:
    1
   / \
  2   2
   \   \
   3    3
</pre>

思路:

<pre>
首先想清楚，判断对称二叉树要比较的是哪两个节点，要比较的不是左右子树节点！
对于二叉树是否对称,要比较是根结点左子树和右子树是否相互翻转得来。
</pre>

定义子树a是否和b相等，需要满足以下条件

- 两棵子树的根结点值相等
- 两棵子树的左右子树分别对称
	- a树的左子树和b树的右子树对应位置应该相等
	- a树的右子树和b数的左子树对应位置应该相等

设计一个check方法，传入待检测的两个子树的头结点a和b。

- 若a和b均为空节点，符合对称；
- 若a或者b有一个为空，不符合对称；
- 若a和b的值不相等，不符合对称；

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if (root == null) {
            return true;
        }
        //调用递归函数，比较左节点，右节点
        return dfs(root.left, root.right);
    }

    private boolean dfs(TreeNode left, TreeNode right) {
        //递归的终止条件是两个节点都为空
        //或者两个节点中有一个为空
        //或者两个节点的值不相等
        if (left == null && right == null) {
            return true;
        }
        if (left == null || right == null) {
            return false;
        }
        if (left.val != right.val) {
            return false;
        }
        //再递归的比较 左节点的左孩子 和 右节点的右孩子
        //以及比较  左节点的右孩子 和 右节点的左孩子
        return dfs(left.left, right.right) && dfs(left.right, right.left);
    }
}
```

还有一种思路，也是我最早想到的办法就是先利用层序遍历的方式，将这一层的数据保存进values临时列表中，然后再对列表二分比对，即可。其中对于空节点，我们要使用dummyNode替代之。

```java
class Solution {
    TreeNode dummyNode = new TreeNode(Integer.MIN_VALUE);
    
    public boolean isSymmetric(TreeNode root) {
        if (root == null) {
            return true;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        //按照层序遍历的方式，将每一层的节点先保存到一个临时列表中，并且保存下一层进queue中
        while (!queue.isEmpty()) {
            int size = queue.size();
            List<Integer> values = new LinkedList<>();
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                if (!node.equals(dummyNode)) {
                    queue.add(node.left != null ? node.left : dummyNode);
                    queue.add(node.right != null ? node.right : dummyNode);
                }
                values.add(node.val);
            }
            /*再使用二分来检查当前层的数据是否符合要求*/
            for (int i = 0; i < size; i++) {
                if (!values.get(i).equals(values.get(size - i - 1))) {
                    return false;
                }
            }
        }
        return true;
    }
}
```

![image-20210714103208827](https://gitee.com/cao_ziqiang/img/raw/master/20210714103208.png)

也可以将递归过程中比较先用队列来存储。即先存储下左子树的左子树，右子树的右子树，左子树的右子树与右子树的左子树。

```java
class Solution {
	public boolean isSymmetric(TreeNode root) {
        if (root == null || (root.left == null && root.right == null)) {
            return true;
        }
        //用队列保存节点
        LinkedList<TreeNode> queue = new LinkedList<TreeNode>();
        //将根节点的左右孩子放到队列中
        queue.add(root.left);
        queue.add(root.right);
        while (queue.size() > 0) {
            //从队列中取出两个节点，再比较这两个节点
            TreeNode left = queue.removeFirst();
            TreeNode right = queue.removeFirst();
            //如果两个节点都为空就继续循环，两者有一个为空就返回false
            if (left == null && right == null) {
                continue;
            }
            if (left == null || right == null) {
                return false;
            }
            if (left.val != right.val) {
                return false;
            }
            //将左节点的左孩子， 右节点的右孩子放入队列
            queue.add(left.left);
            queue.add(right.right);
            //将左节点的右孩子，右节点的左孩子放入队列
            queue.add(left.right);
            queue.add(right.left);
        }

        return true;
    }
}
```

![image-20210714103659039](https://gitee.com/cao_ziqiang/img/raw/master/20210714103659.png)

