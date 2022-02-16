---
title: leetcode-173-二叉搜索树迭代器
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: c8c9d776
date: 2021-08-29 10:31:59
---

[link](https://leetcode-cn.com/problems/binary-search-tree-iterator/)

题目描述:

<pre>
实现一个二叉搜索树迭代器类BSTIterator ，表示一个按中序遍历二叉搜索树（BST）的迭代器：
BSTIterator(TreeNode root) 初始化 BSTIterator 类的一个对象。BST 的根节点 root 会作为构造函数的一部分给出。指针应初始化为一个不存在于 BST 中的数字，且该数字小于 BST 中的任何元素。
boolean hasNext() 如果向指针右侧遍历存在数字，则返回 true ；否则返回 false 。
int next()将指针向右移动，然后返回指针处的数字。
注意，指针初始化为一个不存在于 BST 中的数字，所以对 next() 的首次调用将返回 BST 中的最小元素。
你可以假设 next() 调用总是有效的，也就是说，当调用 next() 时，BST 的中序遍历中至少存在一个下一个数字。
</pre>

示例:

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210829103829.png)

<pre>
输入
["BSTIterator", "next", "next", "hasNext", "next", "hasNext", "next", "hasNext", "next", "hasNext"]
[[[7, 3, 15, null, null, 9, 20]], [], [], [], [], [], [], [], [], []]
输出
[null, 3, 7, true, 9, true, 15, true, 20, false]
解释
BSTIterator bSTIterator = new BSTIterator([7, 3, 15, null, null, 9, 20]);
bSTIterator.next();    // 返回 3
bSTIterator.next();    // 返回 7
bSTIterator.hasNext(); // 返回 True
bSTIterator.next();    // 返回 9
bSTIterator.hasNext(); // 返回 True
bSTIterator.next();    // 返回 15
bSTIterator.hasNext(); // 返回 True
bSTIterator.next();    // 返回 20
bSTIterator.hasNext(); // 返回 False
</pre>

思路:

<pre>
加入两个私有成员,一个list装bst的结点,一个index表示迭代器索引
在构造器被执行的时候,对bst进行中序遍历将list填充好,并把index设置为-1开始迭代
</pre>

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
class BSTIterator {

    private List<TreeNode> nodes;
    private int index;

    public BSTIterator(TreeNode root) {
        nodes = new ArrayList<>();
        index = -1;
        traversal(root, nodes);
    }

    public int next() {
        return nodes.get(++index).val;
    }

    public boolean hasNext() {
        return index < nodes.size() - 1;
    }

    private void traversal(TreeNode root, List<TreeNode> list) {
        if (root == null) {
            return;
        }
        traversal(root.left, list);
        list.add(root);
        traversal(root.right, list);
    }
}

/**
 * Your BSTIterator object will be instantiated and called as such:
 * BSTIterator obj = new BSTIterator(root);
 * int param_1 = obj.next();
 * boolean param_2 = obj.hasNext();
 */
```

![image-20210829104021092](https://gitee.com/cao_ziqiang/img/raw/master/20210829104021.png)

这样保证了next和hasNext两个方法可以在O(1)时间内完成,因为ArrayList支持按索引取值,空间复杂度为O(N)即结点个数;

如果不使用ArrayList事先存储节点的,而是在next的过程中不断取值删值,也可以达到在均摊为O(1)级别的next和hasNext方法

如下:

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
class BSTIterator{
	private LinkedList<TreeNode> stack;
    
    public BSTIterator(TreeNode root) {
        stack = new LinkedList<>();
        while(root != null) {
            stack.addFirst(root);
            root = root.left;
        }
    }
    
    public int next() {
		TreeNode t = stack.removeFirst();
        int val = t.val;
        t = t.right;
        while(t != null) {
            stack.addFirst(t);
            t = t.left;
        }
        return val;
    }
    public boolean hasNext() {
        return !stack.isEmpty();
    }
}

/**
 * Your BSTIterator object will be instantiated and called as such:
 * BSTIterator obj = new BSTIterator(root);
 * int param_1 = obj.next();
 * boolean param_2 = obj.hasNext();
 */
```

![image-20210829105820050](https://gitee.com/cao_ziqiang/img/raw/master/20210829105820.png)

<pre>
因为在大部分情况下都是可以直接取root.left而直接返回,所以均摊时间复杂度仍为
O(1),但是这样空间上得到了优化,stack中最多存储树高度O(lgn)个结点
</pre>

