---
title: leetcode-34-在排序数组中查找元素的第一个和最后一个位置
categories: LeetCode
tags:
  - LeetCode
  - algorithm
hidden: true
abbrlink: 6aad1c9e
date: 2021-11-14 18:32:30
---

[$link$](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

<hr/>

![image-20211114183305838](https://gitee.com/cao_ziqiang/img/raw/master/20211114183305.png)

<hr/>

最暴力的思路还是比较好想到，当找到了元素出现的第一个位置后，再从第一次出现的位置往后查找，不过需要注意，最后一次出现时的位置$end$要满足的条件有$end\ge0 \text{&&} end \lt n \text{&&} nums[end]=target \text{&&} nums[end+1] \neq target$。

以下代码在遍历时为了防止$end$越界，将条件做了一个小变形，把$end+1$改成了$end-1$。

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int n = nums.length;
        int start = -1, end = -1;
        int i = 0;
        while(i < n) {
            if (nums[i] != target) {
                ++i;
            } else {
                start = i;
                end = i;
                int j = start + 1;
                for(; j < n; j++) {
                    if (j >= 0 && j < n && nums[j - 1] == target && nums[j] != target) {
                        break;
                    }
                }
                end = j - 1;
                break;
            }
        }
        return new int[] {start, end};
    }
}
```

当然也可以改成仍然为+1的形式，不过要修改对应位置：

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int n = nums.length;
        int start = -1, end = -1;
        int i = 0;
        while(i < n) {
            if (nums[i] != target) {
                ++i;
            } else {
                start = i;
                end = i;
                int j = start;
                for(; j < n - 1; j++) {
                    if (j >= 0 && j < n && nums[j] == target && nums[j + 1] != target) {
                        break;
                    }
                }
                end = j;
                break;
            }
        }
        return new int[] {start, end};
    }
}
```

这样的搜索就没有利用上题目给定的升序数组条件，所以我们考虑使用二分查找来对数组查找。

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int n = nums.length;
        int low = 0, high = n - 1, mid;
        int start = -1, end = -1, index = -1;
        while (low <= high) {
            mid = ((high - low) >> 1) + (low);
            if(nums[mid] == target) {
                index = mid;
                break;
            } else if (nums[mid] > target){
                // 缩小区间,往小的方向走
                high = mid - 1;
            } else if (nums[mid] < target) {
                // 缩小区间,往大的方向走
                low = mid + 1;
            }
        }
        if (index == -1) {
            return new int[] {start, end};
        } else {
            start = end = index;
            while(start > 0) {
                if (nums[start - 1] != target) {
                    break;
                } else {
                    --start;
                }
            }
            while(end < n - 1) {
                if (nums[end + 1] != target) {
                    break;
                } else {
                    ++end;
                }
            }
            return new int[]{start, end};
        }       
    }
}
```

