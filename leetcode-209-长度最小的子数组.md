---
title: leetcode-209-长度最小的子数组
date: 2021-11-20 16:26:03
categories: LeetCode
tags: [LeetCode,algorithm]
---

[$link$](https://leetcode-cn.com/problems/minimum-size-subarray-sum/)

<hr/>

![image-20211120162641390](https://gitee.com/cao_ziqiang/img/raw/master/20211120162641.png)

<hr/>

又是滑动窗口的题，若窗口内的和大于$target$时，比较此时的窗口大小和之前的窗口，并不断缩小窗口。

借了一下大佬的图：

![img](https://gitee.com/cao_ziqiang/img/raw/master/20211120163334.gif)



```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int ans = Integer.MAX_VALUE, n = nums.length;
        int l = 0, r = 0;
        int cur = 0;
        for (; r < n; ++r) {
            cur += nums[r];
            while(cur >= target) {
                ans = Math.min(ans, r - l + 1);
                cur -= nums[l++];
            }
        }
        return ans == Integer.MAX_VALUE ? 0 : ans;
    }
}
```

Golang:

```go
func minSubArrayLen(target int, nums []int) int {
    l, r, n, ans, cur := 0, 0, len(nums), math.MaxInt32, 0
    for ; r < n; r++ {
        cur += nums[r]
        for cur >= target {
            ans = min(ans, r - l + 1)
            cur -= nums[l]
            l++
        }
    }
    if ans == math.MaxInt32 {
        return 0
    } else {
        return ans
    }
}
func min(a, b int) int {
    if a < b {
        return a
    } 
    return b
}
```

