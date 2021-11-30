---
title: leetcode-11-盛最多水的容器
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-09-06 20:25:50
---

[link](https://leetcode-cn.com/problems/container-with-most-water/)

<hr/>

$tags：(双指针+贪心)$

$题目描述:$

给你 $n$个非负整数 $a_{1}，a_{2}，...，a_{n}$，每个数代表坐标中的一个点 $(i, a_{i})$ 。在坐标内画 $n $条垂直线，垂直线 $i$ 的两个端点分别为 $(i, a_{i})$  和 $(i, 0)$ 。找出其中的两条线，使得它们与 $x$ 轴共同构成的容器可以容纳最多的水。
说明：你不能倾斜容器。

$示例1：$

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210906203019.jpeg)

输入：$[1,8,6,2,5,4,8,3,7]$

输出：$49$ 

解释：图中垂直线代表输入数组 $[1,8,6,2,5,4,8,3,7]$。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 $49$。

$示例 2：$

输入：height = $[1,1]$

输出：$1$

$示例 3：$

输入：height = $[4,3,2,1,4]$

输出：$16$

$示例 4：$

输入：height = $[1,2,1]$

输出：$2$

$思路：$

$S(i,j) = min(h[i],h[j])\cdot(j-i)$

如果在不考虑时间复杂度的情况下，很容易可以想到$O(n^{2})$的解法，但是数组的最大长度给到了$10^{5}$级别，这个时候用这种解法就很容易超时了

代码如下：

```java
class Solution {
    public int maxArea(int[] height) {
        int ans = Integer.MIN_VALUE;
        int len = height.length;
        if (len == 0) {
            return 0;
        }
        for (int i = 0; i < len - 1; i++) {
            for (int j = i + 1; j < len; j++) {
                ans = Math.max(ans, (j - i) * Math.min(height[i], height[j]));
            }
        }
        return ans;
    }
}
```

那么我们观察这题的一些可以优化之处，对于一个矩形的面积$S$，需要考虑$width$和$height$，其中$height$由两端点出较小的那个取得，再看$width$，虽然设置数组起始两个位置可以确保取最大，但此时得面积却不一定取到了最大，那么我们考虑在移动过程中，$width$变化且$height$也变化的这种情况，此时保证$height$是往变大的方向上走。

设置一头一尾两个指针，在决策哪个指针移动时，我们发现，不管是左指针移动还是右指针移动，$width$都是减少了1，所以就需要确保$height$尽量大，我们选择之前两指针中$height$较小的哪个进行移动，这样就保留了较高的那条边，放弃了较小的边，以便后序能够获得更高的$height$。

代码如下:

```java
class Solution {
    public int maxArea(int[] height) {

        int len = height.length;
        if (len == 0) {
            return 0;
        }
        int start = 0, end = len - 1;
        int maxHeight = Math.min(height[start], height[end]);
        int ans = (end - start) * maxHeight;
        while (start < end) {
            int h = Math.min(height[end], height[start]);
            ans = Math.max(ans, (end - start) * h);

            if (height[start] < height[end]) {
                start++;
            } else {
                end--;
            }
        }
        return ans;
    }
}
```

