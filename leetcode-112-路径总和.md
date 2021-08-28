---
title: leetcode-112-路径总和
date: 2021-07-15 21:07:50
categories: LeetCode
tags: [algorithm,Java,LeetCode]
---

[Link](https://leetcode-cn.com/problems/path-sum/)

<hr/>

## 题目描述：

<pre>
给你二叉树的根节点 root 和一个表示目标和的整数 targetSum ，判断该树中是否存在 根节点到叶子节点 的路径，这条路径上所有节点值相加等于目标和 targetSum 。
叶子节点 是指没有子节点的节点。
</pre>

## 示例1：



![img](https://gitee.com/cao_ziqiang/img/raw/master/20210715214210.jpeg)

<pre>
输入：root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22
输出：true
</pre>

## 示例2：

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210715214250.jpeg)

<pre>
输入：root = [1,2,3], targetSum = 5
输出：false
</pre>

## 示例3：

<pre>
输入：root = [1,2], targetSum = 0
输出：false
</pre>

## 思路1：

<pre>
这道题很容易就想到了用递归来做，但是在做的时候碰到了一个很难解决的问题，就是每条路径的和不好记忆化。
正在困恼我的时候，想到了求和不行就反过来想，用减法，sum-root.val是否等于子节点的val即可。
该解法的想法是一直向下找到叶子节点，如果到叶子节点时sum == root.val，说明找到了一条符合要求的路径。
</pre>

## 代码1：

```java
class Solution {
    public boolean hasPathSum(TreeNode root, int targetSum) {
        return dfs(root,targetSum);
    }

    private boolean dfs(TreeNode root, int sum) {
        if(root == null) {
            return false;
        }
        /*注意叶子节点是左子树和右子树都为空的节点*/
        if(root.left == null && root.right == null) {
            return sum == root.val;
        }
        return dfs(root.left, sum -root.val) || dfs(root.right, sum - root.val);
    }
}
```

![image-20210715214542665](https://gitee.com/cao_ziqiang/img/raw/master/20210715214542.png)

原本我使用加法递归的代码是这样的：

```java
class Solution {

    public boolean hasPathSum(TreeNode root, int sum) {
        return dfs(root, 0, sum);
    }

    public boolean dfs(TreeNode root, int plus, int sum){
        if(root == null) return false;
        if(plus == sum && root.left == null && root.right == null){
            return true;
        }
        return dfs(root.left,plus+root.val,sum) ||  dfs(root.right,plus+root.val,sum);
    }
}
```

后来发现，应该这样写

```java
class Solution {

    public boolean hasPathSum(TreeNode root, int sum) {
        return dfs(root, 0, sum);
    }

    public boolean dfs(TreeNode root, int plus, int sum){
        if(root == null) return false;
        plus += root.val;
        if(plus == sum && root.left == null && root.right == null){
            return true;
        }
        return dfs(root.left,plus,sum) ||  dfs(root.right,plus,sum);
    }
}
```



## 思路2：

<pre>
这里我还想到了用回溯算法。但是一下子想不出来怎么回溯，看了好几篇博客之后搞明白了怎么回溯。
关键在于搜索左、右时我们减去的是 root.val ，所以递归终止条件满足时，记得减去当前节点的值
</pre>

## 代码2：

```java
class Solution {
    public boolean hasPathSum(TreeNode root, int targetSum) {
        if(null == root) return false;
        return dfs(root,targetSum);
    }

    private boolean dfs(TreeNode root, int targetSum){
        // 终止搜索条件
        if(root.left == null && root.right == null){
            // 符合条件，返回 true
            if(root.val - targetSum == 0){
                return true;
            }
            return false;
        }
        // 搜索左树
        if(root.left != null && dfs(root.left, targetSum - root.val)) return true;
        // 搜索右树
        if(root.right != null && dfs(root.right, targetSum - root.val)) return true;
        // 如果左右两树均无符合条件的路径和，返回false
        return false;
    }
}
```

![image-20210715220826415](https://gitee.com/cao_ziqiang/img/raw/master/20210715220826.png)

