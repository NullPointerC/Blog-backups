---
title: leetcode-46-全排列
date: 2021-11-10 15:36:18
categories: LeetCode
tags: [LeetCode,algorithm]
---

[$link$](https://leetcode-cn.com/problems/permutations/)

<hr/>

![image-20211110154640610](https://gitee.com/cao_ziqiang/img/raw/master/20211110154640.png)

<hr/>

比较经典的回溯算法应用。

```java
class Solution {
    List<List<Integer>> ans = new ArrayList<>();
    List<Integer> path = new ArrayList<>();
    public List<List<Integer>> permute(int[] nums) {
        int n = nums.length;
        boolean[] visted = new boolean[n];
        dfs(visted, nums, path);
        return ans;
    }
    private void dfs(boolean[] visted, int[] nums, List<Integer> path) {
        if (path.size() == nums.length) {
            ans.add(new ArrayList(path));
            return ;
        }
        for(int i = 0; i < visted.length; i++) {
            if (!visted[i]) {
                visted[i] = true;
                path.add(nums[i]);
                dfs(visted, nums, path);
                path.remove(path.size() - 1);
                visted[i] = false;
            }
        }
    }
}
```

