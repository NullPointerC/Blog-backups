---
title: leetcode-547-省份数量
date: 2021-11-20 17:02:51
categories: LeetCode
tags: [LeetCode,algorithm]
---

[$link$](https://leetcode-cn.com/problems/number-of-provinces/)

<hr/>

![image-20211120170337613](https://gitee.com/cao_ziqiang/img/raw/master/20211120170337.png)

<hr/>

tags: (并查集、dfs、bfs)

比较经典的并查集板子题。

```java
class Solution {
    // 并查集边的数量
    private int n;
    private int[] father;
    // 初始化并查集
    private void init() {
        father = new int[n];
        for(int i = 0; i < n; i++) {
            father[i] = i;
        } 
    }
    // 并查集寻根的过程
    private int find(int u) {
        if (u == father[u]) {
            return u;
        }  
        return father[u] = find(father[u]);
    }

    // 将v->u加入到并查集中
    private void union(int u, int v) {
        u = find(u);
        v = find(v);
        if (u == v) {
            return ;
        }
        father[v] = u;
    }
    // 判断u和v是否在同一个根
    private boolean same(int u, int v) {
        u = find(u);
        v = find(v);
        return u == v;
    }
    public int findCircleNum(int[][] isConnected) {
        this.n = isConnected.length;
        init();
        for(int i = 0; i < n; ++i) {
            for(int j = 0; j < n; ++j) {
                if (isConnected[i][j] == 1) union(i, j);
            }
        }
        int ans = 0;
        for(int i = 0; i < n; ++i) {
            if (i == father[i]) {
                ans++;
            }
        }
        return ans;
    }
}
```

当然也可以使用$dfs$或者$bfs$，不过没有并查集来的直观。

可以把$n$个城市和他们之间的关系看成是图，城市是图中的节点，相连关系是图中的边，给定的矩阵就是图的邻接矩阵，省份的数量即图中的联通分量数量。

如果是通过$dfs$实现，就需要遍历所有的城市，对于每个城市，如果该城市没有被访问过，就从这个城市开始$dfs$，同时通过连接矩阵$isConnected$得到与该城市直接相连接的城市数量，这些城市与该城市属于同一个联通分量，然后继续$dfs$，直到同一个联通分量的所有城市被访问完全。遍历的过程中用$visted[]$数组进行标记。

```java
class Solution {
    public int findCircleNum(int[][] isConnected) {
        int n = isConnected.length;
        boolean[] visted = new boolean[n];
        int ans = 0;
        for(int i = 0; i < n; ++i) {
            if (!visted[i]) {
                ans++;
                dfs(i, isConnected, visted);
            }
        }
        return ans;
    }
    private void dfs(int i, int[][]isConnected, boolean[] visted) {
        visted[i] = true;
        for(int j = 0; j < isConnected.length; ++j) {
            if(isConnected[i][j] == 1 && !visted[j]) {
                dfs(j, isConnected, visted);
            }
        }
    }
}
```

bfs同理。

```java
class Solution {
    public int findCircleNum(int[][] isConnected) {
        int n = isConnected.length, ans = 0;
        boolean[] visted = new boolean[n];
        Queue<Integer> queue = new ArrayDeque<>();
        for(int i = 0; i < n; ++i) {
            if(!visted[i]) {
                ans++;
                queue.offer(i);
                visted[i] = true;
                while(!queue.isEmpty()) {
                    int v = queue.poll();
                    for(int j = 0; j < n; ++j) {
                        if (isConnected[v][j] == 1 && !visted[j]) {
                            visted[j] = true;
                            queue.offer(j);
                        }
                    }
                }
            }
        }
        return ans;
    }
}
```

这里再贴一个大佬的题解：[$link$](https://leetcode-cn.com/problems/number-of-provinces/solution/tu-jie-bing-cha-ji-by-time-limit-6x7p/)

