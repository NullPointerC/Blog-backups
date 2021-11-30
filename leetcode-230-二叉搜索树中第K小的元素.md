---
title: leetcode-230-二叉搜索树中第K小的元素
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-08-29 14:49:50
---

[link](https://leetcode-cn.com/problems/kth-smallest-element-in-a-bst/)

<hr/>

题目描述:

<pre>
给定一个二叉搜索树的根节点 root ，和一个整数 k ，请你设计一个算法查找其中第 
k 个最小元素（从 1 开始计数）。
</pre>

示例1:
![img](https://gitee.com/cao_ziqiang/img/raw/master/20210829145110.jpeg)

<pre>
输入：root = [3,1,4,null,2], k = 1
输出：1
</pre>

示例2:

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210829145128.jpeg)

<pre>
输入：root = [5,3,6,2,4,null,null,1], k = 3
输出：3
</pre>

思路:

<pre>
最简单的办法莫过于先对树进行中序遍历,然后将节点按中序次序放入一个列表中;
再对列表中第k个位置处的值取值即可;
</pre>

代码:

```java
class Solution {
    private List<TreeNode> list = new ArrayList<>();

    public int kthSmallest(TreeNode root, int k) {
        traversal(root, list);
        return list.get(k - 1).val;
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
```

<pre>
当然也可以使用栈来保存遍历序列,并且对于遍历适当剪枝
</pre>

```java
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        LinkedList<TreeNode> stack = new LinkedList<>();
        stack.addFirst(root);
        int ans = 0;
        int count = 0;
        while (!stack.isEmpty()) {
            while (root.left != null) {
                stack.addFirst(root.left);
                root = root.left;
            }
            TreeNode t = stack.removeFirst();
            count++;
            if (count == k) {
                ans = t.val;
                break;
            }
            t = t.right;
            while (t != null) {
                stack.addFirst(t);
                t = t.left;
            }
        }
        return ans;
    }
}
```

![image-20210829150710884](https://gitee.com/cao_ziqiang/img/raw/master/20210829150710.png)

<pre>
题目还提到了一个进阶,就是当BST有频繁的插入和删除的要求的时候,要如何使topK更
加高效;
可以先考虑做一次后续遍历,记录每个节点的左右子树大小,这里考虑使用map实现;
在有插入和删除的过程时,维护这个信息;
topK询问时,利用左子树大小信息得出root的排名(rank),根据rank和询问K的关系;
到左子树或是右子树中去寻找,并且更新排名(rank),就可以再O(h)内完成topK询问;
</pre>

代码:

```java
class Solution {
    HashMap<TreeNode, Integer> leftSubTreeSize = new HashMap<>();
    HashMap<TreeNode, Integer> rightSubTreeSize = new HashMap<>();
    public int kthSmallest(TreeNode root, int k) {
        if (root == null) {
            return 0;
        }
        postorder(root);
        int totalSize = leftSubTreeSize.get(root) + rightSubTreeSize.get(root) + 1;
        int rank = leftSubTreeSize.get(root) + 1;
        while (k != rank) {
            if (k < rank) {
                root = root.left;
                if (root != null) {
                    rank -= rightSubTreeSize.get(root) + 1;
                }
            } else {
                root = root.right;
                if (root != null) {
                    rank += leftSubTreeSize.get(root) + 1;
                }
            }
        }
        return root.val;
    }

    private int postorder(TreeNode root) {
        if (root == null) {
            return 0;
        }
        leftSubTreeSize.put(root, postorder(root.left));
        rightSubTreeSize.put(root, postorder(root.right));
        return leftSubTreeSize.get(root) + rightSubTreeSize.get(root) + 1;
    }
}
```

在评论区看到一位大佬也是类型的思路,但是没有记忆化结点个数,这样每次topK询问都要查询一次,还是比较慢的;

```java
class Solution {
    /**
     * 查找左子树节点个数为leftN,如果K<=leftN,则所查找节点在左子树上.
     * 若K=leftN+1,则所查找节点为根节点
     * 若K>leftN+1,则所查找节点在右子树上,按照同样方法查找右子树第K-leftN个节点
     * @param root
     * @param k
     * @return
     */
    public int kthSmallest(TreeNode root, int k) {
        int leftN=findChild(root.left);
        if(leftN+1==k) return root.val;
        else if(k<=leftN){
            return kthSmallest(root.left, k);
        }
        else return kthSmallest(root.right, k-leftN-1);
    }
    /**
     *查找子节点个数
     * @param root
     * @return
     */
    public int findChild(TreeNode root){
        if(root==null) return 0;
        return findChild(root.left)+findChild(root.right)+1;
    }
}
```

