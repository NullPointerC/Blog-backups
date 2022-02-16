---
title: leetcode-283-移动零
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: ab478c9d
date: 2021-11-02 21:01:09
---

[$link$](https://leetcode-cn.com/problems/move-zeroes/)

<hr/>

![image-20211102210200537](https://gitee.com/cao_ziqiang/img/raw/master/20211102210200.png)

<hr/>

一个比较朴素的办法就是开辟一个新空间，然后再遍历一趟原数组，将非0元素依次添加即可。

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int n = nums.length;
        int[] arr = new int[n];
        int idx = -1;
        for(int i = 0; i < n; i++) {
            if(nums[i] != 0) {
                arr[++idx] = nums[i];
            }
        }
        for(int i = 0; i < n; i++) {
            nums[i] = arr[i];
        }
    }
}
```

时间复杂度$O(n)$，空间复杂度$O(n)$。

也可以使用双指针在原地进行交换。

设置两个指针$i$和$j$，如果$i$指针所指向的非0，就将其赋给$j$指针并移动至数组前部，再将$j$指针后的所有元素置为0即可。

![283_1.gif](https://gitee.com/cao_ziqiang/img/raw/master/20211102212039.gif)

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int n = nums.length;
        int i = 0, j = 0;
        for (; i < n; i++) {
            if(nums[i] != 0) {
                nums[j++] = nums[i];
            }
        }
        while(j < n) {
            nums[j++] = 0;
        }
    }
}
```

