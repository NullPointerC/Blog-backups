---
title: leetcode-540-有序数组中的单一元素
date: 2022-02-12 15:49:00
categories: LeetCode
tags: [LeetCode,algorithm]
---

[540. 有序数组中的单一元素](https://leetcode-cn.com/problems/single-element-in-a-sorted-array/)

![image-20220212161220637](https://gitee.com/cao_ziqiang/img/raw/master/20220212161220.png)

看到这题第一反应是位运算异或，从0异或每一个元素即可，剩下的就是只出现过1次的。

①.$0 \text{^} x = x$

②.$x\text{^}x = 0$

```java
class Solution {
    public int singleNonDuplicate(int[] nums) {
        int xor = 0;
        for(var n : nums) xor ^= n;
        return xor;
    }
}
```

时间复杂度为$O(n)$，空间复杂度为$O(1)$。

题目中又提到了时间复杂度为$O(log\ n)$，空间复杂度为$O(1)$，就是想把我们引到二分上去，这里也提到了数组是有序的，但是还是没有想到怎么二分。

看了题解才知道太秀了这个思路。首先因为其他元素都出现了2次，只有一个元素出现了1次，所以数组的长度就为$2\times n +1$，假定这个唯一元素出现的位置是$x$，显然$x$左边有偶数个元素，$x$右边也是有偶数个元素，所以$x$的下标是一个偶数下标。

那么对于$x$左侧的相邻元素来说，$nums[y] = nums[y+1]$时，$y$一定要和$x$的奇偶性相同，所以$y$为偶数，对于$x$右侧的相邻元素来说，$nums[z]=nums[z+1]$时，$z$的奇偶性一定和$x$不同，所以$z$为奇数，所以也就是说$x$是相邻元素奇偶性的分界点，有了分界点也就可以二分了。

```java
class Solution {
    public int singleNonDuplicate(int[] nums) {
        int l = 0, r = nums.length - 1, m;
        while(l < r) {
            // 数据范围比较小,直接加也没关系
            m = (l + r) >> 1;
            if(m % 2 == 1) {
                m--;
            }
            if(nums[m] == nums[m + 1]) {
                l = m + 2;
            } else {
                r = m;
            }
        }
        return nums[l];
    }
}
```

