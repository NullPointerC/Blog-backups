---
title: leetcode-39-组合总和
categories: LeetCode
tags:
  - LeetCode
  - algorithm
hidden: true
abbrlink: 660ea70c
date: 2021-12-01 16:28:15
---

[组合总和](https://leetcode-cn.com/problems/combination-sum/)

<hr/>

![image-20211201162900999](https://gitee.com/cao_ziqiang/img/raw/master/20211201162901.png)

<hr/>

tags(回溯)

也是一种比较经典的回溯算法了，和子集不同在于需要收集的是从根到叶子节点的一条路径，和排列不同在于要符合条件的路径才收集，而不是长度为$n$的所有路径都收集。

题目说到每个数字可以被无限次数选取，而且要有数字不同才算是不同的组合，所以需要做好去重。

```java
class Solution {
    private List<List<Integer>> ans = new ArrayList<>();
    private LinkedList<Integer> path = new LinkedList<>();
    
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        int n = candidates.length;
        Arrays.sort(candidates);
        if(n == 0 || target <= 0) {
            return ans;
        }
        dfs(candidates, target, 0);
        return ans;
    }

    private void dfs(int[] candidates, int now, int i) {
        if (now == 0) {
            ans.add(new LinkedList(path));
            return ;
        }
        if (now < 0) {
            return ;
        }
        for(int j = i; j < candidates.length; j++) {
            now -= candidates[j];
            path.add(candidates[j]);
            dfs(candidates, now, j);
            path.removeLast();
            now += candidates[j];
        }
    }
}
```

