---
title: leetcode-1567-乘积为正数的最大子数组长度
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: cbfbbfdb
date: 2021-10-16 16:22:59
---

[$link$](https://leetcode-cn.com/problems/maximum-length-of-subarray-with-positive-product/)

![image-20211016164302379](https://gitee.com/cao_ziqiang/img/raw/master/20211016164302.png)

<hr/>

- 一个比较朴素的想法是从当前位置$i$往前遍历,看其前面的子数组乘积，但是很显然这样的方法在数字都比较大时会超过$int$甚至$long$，时间复杂度也比较高；

- 再考虑一个使用动态规划考虑的情形，假设定义$f[i]$为以$nums[i]$结尾处的乘积为正的子数组长度，定义$g[i]$为以$nums[i]$结尾的乘积为负的子数组长度。

	- 那么对于$num$而言，若$num \gt 0$，则$f[i] = f[i - 1] + 1$，而$g[i]$则需要先看$g[i-1]$是否大于0，$g[i] = g[i-1]>0?g[i-1]+1:0$；
	- 若$num \lt 0$，$f[i] = g[i-1] \gt 0 ? g[i-1]+1:0$,$g[i]=f[i-1]+1$。



```java
class Solution {
    public int getMaxLen(int[] nums) {
        int n = nums.length;
        
        int[] f = new int[n];
        int[] g = new int[n];
        
        f[0] = nums[0] > 0 ? 1 : 0;
        g[0] = nums[0] < 0 ? 1 : 0;
        int ans = f[0];
        for(int i = 1; i < n; i++) {
            if(nums[i] > 0) {
                f[i] = f[i - 1] + 1;
                f[i] = f[i - 1] > 0 ? f[i - 1] + 1 : 0;
            }else if (nums[i] < 0) {
                f[i] = g[i - 1] > 0 ? g[i - 1] + 1 : 0;
                g[i] = f[i - 1] + 1;
            }else {
                f[i] = 0;
                g[i] = 0;
            }
            ans = Math.max(f[i], ans);
        }
        return ans;
    }
}
```

​	对于上述动态规划过程也可以使用状态压缩。

```java
class Solution {
    public static int getMaxLen(int[] nums) {
        int z = 0, f = 0, ans = 0;
        for (int num : nums) {
            if (num == 0) {
                z = 0;
                f = 0;
            } else if (num > 0) {
                z = z + 1;
                if (f > 0) {
                    f = f + 1;
                }
            } else {
                int temp = z;
                if(f > 0) {
                    z = f + 1;
                }else {
                    z = 0;
                }
                f = temp + 1;
            }
            ans = Math.max(ans, z);
        }
        return ans;
    }
}
```

