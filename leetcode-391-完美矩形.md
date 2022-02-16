---
title: leetcode-391-完美矩形
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: 1cd6bf45
date: 2021-11-16 08:37:12
---

[$link$](https://leetcode-cn.com/problems/perfect-rectangle/)

<hr/>

![image-20211116083808196](https://gitee.com/cao_ziqiang/img/raw/master/20211116083808.png)

<hr/>

![image-20211116083831688](https://gitee.com/cao_ziqiang/img/raw/master/20211116083831.png)

<hr/>

一开始想到的办法是遍历所有的小矩形,若所有的小矩形面积之和等于最终的大矩形的面积,说明就被完美覆盖了。但是显然只有这样考虑是不够的，我提交过后就看到一种不符合以上思路的情形。

![image-20211116084005244](https://gitee.com/cao_ziqiang/img/raw/master/20211116084005.png)

我们来看这组输入，在图上绘制一下就是如下所示：

![image-20211116084049849](https://gitee.com/cao_ziqiang/img/raw/master/20211116084049.png)

可以比较清晰的看出，红色那块区域是少了的，但是由于绿色区域叠加了，故面积总和还是完美覆盖时那么多，所以显然这样考虑是会有欠缺的。

于是我们发现，因为完美矩形的每个小矩形它们都存在着有边相邻，所以就会有顶点之间的覆盖，除了最终那个大矩形的四个顶点外，其余顶点出现的次数均为偶数次，因而考虑使用一个$set$来暂时存储这些顶点的值，若最终只留下四个端点，且这四个端点围成的大矩形的面积和前面每个小矩形的面积之和相等，说明满足完美覆盖的条件。

```java
class Solution {
    public boolean isRectangleCover(int[][] rectangles) {
        int left = Integer.MAX_VALUE;
        int right = Integer.MIN_VALUE;
        int top = Integer.MIN_VALUE;
        int bottom = Integer.MAX_VALUE;
        int n = rectangles.length;

        Set<String> set = new HashSet<>();
        int sumArea = 0;

        for (int i = 0; i < n; i++) {
            //获取四个点的坐标
            left = Math.min(left, rectangles[i][0]);
            bottom = Math.min(bottom, rectangles[i][1]);
            right = Math.max(right, rectangles[i][2]);
            top = Math.max(top, rectangles[i][3]);
            //计算总小矩形的面积
            sumArea += (rectangles[i][3] - rectangles[i][1]) * (rectangles[i][2] - rectangles[i][0]);
            //分别记录小矩形的坐标
            String lt = rectangles[i][0] + " " + rectangles[i][3];
            String lb = rectangles[i][0] + " " + rectangles[i][1];
            String rt = rectangles[i][2] + " " + rectangles[i][3];
            String rb = rectangles[i][2] + " " + rectangles[i][1];
            //如果有就移除 没有就加入
            if (!set.contains(lt)) set.add(lt);
            else set.remove(lt);
            if (!set.contains(lb)) set.add(lb);
            else set.remove(lb);
            if (!set.contains(rt)) set.add(rt);
            else set.remove(rt);
            if (!set.contains(rb)) set.add(rb);
            else set.remove(rb);
        }

        //最后只剩4个大矩形坐标且面积相等即为完美矩形
        if (set.size() == 4 && set.contains(left + " " + top) && set.contains(left + " " + bottom) && set.contains(right + " " + bottom) && set.contains(right + " " + top)) {
            return sumArea == (right - left) * (top - bottom);
        }
        return false;
    }
}
```

