---
title: leetcode-84-柱状图中的最大矩形
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: 3dba54d2
date: 2022-02-23 21:31:36
---

[84. 柱状图中最大的矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)

![image-20220223213211678](https://gitee.com/cao_ziqiang/img/raw/master/20220223213211.png)

最容易想到的办法就是以每个柱子往两边扩展，枚举以这个柱子作为`height`的矩形面积大小。这样只需要求出以当前高度为矩阵的最大宽度`width`是多少即可。

但是可以发现，枚举的过程是`O(n^2)`的过程，对应这道题的数据范围，是会超时的。

所以就需要想到用单调栈来求解，这里我们可以这么考虑，需要面积更大，所以就需要在固定了高度`height`的情况下，使`width`达到最大，所以需要设置一个递增栈，这样对于每个`height`，都可以方便的向左右两边扩展，这样才可以维持当前柱子作为矩形的高度。

如果新入栈的柱子高度更大，继续入栈即可，否则，记录下当前的栈顶柱子，不断弹出，直到栈顶的比新入栈的小。

1. 对于一个高度，如果能得到向左和向右的边界；
2. 对每个高度求一次面积，这样就可以得到最大的面积；
3. 使用单调栈，得到每个高度的前后边界即可；

```java
class Solution {
    public static int largestRectangleArea(int[] heights) {
        int res = 0;
        Stack<Integer> st = new Stack<>();
        int n = heights.length;
        int[] h = new int[n + 1];
        System.arraycopy(heights, 0, h, 0, n);
        for (int i = 0; i < n + 1; i++) {
            // 碰到了更小的元素
            while (!st.isEmpty() && h[st.peek()] > h[i]) {
                int pos = st.pop();
                res = Math.max(res, h[pos] * (st.isEmpty() ? i : i - st.peek() - 1));
            }
            st.push(i);
        }
        return res;
    }
}
```

