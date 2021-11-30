---
title: leetcode-704-二分查找
date: 2021-10-31 17:00:36
categories: [LeetCode]
tags: [LeetCode,algorithm]
---

[$link$](https://leetcode-cn.com/problems/binary-search/)

<hr/>

![image-20211031170125744](https://gitee.com/cao_ziqiang/img/raw/master/20211031170125.png)

<hr/>

模板题，没有很多特殊的技巧。

```java
class Solution {
    public int search(int[] nums, int target) {
        int low = 0, high = nums.length - 1;
        while(low <= high) {
            int mid = ((high - low) >> 1) + low;
            if(nums[mid] > target) {
                high = mid - 1;
            }else if (nums[mid] < target) {
                low = mid + 1;
            }else{
                return mid;
            }
        }
        return -1;
    }
}
```

