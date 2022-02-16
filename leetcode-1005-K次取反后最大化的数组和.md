---
title: leetcode-1005-K次取反后最大化的数组和
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: f097161c
date: 2021-12-03 10:23:57
---

[K 次取反后最大化的数组和](https://leetcode-cn.com/problems/maximize-sum-of-array-after-k-negations/)

<hr/>

![image-20211203102443474](https://gitee.com/cao_ziqiang/img/raw/master/20211203102443.png)

<hr/>

tags(贪心、排序)

这里考虑如何让数组和最大。利用贪心的思路，先达到局部最优，也就是使负数都变为正数，当前的数值达到最大，于是可以使得整个数组和达到最大。

若所有的负数都大于0了，K仍然大于0，则将最小的非负数进行若干次正负调整即可。

```java
class Solution {
    public int largestSumAfterKNegations(int[] nums, int k) {
        int n = nums.length, ans = 0;
        if(n == 0 || k <= 0) {
            return ans;
        }
        Integer[] copy = new Integer[n];
        for(int i = 0; i < n; i++) {
            copy[i] = nums[i];
        }
        Arrays.sort(copy, (a, b) -> {return Math.abs(b) - Math.abs(a);});
        for(int i = 0; i < n; i++) {
            if(copy[i] < 0 && k > 0) {
                copy[i] *= -1;
                k--;
            }
        }
        if(k % 2 == 1) {
            copy[n - 1] *= -1;
        }
        ans = Arrays.stream(copy).mapToInt(i -> new Integer(i)).sum();
        return ans;
    }
}
```

Golang:

```go
func abs(num int) int  {
    if num < 0 {
        return -num
    }
    return num
}
type ints []int
func (s ints) Len() int {
    return len(s)
}
func (s ints) Less(i, j int) bool {
    return (abs(s[i]) - abs(s[j])) > 0
} 
func (s ints) Swap(i, j int) {
    s[i], s[j] = s[j], s[i]
}

func largestSumAfterKNegations(nums []int, k int) int {
    n, ans := len(nums), 0
    sort.Sort(ints(nums))
    // fmt.Println(nums)
    for i := 0; i < n; i++ {
        if nums[i] < 0 && k > 0 {
            nums[i] *= -1
            k--
        }
    } 
    if k % 2 == 1 {
        nums[n - 1] *= -1
    }
    for i := 0; i < n; i++ {
        ans += nums[i]
    }
    return ans
}
```

