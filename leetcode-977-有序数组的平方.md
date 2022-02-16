---
title: leetcode-977-有序数组的平方
categories:
  - LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: b1131f5c
date: 2021-11-01 21:19:46
---

[$link$](https://leetcode-cn.com/problems/squares-of-a-sorted-array/)

<hr/>

![image-20211101212129801](https://gitee.com/cao_ziqiang/img/raw/master/20211101212129.png)

<hr/>

首先比较容易想到的办法应该是先将所有数求平方,再将其排序即可,但是这样的时间复杂度应该就为排序的时间复杂度$O(nlogn)$,其中$n$为数组长度。

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        for(int i = 0; i < nums.length; i++) {
            nums[i] = nums[i] * nums[i];
        }
        Arrays.sort(nums);
        return nums;
    }
}
```

因为数组是有序的，只不过负数平方之后可能成为最大数了。所以数组平方的最大值就在数组的两端，不是最左边就是最右边，不可能是中间。

使用双指针即可，设置左指针和右指针，判断二者所指向数哪个的平方更大，选择大的先加入到数组尾部。

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int n = nums.length;
        int[] ans = new int[n];
        int left = 0, right = n - 1;
        while(left <= right) {
            if((nums[left] * nums[left]) >= (nums[right] * nums[right])) {
                ans[--n] = (nums[left] * nums[left]);
                left++;
            }else{
                ans[--n] = (nums[right] * nums[right]);
                right--;
            }
        }
        return ans;
    }
}
```

