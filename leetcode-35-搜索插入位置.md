---
title: leetcode-35-搜索插入位置
categories:
  - LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: d5667e0c
date: 2021-10-31 19:28:42
---

[$link$](https://leetcode-cn.com/problems/search-insert-position/)

<hr/>

![image-20211031193006844](https://gitee.com/cao_ziqiang/img/raw/master/20211031193006.png)

<hr/>

- 一个比较暴力的方法就是直接遍历，如果$nums[i] \ge target$，那么将选择此处插入，如果都不大于，说明在最后插入。

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        for(int i=0;i<nums.length;i++){
            if(nums[i]>=target){return i;}
        }
        return nums.length;
    }
}
```

<hr/>

这也是二分搜索中的$highBoundSearch$。

可以使用模板：

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        int low = 0, high = nums.length;
        while(low < high) {
            int mid = low + ((high - low) >> 1);
            if(nums[mid] > target) {
                high = mid;
            } else if (nums[mid] < target) {
                low = mid + 1;
            } else {
                return mid;
            }
        }
        return high;
    }
}
```

