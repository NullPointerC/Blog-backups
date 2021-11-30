---
title: leetcode-78-子集
date: 2021-11-23 20:38:32
categories: LeetCode
tags: [LeetCode,algorithm]
---

[子集](https://leetcode-cn.com/problems/subsets/)

<hr/>

![image-20211123203954058](https://gitee.com/cao_ziqiang/img/raw/master/20211123203954.png)

<hr/>

这里告诉了我们所有元素都互不相同，这样就更加方便我们回溯。

对于每个位置$index$，考虑往后继续选择，可选项包括它后面的所有元素。

选到了$n$时就可以退出了，因为没得选了的😂。

并从0开始，记得遍历过的元素要回溯消除对自身的影响。

借一个大佬的图：

![子集问题递归树.png](https://gitee.com/cao_ziqiang/img/raw/master/20211123204637.png)

可见子集问题就是把所有的叶子节点收集起来。

```java
class Solution {
    List<List<Integer>> ans = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();
    public List<List<Integer>> subsets(int[] nums) {
        int n = nums.length;
        dfs(path, 0, nums);
        return ans;
    }
    private void dfs(LinkedList<Integer> path, int cur, int[] nums) {
        ans.add(new LinkedList(path));
        int n = nums.length;
        if(cur >= n) {
            return ;
        }
        for(int i = cur; i < n; i++) {
            path.add(nums[i]);
            dfs(path, i + 1, nums);
            path.removeLast();
        }
    }
}
```

