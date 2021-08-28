---
title: leetcode-100-相同的树
date: 2021-06-24 19:23:55
categories: LeetCode
tags: [algorithm,Java,LeetCode]
---

<a href="https://leetcode-cn.com/problems/same-tree/submissions/">传送门</a>

<hr/>

题目描述:

<pre>
给你两棵二叉树的根节点 p 和 q ，编写一个函数来检验这两棵树是否相同。
如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。
</pre>

<hr/>

示例1:

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210624192631.jpeg)

<pre>
输入：p = [1,2,3], q = [1,2,3]
输出：true
</pre>

<hr/>

示例2:

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210624192706.jpeg)

<pre>
输入：p = [1,2], q = [1,null,2]
输出：false   
</pre>

<hr/>

示例3:

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210624192731.jpeg)

<pre>
输入：p = [1,2,1], q = [1,1,2]
输出：false
</pre>

<hr/>

提示:

<pre>
两棵树上的节点数目都在范围 [0, 100] 内
-104 <= Node.val <= 104
</pre>

<hr/>

思路:

```java
    由于树结构的天然递归性质,所以使用递归可以既简单又高效的完成,首先判断根结点是否相同,然后递归判断左子树
和右子树即可
	再额外编写一个判断是否相同结点的递归函数帮助我们判断即可,在判断时要注意逻辑判断的先后顺序
```

<hr/>

代码:

```java
class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if(p != null && q!=null) {
            /*根结点不为空,则判断根结点和以它的左右子树为根的数*/
            return isSameNode(p, q) && isSameTree(p.left,q.left) && isSameTree(p.right,q.right);
        }else {
            /*根结点为空则直接判断根结点即可*/
            return isSameNode(p,q);
        }
    }
    public boolean isSameNode(TreeNode p, TreeNode q) {
        /*若两根中有一根为null节点,则返回false即可*/
        if(p == null && q != null || p!=null && q == null) {
            return false;
        }else if((null == p) && (null == q)){
            /*若两根节点均为null,则说明这两节点相同*/
            return true;
        }else if(p == q || p.val == q.val){
            /*若两节点指向的地址或两节点的值相同,也说明这两节点相同*/
            return true;
        }else {
            /*其他情况均为false*/
            return false;
        }
    }
}
```

![image-20210624193313229](https://gitee.com/cao_ziqiang/img/raw/master/20210624193313.png)

<hr/>

PS:在leetcode中常常有关于树的题目,而且leetcode经常使用数组来创建树,如果每次我们都手动创建树是十分麻烦的,这里我自己写了一个TreeNode简化了一些自己的调试过程

```java
import java.util.ArrayDeque;

/**
 * @author Ziqiang CAO
 * @email 1213409187@qq.com
 */
public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;
    TreeNode() {}
    TreeNode(int val) { this.val = val; }
    TreeNode(int val, TreeNode left, TreeNode right) {
        this.val = val;
        this.left = left;
        this.right = right;
    }

    public static TreeNode createTreeByArray(Integer[] arr) {
        // 使用队列来存储每一层的非空节点，下一层的数目要比上一层高
        ArrayDeque<TreeNode> pre = new ArrayDeque<>();
        TreeNode root = new TreeNode(arr[0]);
        pre.addLast(root);
        // 表示要遍历的下一个节点
        int index = 0;
        while (!pre.isEmpty()) {
            ArrayDeque<TreeNode> cur = new ArrayDeque<>();
            while (!pre.isEmpty()) {
                TreeNode node = pre.removeFirst();
                TreeNode left=null;
                TreeNode right=null;
                // 如果对应索引上的数组不为空的话就创建一个节点,进行判断的时候，
                // 要先索引看是否已经超过数组的长度，如果索引已经超过了数组的长度，那么剩下节点的左右子节点就都是空了
                // 这里index每次都会增加，实际上是不必要的，但是这样写比较简单
                if (++index<arr.length&&arr[index]!=null){
                    left=new TreeNode(arr[index]);
                    cur.addLast(left);
                }
                if (++index<arr.length&&arr[index]!=null){
                    right=new TreeNode(arr[index]);
                    cur.addLast(right);
                }
                node.left=left;
                node.right=right;
            }
            pre=cur;
        }

        return root;
    }
    public static void prePrintTree(TreeNode node) {
        if (node == null) {
            return;
        }
        System.out.print(node.val + " ");
        prePrintTree(node.left);
        prePrintTree(node.right);
    }
}
```

