---
title: leetcode-268-丢失的数字
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-09-16 16:46:51
---

[link](https://leetcode-cn.com/problems/missing-number/)

<hr/>

![image-20210916164959823](https://gitee.com/cao_ziqiang/img/raw/master/20210916164959.png)

<hr/>

<hr/>

对于$nums$中的数字，我们先遍历一次，将其添加到哈希表中。

再对区间$[0,nums.length]$中的元素遍历,看其是否没有出现在哈希表中,如果没有出现在哈希表中,说明其就是丢失的元素,将其返回即可

```java
class Solution {
    public int missingNumber(int[] nums) {
        int n = nums.length;
        Set<Integer> set = new HashSet<>();
        for(int num : nums) {
            set.add(num);
        }
        for(int i = 0;i< n + 1;i++) {
            if(!set.contains(i)) {
                return i;
            }
        }
        return 0;
    }
}
```

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210916165503.jpg)

看到下面的标签中有一个位运算,突然恍然大悟,这种连续数字的题目有一种很快的解法就是使用位运算.

首先我们需要知道异或$ {\wedge} $运算的一些规律

```
a ^ a = 0;
a ^ 0 = a;
```

所以对于区间$[0,nums.length]$，若先用$0$去异或区间内的每一个数，则得到的是$0{\wedge}1{\wedge}2{\wedge}3{\wedge}...{\wedge}nums.length$。再将这个结果和给出的数组进行异或，相同元素将会被消去，最终结果即为丢失的元素。

```java
class Solution {
    public int missingNumber(int[] nums) {
        int n = nums.length;
        int ans = 0;
        for(int i = 0;i<=n;i++) {
            ans ^= i;
        }
        for(int i = 0;i<n;i++) {
            ans ^= nums[i];
        }
        return ans;
    }
}
```

