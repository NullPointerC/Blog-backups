---
title: leetcode-88-合并两个有序数组
date: 2021-07-03 14:55:28
categories: LeetCode
tags: [algorithm,Java,LeetCode]
---

[link](https://leetcode-cn.com/problems/merge-sorted-array/)

<hr/>

![image-20210703145619584](https://gitee.com/cao_ziqiang/img/raw/master/20210703145619.png)

思路:利用好已经拍好序这个条件,使用双指针即可,遍历nums1和nums2,判断哪个遍历到的位置哪个小或者是否已经遍历到尾,选择添加进sorted数组即可

```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int i = 0, j = 0;
        int[] sorted = new int[m + n];
        int temp;
        /*两个数组中若还有一个还有没有遍历完的元素*/
        while(i < m || j < n) {
            if(i == m) {
                /*nums1已经遍历完了*/
                temp = nums2[j++];
            }else if (j == n) {
                /*nums2已经遍历完了*/
                temp = nums1[i++];
            }else if(nums1[i] < nums2[j]) {
                /*二者还没有遍历完,选择二者中较小的那个加入进去*/
                temp = nums1[i++];
            }else {
                temp = nums2[j++];
            }
            sorted[i + j - 1] = temp;
        }
        for(int k = 0;k < m + n ; k++) {
            nums1[k] = sorted[k];
        }
    }
}
```

也有看到直接调用内置函数的,把nums2copy到nums1的尾部,然后调用排序

```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        System.arraycopy(nums2,0,nums1,m,n);
        Arrays.sort(nums1);
    }
}
```

![image-20210703150152925](https://gitee.com/cao_ziqiang/img/raw/master/20210703150153.png)

PS:后来在评论区看到一个更好的解法,补充一下

这个解法的优势在于不保存temp临时变量,从后开始遍历

```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int i = m-1, j = n-1, index = m+n-1;
        while (i >= 0 && j >= 0) {
            if (nums1[i] > nums2[j]) nums1[index--] = nums1[i--];
            else nums1[index--] = nums2[j--];
        }
        while (i >= 0) nums1[index--] = nums1[i--];
        while (j >= 0) nums1[index--] = nums2[j--];
    }
}
```

![image-20210703150544553](https://gitee.com/cao_ziqiang/img/raw/master/20210703150544.png)

