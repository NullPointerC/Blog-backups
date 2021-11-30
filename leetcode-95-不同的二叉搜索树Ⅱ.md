---
title: leetcode-95-不同的二叉搜索树Ⅱ
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-08-16 13:56:41
---

[link](https://leetcode-cn.com/problems/unique-binary-search-trees-ii)

<hr/>

题目描述:

<pre>
给你一个整数 n ，请你生成并返回所有由 n 个节点组成且节点值从 1 到 n 互不
相同的不同 二叉搜索树 。可以按 任意顺序 返回答案。
</pre>

示例1:

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210816135735.jpeg)

<pre>
输入：n = 3
输出：[[1,null,2,null,3],[1,null,3,2],[2,1,3],
[3,1,null,null,2],[3,2,null,1]]
</pre>

示例2:

<pre>
输入：n = 1
输出：[[1]]
</pre>

思路:

```
对于连续的序列[begin, end]中的每一点i,生成以i为根节点的BST
i左边的序列可以作为左子树,左儿子可能有多个,所以List<TreeNode> leftTree = generate(begin, i - 1);
i右边的序列可以作为右子树,同上,List<TreeNode> rightTree = generate(i + 1, end);
产生的以当前i为根结点的BST（子）树有leftTree.size() * rightTree.size()个，遍历每种情况，即可生成以i为根节点的BST序列；
然后以for循环使得[begin, end]中每个结点都能生成子树序列。
一旦begin大于end，则说明这里无法产生子树，所以此处应该是作为空结点返回：ans.add(null); return ans;
```

代码:

```java
class Solution {
    
    public List<TreeNode> generateTrees(int n) {
        return generate(1, n);
    }

    private List<TreeNode> generate(int begin, int end) {
        
        List<TreeNode> ans = new LinkedList<>();
        
        if (begin > end) {
            ans.add(null);
            return ans;
        }
        
        for (int i = begin; i <= end; i++) {
            
            List<TreeNode> leftTree = generate(begin, i - 1);
            
            List<TreeNode> rightTree = generate(i + 1, end);
            
            for (TreeNode leftNode : leftTree) {
                for (TreeNode rightNode : rightTree) {
                    TreeNode root = new TreeNode(i);
                    root.left = leftNode;
                    root.right = rightNode;
                    ans.add(root);
                }
            }
        }
        return ans;
    }
}
```

![image-20210816140525565](https://gitee.com/cao_ziqiang/img/raw/master/20210816140525.png)

