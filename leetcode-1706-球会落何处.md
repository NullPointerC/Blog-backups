---
title: leetcode-1706-球会落何处
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: db7e35c9
date: 2022-02-24 13:59:19
---

[1706. 球会落何处](https://leetcode-cn.com/problems/where-will-the-ball-fall/)

![image-20220224140101213](https://gitee.com/cao_ziqiang/img/raw/master/20220224140101.png)

![image-20220224140111989](https://gitee.com/cao_ziqiang/img/raw/master/20220224140112.png)

想到的办法是对于每个球，都去枚举它的落下的轨迹，如果是轨迹是`1`且不会碰到墙壁或者反方向的轨道，就继续滑落下去，否则，停止滑落。对于`-1`的轨道也同样的方法分析，如果最后可以当最后一行，就说明可以滑下来，落点就是此时的`y`。

```java
class Solution {
    public int[] findBall(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[] res = new int[n];
        for(int i = 0; i < n; i++) {
            int x = 0, y = i;
            while(x < m && y < n) {
                if(grid[x][y] == 1) {
                    if(y == n - 1 || grid[x][y + 1] == -1) {
                        break;
                    } else {
                        x += 1;
                        y += 1;
                    }
                } else {
                    if(y == 0 || grid[x][y - 1] == 1) {
                        break;
                    } else {
                        x += 1;
                        y -= 1;
                    }
                }
            }
            if(x >= m) {
                res[i] = y;
            } else {
                res[i] = -1;
            }
        }
        return res;
    }
}
```

