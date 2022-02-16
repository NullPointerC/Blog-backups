---
title: leetcode-797-所有可能的路径
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: ed3a23f1
date: 2021-11-22 21:25:52
---

[所有可能的路径](https://leetcode-cn.com/problems/all-paths-from-source-to-target/)

<hr/>

![image-20211122214724844](https://gitee.com/cao_ziqiang/img/raw/master/20211122214724.png)

<hr/>

这里没有很理解给出的$graph$,还去自己建图了。思路还是比较容易想到的回溯算法。

```java
class Solution {
    private int n;
    private List<List<Integer>> ans = new ArrayList<>();
    public List<List<Integer>> allPathsSourceTarget(int[][] graph) {
        this.n = graph.length;
        boolean[][] isConnected = new boolean[n][n];
        for(int i = 0; i < n; i++) {
            int[] nextPoint = graph[i];
            for(int next : nextPoint) {
                isConnected[i][next] = true;
            }
        }
        // System.out.println(Arrays.deepToString(isConnected));
        LinkedList<Integer> path = new LinkedList<>();
        path.add(0);
        dfs(path, isConnected, 0);
        return ans;
    }
    private void dfs(LinkedList<Integer> path, boolean[][] isConnected, int cur) {
        // 到n-1节点了,添加一条路径进去
        if (cur == n - 1) {
            ans.add(new ArrayList(path));
            return ;
        }
        // 遍历所有可能到得了的节点
        for(int i = 0; i < n; i++) {
            if (isConnected[cur][i]) {
                path.add(i);
                dfs(path, isConnected, i);
                path.removeLast();
            }
        }
    }
}
```

<hr/>

```java
class Solution {
    List<List<Integer>> list = new ArrayList<>();
    Stack<Integer> path = new Stack<>();

    public List<List<Integer>> allPathsSourceTarget(int[][] graph) {
        dfs(graph,0);
        return list;
    }
    public void dfs(int[][] graph, int cur) {
        path.push(cur);
        if (cur == graph.length - 1) 
            list.add(new ArrayList<>(path));
        for (int next : graph[cur]) 
            dfs(graph, next);
        path.pop();
    }
}
```

