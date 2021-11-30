---
title: leetcode-153-寻找旋转排序数组中的最小值
date: 2021-11-15 11:03:22
categories: LeetCode
tags: [LeetCode,algorithm]
---

[$link$](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)

tags: (二分)

![image-20211115110417785](https://gitee.com/cao_ziqiang/img/raw/master/20211115110417.png)

<hr/>

一个比较容易想到的办法就是直接搜，时间复杂度$O(n)$。

```go
func findMin(nums []int) int {
    min := 0x0fffffff
    for _, v := range(nums) {
        if min > v {
            min = v
        }
    }
    return min
}
```

另外还可以考虑二分查找，在一个不包含重复元素的升序数组在经过旋转过后，就可以得到以下的折线图。

![fig1](https://gitee.com/cao_ziqiang/img/raw/master/20211115204813.png)

其中横轴表示数组元素的下标，纵轴表示数组元素的值。图中标出了最小值的位置，是我们需要查找的目标。

于是我们看出，若最后一个元素为$x$，在最小值左边的元素，都是从$x$的右边旋转过去的，所以它们都满足大于$x$的条件，而在最小值右边的元素，由于一开始就比$x$小，所以还是满足比$x$要更小的条件。

于是考虑一个二分查找的变种，即以最后一个元素$x$作为二分的$target$，设左边界为$left$，右边界为$right$，我们将区间内的中间元素$nums[mid]$与右边界元素$nums[right]$比较。

若$nums[mid] \lt nums[right]$，说明$nums[mid]$在最小值的右侧，因此可以忽略二分的右半部分，收缩$right$为$mid$。（这里因为没法确认$nums[mid]$和最小值的关系，所以不能收缩为$right-1$）。

![fig2](https://gitee.com/cao_ziqiang/img/raw/master/20211115205615.png)

若$nums[mid] \gt nums[right]$，说明$nums[mid]$在最小值的左侧，因此可以继续收缩区间，收缩$left$为$mid + 1$。这里因为可以明确$nums[mid]$一定会比最小值大，所以可以收缩到$left + 1$。即使加上1到了最小值的位置，也不会错过这个下标。

![fig3](https://gitee.com/cao_ziqiang/img/raw/master/20211115205831.png)

由于数组不包含有重复元素,所以只要当前的二分区间长度不为1,$mid$就不会与$right$重合,而当区间的长度为1时,就可以结束二分了。

```java
class Solution {
    public int findMin(int[] nums) {
        int n = nums.length;
        int left = 0, right = n - 1;
        int min = Integer.MAX_VALUE;
        while (left < right) {
            int mid = ((right - left) >> 1) + left;
            if (nums[mid] <= nums[n - 1]) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return nums[left];
    }
}
```

Go版本：

```Go
func findMin(nums []int) int {
    
    l, r, n := 0, len(nums) - 1, len(nums)
    for l < r {
        mid := l + (r -l) >> 1
        if nums[mid] <= nums[n - 1] {
            r = mid
        } else {
            l = mid + 1
        }
    }
    return nums[l]
}
```

