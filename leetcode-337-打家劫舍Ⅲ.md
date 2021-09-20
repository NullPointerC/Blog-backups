---
title: leetcode-337-打家劫舍Ⅲ
date: 2021-09-02 18:40:29
categories: LeetCode
tags: [algorithm,Java,LeetCode]

---

[link](https://leetcode-cn.com/problems/house-robber-iii/submissions/)

<hr/>

tags: 后序遍历、dp、记忆化

**题目描述:**

<pre>
在上次打劫完一条街道之后和一圈房屋后，小偷又发现了一个新的可行窃的地区。这
个地区只有一个入口，我们称之为“根”。 除了“根”之外，每栋房子有且只有一
个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排
列类似于一棵二叉树”。 如果两个直接相连的房子在同一天晚上被打劫，房屋将自动
报警。
计算在不触动警报的情况下，小偷一晚能够盗取的最高金额。
</pre>

**示例 1:**

<pre>
输入: [3,2,3,null,3,null,1]
	 3
	/ \
   2   3
    \   \ 
     3   1
输出: 7 
解释: 小偷一晚能够盗取的最高金额 = 3 + 3 + 1 = 7
</pre>

**示例 2:**

<pre>
输入: [3,4,5,1,3,null,1]
     3
    / \
   4   5
  / \   \ 
 1   3   1
输出: 9
解释: 小偷一晚能够盗取的最高金额 = 4 + 5 = 9.
</pre>

**思路:**

<pre>
对于一个节点来说,有偷和不偷两种选择,
如果选择偷该节点,那么就需要对其左子树的子树和右子树的子树进行查找;
如果选择不偷该节点,那么就可以直接查找左子树和右子树之和;
可以看出在这个过程中,很多数据将会被重复利用上,所以我们选择使用map来存储
</pre>

**代码:**

```java
class Solution {
    Map<TreeNode, Integer> sums = new HashMap<>();

    public int rob(TreeNode root) {
        return traversal(root);
    }

    private int traversal(TreeNode root) {
        if (root == null) {
            return 0;
        }
        if (sums.containsKey(root)) {
            return sums.get(root);
        }
        // 偷取该节点
        int res1 = 0;
        if (root.left != null) {
            res1 += traversal(root.left.left) + traversal(root.left.right);
        }
        if (root.right != null) {
            res1 += traversal(root.right.left) + traversal(root.right.right);
        }
        res1 += root.val;
        // 不偷取该节点
        int res2 = traversal(root.left) + traversal(root.right);
        sums.put(root, Math.max(res1, res2));
        return sums.get(root);
    }
}
```

<pre>
也可以一个数组来临时记录,这个数组可以只有2位,int[0]代表不偷根结点,
int[1]代表偷根结点
</pre>

**代码:**

```java
class Solution {
    public int rob(TreeNode root) {
       int[] res = helper(root);//int[0]代表不偷，int[1]代表偷
       return Math.max(res[0],res[1]);
    }

    public int[] helper(TreeNode root) {
       int[] res =new int[2];
       if(root == null){
           return res;
       }
       int[] left = helper(root.left);
       int[] right = helper(root.right);

       //叶子偷
       res[1]=root.val+left[0]+right[0];
       //不偷
  		res[0]=0+Math.max(left[0],left[1])+Math.max(right[0],right[1]);

       return res;

    }
}
```

![image-20210902202935927](https://gitee.com/cao_ziqiang/img/raw/master/20210902202936.png)

