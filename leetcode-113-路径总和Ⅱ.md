---
title: leetcode-113-路径总和Ⅱ
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-08-21 09:19:05
---

[link](https://leetcode-cn.com/problems/path-sum-ii/)

题目描述:

<pre>
给你二叉树的根节点 root 和一个整数目标和 targetSum ，找出所有 从根节点到叶子节点 路径总和等于给定标和的路径。
叶子节点 是指没有子节点的节点。
</pre>

示例1:

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210821092740.jpeg)

<pre>
输入：root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
输出：[[5,4,11,2],[5,8,4,5]]
</pre>

**示例 2：**

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210821092800.jpeg)

<pre>
输入：root = [1,2,3], targetSum = 5
输出：[]
</pre>



**示例3**

<pre>
输入：root = [1,2], targetSum = 0
输出：[]
</pre>

思路:

<pre>
和上一题一样的思路,不过要注意这里需要标记回溯,并且要标记当前遍历树的状态
</pre>



代码:

```java
class Solution {
    public List<List<Integer>> ans = new LinkedList<>();


    public List<List<Integer>> pathSum(TreeNode root, int targetSum) {
        List<Integer> subList = new ArrayList<>();
        traversal(root, targetSum, subList);
        return ans;
    }

    private void traversal(TreeNode root, int targetSum, List<Integer> subAns) {
        if (root == null) {
            return;
        }
        subAns.add(root.val);

        if ((root.left == null && root.right == null)) {
            if (targetSum == root.val) {
                ans.add(new LinkedList<>(subAns));
                // ans.add(subAns);
            }
        }


        traversal(root.left, targetSum - root.val, subAns);
        traversal(root.right, targetSum - root.val, subAns);
        subAns.remove(subAns.size() - 1);
    }
}
```

![image-20210821093008152](https://gitee.com/cao_ziqiang/img/raw/master/20210821093008.png)

