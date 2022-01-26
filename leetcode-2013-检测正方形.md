---
title: leetcode-2013-检测正方形
date: 2022-01-26 22:56:42
categories: LeetCode
tags: [LeetCode,algorithm]
---

[2013. 检测正方形](https://leetcode-cn.com/problems/detect-squares/)

![image-20220126225723060](https://gitee.com/cao_ziqiang/img/raw/master/20220126225723.png)

<hr/>

![img](https://gitee.com/cao_ziqiang/img/raw/master/20220126225750.png)

几何+计数+哈希

由于点可以重复算，这里已经提醒我们用计数了，而且由于是对二维的点进行哈希计数，且数据范围在$0 \le x,y \le 1000$，所以使用横纵长度都为1000的二维数组来对坐标计数。

这里我们可以发现，因为要求的是正方形，所以对于$(x,y)$，它与对角那个点的斜率是1或-1。且对于$(x,y)$有4个方向可以延展，左上方向，左下方向，右上方向，右下方向。对应分别是$(-1,-1),(-1,1),(1,-1),(1,1)$。

故而可以尝试分别往4个方向不断延展，直至越界不符合范围，我们找到了对角的位置$(nx,ny)$后，就可以比较容易地求得另外两个点分别是$(x,ny)$和$(nx,y)$。统计结果时，由于需要对同一个位置的不同点计算，所以使用$\times$来叠加计算结果。

```java
class DetectSquares {
    private static final int N = 1010;
    private int[][] map;
    private int[] dx = new int[]{-1, 1, 1, -1};
    private int[] dy = new int[]{-1, -1, 1, 1};
    public DetectSquares() {
        map = new int[N][N];
    }
    
    public void add(int[] point) {
        map[point[0]][point[1]]++;
    }
    
    public int count(int[] point) {
        int ans = 0;
        int x = point[0], y = point[1];
        // 分别尝试往4个方向延展
        for(int i = 0; i < 4; i ++) {
            int nx = x + dx[i], ny = y + dy[i];
            // 没有越界继续延展
            while(nx >= 0 && nx <= 1000 && ny >= 0 && ny <= 1000) {
                ans += map[x][ny] * map[nx][y] * map[nx][ny];
                nx += dx[i];
                ny += dy[i];
            }
        }
        return ans;
    }
}

/**
 * Your DetectSquares object will be instantiated and called as such:
 * DetectSquares obj = new DetectSquares();
 * obj.add(point);
 * int param_2 = obj.count(point);
 */
```

