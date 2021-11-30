---
title: leetcode-501-二叉搜索树中的众数
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-09-19 17:44:15
---

[$link$](https://leetcode-cn.com/problems/find-mode-in-binary-search-tree/)

<hr/>

![image-20210919174452381](https://gitee.com/cao_ziqiang/img/raw/master/20210919174452.png)

<hr/>

题目倒是不难，就是处理上稍微麻烦一些。

比较简单的办法是对$BST$深搜，用$map$存每个数字和它出现的频率，并且更新下众数的频率。

最后遍历$map$的$keySet$，如果$key$对应的频率和众数一致，那么就说明这个就是众数，加入到$list$中再返回$int[]$

```java
class Solution {
    Map<Integer, Integer> map = new HashMap<>();
    int maxTimes = 0;
    public int[] findMode(TreeNode root) {
        dfs(root);
        List<Integer> ansList = new ArrayList<>();
        for(Integer val : map.keySet()) {
            if(map.get(val) == maxTimes) {
                ansList.add(val);
            }
        }
        int[] ans = new int[ansList.size()];
        for(int i = 0;i<ansList.size();i++) {
            ans[i] = ansList.get(i);    
        }
        return ans;
    }

    private void dfs(TreeNode root) {
        if(root == null) {
            return;
        }
        map.put(root.val,map.getOrDefault(root.val, 0) + 1);
        maxTimes = Math.max(map.get(root.val), maxTimes);
        dfs(root.left);
        dfs(root.right);
    }
}
```

<hr/>

不适用额外空间，空间复杂度为$O(n)$的解法

```java
class Solution {
    List<Integer> list = new ArrayList<>();
    // 用来表示前一个遍历元素
    int base = Integer.MIN_VALUE;
    // 当前元素出现的次数
    int count = 0;
    // 最大出现频率
    int freq = 0;
    public int[] findMode(TreeNode root) {
        inOrder(root);
        int[] ans = new int[list.size()];
        for(int i = 0;i<list.size();i++) {
            ans[i] = list.get(i);
        }
        return ans;
    }

    private void update(int num) {
        // 和前一个元素相同则直接更新频率
        if(num == base) {
            count++;
        }else{
            // 否则重置base
            base = num;
            count = 1;
        }
        if(count == freq) {
            // 当前元素频率为众数的频率，添加进list
            list.add(num);
        }
        if(count > freq) {
            // 否则重置list
            freq = count;
            list = new ArrayList<>();
            list.add(num);
        }
    }

    private void inOrder(TreeNode root) {
        if(root == null) {
            return;
        }
        inOrder(root.left);
        update(root.val);
        inOrder(root.right);
    }
}
```

