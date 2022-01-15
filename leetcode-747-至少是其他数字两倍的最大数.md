---
title: leetcode-747-至少是其他数字两倍的最大数
date: 2022-01-13 18:17:25
categories: LeetCode
tags: [LeetCode,algorithm]
---

[747. 至少是其他数字两倍的最大数](https://leetcode-cn.com/problems/largest-number-at-least-twice-of-others/)

![image-20220113181804790](https://gitee.com/cao_ziqiang/img/raw/master/20220113181804.png)

可以贪心的考虑,如果对数组按照升序排序,若最大值比次大值大至少2倍,那么肯定存在这样的最大元素。

所以可以先一趟遍历找到最大值所在的下标，然后再对数组进行排序，若最大值比次大值大至少2倍，则返回最大值下标即可。

```java
class Solution {
    public int dominantIndex(int[] nums) {
        int n = nums.length;
        if(n == 1) {
            return 0;
        }
        int maxIndex = 0;
        for(int i = 0;  i < n; i++) {
            if(nums[i] > nums[maxIndex]) {
                maxIndex = i;
            }
        }
        Arrays.sort(nums);
        if((nums[n - 1] >> 1) >= nums[n - 2]) {
            return maxIndex;
        } else {
            return -1;
        }
    }
}
```

当然，也可以不进行排序，直接保存最大值和次大值即可。

```java
class Solution {
    public int dominantIndex(int[] nums) {
        int n = nums.length;
        if (n == 1) return 0;
        int a = -1, b = 0;
        for (int i = 1; i < n; i++) {
            if (nums[i] > nums[b]) {
                a = b; b = i;
            } else if (a == -1 || nums[i] > nums[a]) {
                a = i;
            }
        }
        return nums[b] >= nums[a] * 2 ? b : -1;
    }
}
```

