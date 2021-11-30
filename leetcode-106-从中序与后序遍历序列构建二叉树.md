---
title: leetcode-106-从中序与后序遍历序列构建二叉树
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-08-19 09:11:16
---

[link](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

[从前序与中序遍历序列构建二叉树](https://www.codenote.xyz/algorithm/2021/08/15/leetcode-105-cong-qian-xu-he-zhong-xu-bian-li-xu-lie-gou-zao-er-cha-shu/)

<hr/>

题目描述:

<pre>
根据一棵树的中序遍历与后序遍历构造二叉树。
注意:你可以假设树中没有重复的元素。
例如，给出
中序遍历 inorder = [9,3,15,20,7]
后序遍历 postorder = [9,15,7,20,3]
返回如下的二叉树：
    3
   / \
  9  20
    /  \
   15   7
</pre>

思路：

<pre>
这里我们先要了解后序遍历的特点，即后序最后元素是根节点。
那么我们就可以根据这个根找到它在中序序列中的位置inRoot
中序中根结点的左边就是它的左子树结点，右边就是右子树节点[左子树结点,根结点,右子树结点]
所以左子树节点个数为numsLeft = inRoot - inStart
后序中结点分布不[左子树结点,右子树结点,根结点]
根据前一步确定的左子树个数,可以确定后序中左子树节点的范围和右子树结点的范围;
</pre>

代码如下：

<pre>
此处用了一个inMap来存储中序结点值和它所在索引的映射关系
</pre>



```java
class Solution {
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        Map<Integer, Integer> mapping = new HashMap<>();
        for (int i = 0; i < inorder.length; i++) {
            mapping.put(inorder[i], i);
        }
        return buildTree(inorder, 0, inorder.length - 1, postorder, 0, postorder.length - 1, mapping);
    }

    private TreeNode buildTree(int[] inorder, int inStart, int inEnd,
                               int[] postorder, int postStart, int postEnd,
                               Map<Integer, Integer> inMap) {
        if (inStart > inEnd || postStart > postEnd) {
            return null;
        }
        // 后序遍历最后一个为根结点
        TreeNode root = new TreeNode(postorder[postEnd]);
        int inRoot = inMap.get(root.val);
        int numsLeft = inRoot - inStart;

        root.left = buildTree(inorder, inStart, inRoot - 1, postorder, postStart, postStart + numsLeft - 1, inMap);
        root.right = buildTree(inorder, inRoot + 1, inEnd, postorder, postStart + numsLeft, postEnd - 1, inMap);
        return root;
    }
}
```

![image-20210819091848204](https://gitee.com/cao_ziqiang/img/raw/master/20210819091848.png)

summary：

<pre>
 根据前序和中序可以构造一颗二叉树，根据中序和后续也可以构建一颗二叉树。 反正必须要有中序才能构建，因为没有中序，你没办法确定树的形状。
</pre>

