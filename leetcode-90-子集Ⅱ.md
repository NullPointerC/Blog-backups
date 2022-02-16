---
title: leetcode-90-子集Ⅱ
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: '825019e3'
date: 2021-11-23 20:49:03
---

[子集 II](https://leetcode-cn.com/problems/subsets-ii/)

<hr/>

![image-20211123204936940](https://gitee.com/cao_ziqiang/img/raw/master/20211123204937.png)

<hr/>

这里比起上一题多了去重这一步，即选过的元素不能被重复取，即在选取过的元素中出现就不选。理论上这样是可行的，但是实际上会出问题。

![image-20211123211613514](https://gitee.com/cao_ziqiang/img/raw/master/20211123211613.png)

因为在这里使用过是区分为两个维度上的使用过，一个是同一个树枝上使用过，一个是同一层上使用过。

我们发现元素如果在同一个组合里，是可以重复的，在所有的路径里，是不允许重复的。因此，我们需要去重的是同一树层上的使用过，同一树枝上使用过的可以使用同一个组合里的元素，可以不用去重。

![90.子集II.png](https://gitee.com/cao_ziqiang/img/raw/master/20211123211836.png)

```java
class Solution {
    List<List<Integer>> ans = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        boolean[] used = new boolean[nums.length];
        Arrays.sort(nums);
        dfs(path, 0, nums, used);
        return ans;
    }
    private void dfs(LinkedList<Integer> path, int cur, int[] nums, boolean[] used) {
        ans.add(new LinkedList<>(path));
        int n = nums.length;
        if(cur >= n) {
            return ;
        }
        for(int i = cur; i < n; i++) {
            // 同一树层的元素不允许重复
            if(i > 0 && nums[i] == nums[i - 1] && used[i - 1] == false) {
                continue;
            }
            path.add(nums[i]);
            used[i] = true;
            dfs(path, i + 1, nums, used);
            path.removeLast();
            used[i] = false;
        }
    }
}
```

使用set会使得代码看上去更加简洁：

```java
class Solution {
    List<List<Integer>> ans = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        // boolean[] used = new boolean[nums.length];
        Arrays.sort(nums);
        dfs(path, 0, nums);
        return ans;
    }
    private void dfs(LinkedList<Integer> path, int cur, int[] nums) {
        ans.add(new LinkedList<>(path));
        int n = nums.length;
        if(cur >= n) {
            return ;
        }
        Set<Integer> set = new HashSet<>();
        for(int i = cur; i < n; i++) {
            // 同一树层的元素不允许重复
            if(set.contains(nums[i])) {
                continue;
            }
            set.add(nums[i]);
            path.add(nums[i]);
            // used[i] = true;
            dfs(path, i + 1, nums);
            path.removeLast();
            // used[i] = false;
        }
    }
}
```

