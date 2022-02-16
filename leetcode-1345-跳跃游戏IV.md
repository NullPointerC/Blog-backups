---
title: leetcode-1345-跳跃游戏IV
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: a147900c
date: 2022-01-21 16:29:46
---

[1345. 跳跃游戏 IV](https://leetcode-cn.com/problems/jump-game-iv/)

![image-20220121163026100](https://gitee.com/cao_ziqiang/img/raw/master/20220121163026.png)

由题意要从0走到最后一个位置，并且相同元素之间可以相互跳转，不仅仅可以顺序跳转，所以把整个数组抽象为**无向图**（因为相互之间都可以互相跳转）。数组中的每个元素作为图的顶点，相邻下标的元素有边连接，相同值元素之间也有边连接。每条边的权重都为1，要求从顶点0走到顶点$n-1$的最少操作数，因此这里用$bfs$来解决。

同时，这里因为在访问每一个节点时，会将它的所有可达但未访问节点入队。所以当访问有相同元素时，会使得之前已经被访问的节点会重复入队，可以分析得出无需重复入队，所以还可以设置一个set来防止节点重复入队。

```java
class Solution {
    public int minJumps(int[] arr) {
        int n = arr.length;
        if(n == 0) return 0;
        Map<Integer, List<Integer>> map = new HashMap<>();
        for(int i = 0; i < n; i++) {
            if(map.containsKey(arr[i])) {
                map.get(arr[i]).add(i);
            } else {
                map.put(arr[i], new ArrayList<>());
                map.get(arr[i]).add(i);
            }
        }
        Set<Integer> set = new HashSet<>();
        set.add(0);
        Queue<int[]> q = new ArrayDeque<>();
        q.offer(new int[]{0, 0});
        while(!q.isEmpty()) {
            int[] cur = q.poll();
            int idx = cur[0], step = cur[1];
            if(idx == n - 1) {
                return step;
            }
            step++;
            if(map.containsKey(arr[idx])) {
                for(int portal : map.get(arr[idx])) {
                    if(!set.contains(portal)) {
                        q.offer(new int[]{portal, step});
                        set.add(portal);
                    }
                }
                map.remove(arr[idx]);
            }
            if(idx - 1 >= 0 && !set.contains(idx - 1)) {
                q.offer(new int[]{idx - 1, step});
                set.add(idx - 1);
            }
            if(idx + 1 < n && !set.contains(idx + 1)) {
                q.offer(new int[]{idx + 1, step});
                set.add(idx + 1);
            }
        }
        return -1;
    }
}
```

