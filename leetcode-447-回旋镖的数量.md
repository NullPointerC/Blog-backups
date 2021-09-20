---
title: leetcode-447-回旋镖的数量
date: 2021-09-13 21:59:33
categories: LeetCode
tags: [algorithm,Java,LeetCode]

---

[$\huge link$](https://leetcode-cn.com/problems/number-of-boomerangs/)

<hr/>

![image-20210913220029970](https://gitee.com/cao_ziqiang/img/raw/master/20210913220030.png)

<hr/>

读懂题目的意思就好办了,即每一对点和其他点之间的距离只要相等,就可以认为这三个点之间可以组成回旋镖。又因为需要考虑元组的顺序，即$(i,j,k)$和$(i,k,j)$被视为不一样的回旋镖。

这个距离由于后面可能还会用上，所以先用$map$存起来，以${距离:个数}$的形式存起来，为了避免浮点数的精度丢失问题，直接使用$x^2+y^2$来计算距离。

```java
class Solution {
    public int numberOfBoomerangs(int[][] points) {
        int n = points.length;
        int ans = 0;
        for (int i = 0; i < n; i++) {
            Map<Integer, Integer> map = new HashMap<>();
            for (int j = 0; j < n; j++) {
                if (i == j) continue;
                int x = points[i][0] - points[j][0], y = points[i][1] - points[j][1];
                int dist = x * x + y * y;
                map.put(dist, map.getOrDefault(dist, 0) + 1);
            }
            for (int dist : map.keySet()) {
                int cnt = map.get(dist);
                ans += cnt * (cnt - 1);
            }
        }
        return ans;
    }
}
```

