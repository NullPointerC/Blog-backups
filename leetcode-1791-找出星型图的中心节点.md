---
title: leetcode-1791-找出星型图的中心节点
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: bfe338cf
date: 2022-02-18 00:06:01
---

[1791. 找出星型图的中心节点](https://leetcode-cn.com/problems/find-center-of-star-graph/)

![image-20220218000705609](https://gitee.com/cao_ziqiang/img/raw/master/20220218000705.png)

由于给定了`edges`是边的集合,共计有`edges.length`条边,所以我们需要找到入度为`edges.length`的点即可。

```java
class Solution {
    public int findCenter(int[][] edges) {
        int n = edges.length + 1;
        int[] cnt = new int[n + 1];
        for(int[] edge : edges) {
            cnt[edge[0]]++;
            cnt[edge[1]]++;
        }
        for(int i = 1; i <= n; i++) {
            if(cnt[i] == n - 1) {
                return i;
            }
        }
        return -1;
    }
}
```

另外，可以观察题目给定的数据可以发现，任意两组数据中肯定能够发现中心，因为这是星型布局，所有任意两组数据中的交集就是中心节点。

```java
class Solution {
    public int findCenter(int[][] edges) {
        int[] edge1 = edges[0];
        int[] edge2 = edges[1];
        if(edge1[0] == edge2[0]) {
            return edge1[0];
        } else if(edge1[1] == edge2[0]) {
            return edge1[1];
        } else if(edge1[0] == edge2[1]) {
            return edge1[0];
        } else if(edge1[1] == edge2[1]) {
            return edge1[1];
        }
        return -1;
    }
}
```

