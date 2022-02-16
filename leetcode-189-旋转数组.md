---
title: leetcode-189-旋转数组
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: f58b266b
date: 2021-11-01 21:35:32
---

[$link$](https://leetcode-cn.com/problems/rotate-array/)

<hr/>

![image-20211101213618665](https://gitee.com/cao_ziqiang/img/raw/master/20211101213618.png)

<hr/>

比较容易想到的办法还是开过一个新数组，在新数组上添加原数组的元素。

```java
class Solution {
    public void rotate(int[] nums, int k) {
        int n = nums.length;
        int move = k % n;
        int[] arr = new int[n];
        for(int i = 0; i < move; i++) {
            arr[i] = nums[n - move + i];
        }
        
        for(int i = 0; i < n - move; i++) {
            arr[move + i] = nums[i];
        }
        for(int i = 0; i < n; i++) {
            nums[i] = arr[i];
        }
        // System.out.println(Arrays.toString(arr));
    }
}
```

对于移动次数$move$可能会比数组长度$n$更大，但是很显然，就算是超过了n，移动了一个完整的n，数组还是原数组，所以这里对移动次数取模，就可以得到确定的次数。

时间复杂度为$O(n)$，空间复杂度为$O(n)$。

<hr/>

同样，如果有几何思维的话，可以考虑旋转。先将数组整体旋转，就由数组$[0...n-move-1,n-move...n-1]$转为$[n-1...n-move,n-move-1...0]$，在考虑对两部分分别旋转即可以得到正确的顺序，$[n-move...n-1,0...n-move-1]$。

```java
class Solution {
    public void rotate(int[] nums, int k) {
        int n = nums.length;
        int move = k % n;
        reverse(nums, 0, n - 1);
        reverse(nums, 0, move - 1);
        reverse(nums, move, n - 1);
        System.out.println(Arrays.toString(nums));
    }
    private void reverse(int[] nums, int left, int right) {
        while(left <= right) {
            int temp = nums[right];
            nums[right] = nums[left];
            nums[left] = temp;
            left++;
            right--;
        }
    }
}
```

时间复杂度$O(n)$，空间复杂度$O(1)$

