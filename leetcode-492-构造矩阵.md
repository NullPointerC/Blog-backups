---
title: leetcode-492-构造矩阵
date: 2021-10-23 12:16:30
categories: [LeetCode]
tags: [LeetCode,algorithm]
---

[$link$](https://leetcode-cn.com/problems/construct-the-rectangle/)

![image-20211026121814135](https://gitee.com/cao_ziqiang/img/raw/master/20211026121814.png)

<hr/>

今天的题还是比较简单的,只需进行简单的数学推理即可。

先进行开根找到适中的区间，然后依次递减。

```java
class Solution {
    public int[] constructRectangle(int area) {
        int W =  (int)Math.sqrt(area);
        while(area % W != 0) {
            W--;
        } 
        return new int[]{area / W, W};
    }
}
```

