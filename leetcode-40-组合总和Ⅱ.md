---
title: leetcode-40-组合总和Ⅱ
categories: LeetCode
tags:
  - LeetCode
  - algorithm
hidden: true
abbrlink: 3d7e523e
date: 2021-12-01 17:20:23
---

[组合总和 II](https://leetcode-cn.com/problems/combination-sum-ii/)

<hr/>

![image-20211201172122304](https://gitee.com/cao_ziqiang/img/raw/master/20211201172122.png)

<hr/>

本题的不同在于，每个数字只能使用1次，而且数字是有重复的。

所以要在回溯的过程中就考虑去重这个问题，去重这里有同一路径上的去重和同一个树层上的去重，这里分析到一个集合里的可以重复，而如果两个集合的组合一样，那么就是重复的，所以考虑在树层上去重，而不是在路径上去重。

这里可以先对数组排序，这样更加遍历在同一层上去重，因为上一节点做选择时，如果下面的几个相邻节点是相等的，那么显然可以做部分剪枝。

![img](https://gitee.com/cao_ziqiang/img/raw/master/20211201172515.png)

引一位大佬的图解释一下。

这道题还需要引入一个boolean类型的数组$used$来记录同一个树枝上的元素是否被使用过。集合去重的任务就是利用$used$来完成的。

```java
class Solution {
    List<List<Integer>> lists = new ArrayList<>();
    Deque<Integer> deque = new LinkedList<>();
    int sum = 0;

    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        //为了将重复的数字都放到一起，所以先进行排序
        Arrays.sort(candidates);
        //加标志数组，用来辅助判断同层节点是否已经遍历
        boolean[] flag = new boolean[candidates.length];
        backTracking(candidates, target, 0, flag);
        return lists;
    }

    public void backTracking(int[] arr, int target, int index, boolean[] flag) {
        if (sum == target) {
            lists.add(new ArrayList(deque));
            return;
        }
        for (int i = index; i < arr.length && arr[i] + sum <= target; i++) {
            //出现重复节点，同层的第一个节点已经被访问过，所以直接跳过
            if (i > 0 && arr[i] == arr[i - 1] && !flag[i - 1]) {
                continue;
            }
            flag[i] = true;
            sum += arr[i];
            deque.push(arr[i]);
            //每个节点仅能选择一次，所以从下一位开始
            backTracking(arr, target, i + 1, flag);
            int temp = deque.pop();
            flag[i] = false;
            sum -= temp;
        }
    }
}
```

也可以用下标来标识去重：

```java
class Solution {

    private List<List<Integer>> ans = new ArrayList<>();
    private LinkedList<Integer> path = new LinkedList<>();

    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        int n = candidates.length;
        if(n == 0 || target < 0) {
            return ans;
        }
        Arrays.sort(candidates);
        dfs(candidates, 0, target);
        return ans;
    }

    private void dfs(int[] candidates, int idx, int now) {
        if(now == 0) {
            ans.add(new LinkedList(path));
            return ;
        }
        for(int i = idx; i < candidates.length; i++) {
            if((i > idx && candidates[i] == candidates[i - 1])) {
                continue;
            }
            if(now - candidates[i] < 0) {
                return ;
            }
            path.add(candidates[i]);
            dfs(candidates, i + 1, now - candidates[i]);
            path.removeLast();
        }
    }
}
```

