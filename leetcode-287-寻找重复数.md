---
title: leetcode-287-寻找重复数
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-09-20 14:30:22
---

[$link$](https://leetcode-cn.com/problems/find-the-duplicate-number/)

<hr/>

![image-20210920143207142](https://gitee.com/cao_ziqiang/img/raw/master/20210920143207.png)

<hr/>

如果不计较排序的成本,那么可以先排序,这样重复的元素就在相邻位置了。只需要一躺遍历即可。

```java
class Solution {
    public int findDuplicate(int[] nums) {
        int ans = nums[0];
        Arrays.sort(nums);
        for(int i = 1;i<nums.length;i++) {
            if(nums[i] == nums[i - 1]) {
                return nums[i];
            }
        }
        return ans;
    }
}
```

时间复杂度为$O(nlogn)$即排序的复杂度，空间复杂度为排序所开辟的额外空间。

但是题目提示我们不修改$nums$且常量级空间$O(1)$。

<hr/>

快慢指针思想, $fast$ 和 $slow$ 是指针, $nums[slow]$ 表示取指针对应的元素，注意 nums 数组中的数字都是在 1 到 n 之间的(在数组中进行游走不会越界)。
        因为有重复数字的出现, 所以这个游走必然是成环的, 环的入口就是重复的元素，即按照寻找链表环入口的思路来做。

```java
class Solution {
    public int findDuplicate(int[] nums) {
        int fast = 0, slow = 0;
        while(true) {
            fast = nums[nums[fast]];
            slow = nums[slow];
            if(slow == fast) {
                fast = 0;
                while(nums[slow] != nums[fast]) {
                    fast = nums[fast];
                    slow = nums[slow];
                }
                return nums[slow];
            }
        }
    }
}
```

