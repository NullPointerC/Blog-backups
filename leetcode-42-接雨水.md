---
title: leetcode-42-接雨水
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
hidden: true
abbrlink: 78b424c5
date: 2021-10-08 10:59:41
---

[$link$](https://leetcode-cn.com/problems/trapping-rain-water/)

<hr/>

![image-20211008110018142](https://gitee.com/cao_ziqiang/img/raw/master/20211008110018.png)

<hr/>

由木桶原理可知, 一个木桶能装的水由最短的那根决定,所以对于能够接水的柱子而言,由其左右两边比它高的最短的那根和它当前的差值来决定。

故我们考虑,每次遍历到$height[i]$时,向其左右两边遍历,找到左边的最大值和右边的最大值。然后比较两个最大值中较小的那个和自身的差值来决定装多少雨水。

```java
class Solution {
    public int trap(int[] height) {
        int n = height.length;
        int cnt = 0;
        for(int i = 0; i < n; i++) {
            int left = i;
            int right = i;
            int leftMax = Integer.MIN_VALUE;
            int rightMax = Integer.MIN_VALUE;
            while(left >= 0) {
                leftMax = Math.max(leftMax, height[left]);
                left--;
            }
            while(right < n) {
                rightMax = Math.max(rightMax, height[right]);
                right++;
            }
            cnt += Math.max(0, Math.min(leftMax, rightMax) - height[i]);
        }
        return cnt;
    }
}
```

但是每次这样都需要从当前位置又继续往前面和后面两个方向遍历找最值。故可以先使用动态规划记录下以$height[i]$为起点的左右两边的最大值。

其中$leftMax[i]$代表$i$左边的最大值，$rightMax$为$i$右边的最大值。

```java
class Solution {
    public int trap(int[] height) {
        int n = height.length;
        int cnt = 0;
        int[] leftMax = new int[n];
        int[] rightMax = new int[n];
        for(int i = 1; i < n; i++) {
            leftMax[i] = Math.max(leftMax[i - 1], height[i - 1]);
        }
        
        for(int i = n - 2; i >= 0; i--) {
            rightMax[i] = Math.max(rightMax[i + 1], height[i + 1]);
        }

        for(int i = 0; i < n; i++) {
            cnt += Math.max(0, Math.min(leftMax[i], rightMax[i]) - height[i]);
        }

        return cnt;
    }
}
```

