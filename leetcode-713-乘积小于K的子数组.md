---
title: leetcode-713-乘积小于K的子数组
date: 2021-11-20 15:48:09
categories: LeetCode
tags: [LeetCode,algorithm]
---

[$link$](https://leetcode-cn.com/problems/subarray-product-less-than-k/)

<hr/>

![image-20211120154848438](https://gitee.com/cao_ziqiang/img/raw/master/20211120154848.png)

<hr/>

比较经典的滑动窗口，但是要注意这里很容易产生重复计算。

如果一个子数组的乘积小于$K$,那么这个子数组的所有子集都小于$K$。而一个长度为$n$的数组，它所有的子数组个数为$\sum_{i=1}^{n} x$，但是这样计算的话就会和前面的重复。

如样例1中的数据：

$[10,5,2,6]$，其中$[10]$满足条件，第二个满足条件的是$[10,5]$，显然$10$被重复计算了。因此，我们只需要统计最右边数字的字串个数，就不会得到重复的，即在$[10]$中添加$5$，只计算$[5]$和$[10,5]$，也就是窗口的大小。

```java
class Solution {
    public int numSubarrayProductLessThanK(int[] nums, int k) {
        int ans = 0, n = nums.length;
        if (k <= 1) {
            return ans;
        }
        int mul = 1, l = 0, r = 0; 
        for( ; r < n; ++r) {
            mul *= nums[r];
            while (mul >= k) {
                mul /= nums[l++];
            }
            ans += (r - l + 1);
        }
        return ans;
    }
}
```

```go
func numSubarrayProductLessThanK(nums []int, k int) int {
    if k <= 1 {
        return 0
    }
    l, r, n, base, ans := 0, 0, len(nums), 1, 0
    for ; r < n; r++ {
        base *= nums[r]
        for base >= k {
            base /= nums[l]
            l++
        }
        ans += (r - l + 1)
    }
    return ans
}
```

