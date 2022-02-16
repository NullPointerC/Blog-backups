---
title: leetcode-94-二叉树的中序遍历
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: 3acd2e71
date: 2021-06-11 20:54:43
---

<a href="https://leetcode-cn.com/problems/binary-tree-inorder-traversal/submissions/" style="color:red;text-decoration:none">传送门</a>

<hr/>

![image-20210611205604800](https://gitee.com/cao_ziqiang/img/raw/master/20210611205604.png)

思路:

中序遍历的顺序为:左子树	->	根结点	->	右子树

而且因为二叉树天生的递归定义,所以使用递归可以非常简单的求解出问题

代码:

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
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        inorder(root, res);
        return res;
    }
    public void inorder(TreeNode root, List<Integer> list) {
        if (root == null) {
            return;
        }
        // 以下即为中序遍历的顺序,先左子树,再根结点,再右子树
        inorder(root.left, list);
        list.add(root.val);
        inorder(root.right, list);
    }
}
```

通过截图:

![image-20210611205821203](https://gitee.com/cao_ziqiang/img/raw/master/20210611205821.png)

<hr/>

我们也可以使用迭代的方式来求解,因为我们使用递归,系统的底层替我们隐性地维护了一个函数调用栈

而我们如果使用迭代的方式,则需要自己来维护这个栈

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
    public List<Integer> inorderTraversal(TreeNode root) {
        Stack<TreeNode> stack = new Stack();
        LinkedList<Integer> res = new LinkedList();
        // root为空且stack为空，遍历结束
        while(stack.size() != 0 || root != null) {
            //如果左子树不为空,则继续往栈内添加左子树
           	while(root!=null) {
                // 由于中序的顺序为先左后根,故入栈顺序为先根后左
                stack.push(root);
                root = root.left;
            }
            // 此时root==null，说明上一步的root没有左子树
            // 1. 执行左出栈。因为此时root==null，导致root.right一定为null
            // 2. 执行下一次外层while代码块，根出栈。此时root.right可能存在
            root = stack.pop();
            //再依次遍历根结点和右子树
            res.add(root.val);
            // 3a. 若root.right存在，右入栈，再出栈
            // 3b. 若root.right不存在，重复步骤2
            root = root.right;
        }
        return res;
    }
}
```

