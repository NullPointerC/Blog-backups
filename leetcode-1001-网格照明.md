---
title: leetcode-1001-网格照明
date: 2022-02-08 23:55:54
categories: LeetCode
tags: [LeetCode,algorithm]
---

[1001. 网格照明](https://leetcode-cn.com/problems/grid-illumination/ "1001. 网格照明")

<hr/>

![image-20220208235702600](https://gitee.com/cao_ziqiang/img/raw/master/20220208235702.png)

![image-20220208235754094](https://gitee.com/cao_ziqiang/img/raw/master/20220208235910.png)

![img](https://gitee.com/cao_ziqiang/img/raw/master/20220208235923.jpeg)

感觉这题不难,但是考虑的东西比较仔细。

观察到数据范围有$1e9$，如果强行模拟，肯定会TLE。所以我们这里观察到一个灯泡可以使一行、一列和正反对角线的单元格都亮起，所以可以仅记录下亮起来的行列和两条对角线。

这里用$row$和$col$来标记当前行或列是否亮起，用$left$和$right$表示两个不同走向的对角线是否亮起。

最后将二维坐标$(x,y)$一维化成$x \times n + y$。防止重复添加灯泡，当加入一颗灯泡💡时，标记$row(x)$和$col(y)$都加1，对于对角线的，$left$表示正对角线，与$x$轴交点坐标为$(x+y)$，将其标记，$right$表示为反对角线，与$y$轴交点的坐标为$(x-y)$。

首先枚举所有灯泡，并且标记所有可以被灯照到的位置。

再枚举所有的查询，对于每个查询，我们也用同样的办法转换过去，如果这个地方有灯泡，我们还需要把灯泡移除，把它能照到的地方减少。

```java
class Solution {
    int[][] dirs = new int[][]{{0,0},{0,-1},{0,1},{-1,0},{-1,-1},{-1,1},{1,0},{1,-1},{1,1}};
    public int[] gridIllumination(int n, int[][] lamps, int[][] queries) {
        long N = n;
        Map<Integer, Integer> row = new HashMap<>(), col = new HashMap<>();
        Map<Integer, Integer> left = new HashMap<>(), right = new HashMap<>();
        Set<Long> set = new HashSet<>();
        for (int[] l : lamps) {
            int x = l[0], y = l[1];
            int a = x + y, b = x - y;
            if (set.contains(x * N + y)) continue;
            increment(row, x); increment(col, y);
            increment(left, a); increment(right, b);
            set.add(x * N + y);
        }
        int m = queries.length;
        int[] ans = new int[m];
        for (int i = 0; i < m; i++) {
            int[] q = queries[i];
            int x = q[0], y = q[1];
            int a = x + y, b = x - y;
            if (row.containsKey(x) || col.containsKey(y) || left.containsKey(a) || right.containsKey(b)) ans[i] = 1;

            for (int[] d : dirs) {
                int nx = x + d[0], ny = y + d[1];
                int na = nx + ny, nb = nx - ny;
                if (nx < 0 || nx >= n || ny < 0 || ny >= n) continue;
                if (set.contains(nx * N + ny)) {
                    set.remove(nx * N + ny);
                    decrement(row, nx); decrement(col, ny);
                    decrement(left, na); decrement(right, nb);
                }
            }
        }
        return ans;
    }
    void increment(Map<Integer, Integer> map, int key) {
        map.put(key, map.getOrDefault(key, 0) + 1);
    }
    void decrement(Map<Integer, Integer> map, int key) {
        if (map.get(key) == 1) map.remove(key);
        else map.put(key, map.get(key) - 1);
    }
}
```

