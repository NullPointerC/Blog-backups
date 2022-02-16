---
title: leetcode-1414-和为K的最少斐波那契数字数目
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: 9f74fad6
date: 2022-02-03 00:28:53
---

[1414. 和为 K 的最少斐波那契数字数目](https://leetcode-cn.com/problems/find-the-minimum-number-of-fibonacci-numbers-whose-sum-is-k/)

![image-20220203002937167](https://gitee.com/cao_ziqiang/img/raw/master/20220203002937.png)

一开始以为是$dp$，想枚举$[1...k]$的所有数，令$dp[i]$为$i$的斐波那契数字最少数目，如果$i$本身是斐波那契数字，则返回1即可，否则从$[1,1,2,3...]$枚举最近的斐波那契数字$near$，$dp[i] = dp[i - near] + 1$。后来想明白这其实是贪心的思路了，并不是$dp$。

```java
class Solution {

    private static int[] fib = new int[45];
    private static int size = 0;
    static {
        fib[0] = 1;
        fib[1] = 1;
        int cur = 1;
        size = 2;
        while(cur < (int)(1e9)) {
            fib[size] = fib[size - 1] + fib[size - 2];
            cur = fib[size];
            size++;
        }
        // System.out.println(Arrays.toString(fib));
        // System.out.println(size);
    }
    public int findMinFibonacciNumbers(int k) {
        int cnt = 0;
        while(k > 0) {
            int near = find(k);
            k -= near;
            cnt++;
        }
        return cnt;
    }
    // 从fib数组中寻找小于等于k的最大值
    private int find(int k) {
        int idx = -1;
        for(int i = 0; i < size; i++) {
            if(fib[i] <= k) {
                idx = i;
            } else {
                break;
            }
        }
        return fib[idx];
    }
}
```

这里计算过$fib$数组的大小为45，这里用的是$O(n)$的枚举，其实也可以优化成$O(logn)$的二分，并且$fib$数组也没有必要枚举到$1e9$，只需枚举到距离$k$最近的斐波那契数字即可。

```java
class Solution {
    public int findMinFibonacciNumbers(int k) {
        int cnt = 0;
        int[] arr = new int[50];
        arr[0] = 1;
        arr[1] = 1;
        int idx = 1;
        while(arr[idx] < k) {
            arr[idx + 1] = arr[idx] + arr[idx - 1];
            idx++;
        }
        while(k != 0) {
            k -= find(k, arr, 0, idx);
            cnt++;
        }
        return cnt;
    }
    // 从arr数组中寻找小于等于k的最大值
    private int find(int k, int[] arr, int lo, int hi) {
        while(lo < hi) {
            int mid = ((hi - lo) >> 1) + lo + 1;
            if(arr[mid] <= k) {
                lo = mid;
            } else {
                hi = mid - 1;
            }
        }
        return arr[lo];
    }
}
```

