---
title: leetcode-2006-差的绝对值为K的数对数目
date: 2022-02-09 00:26:22
categories: LeetCode
tags: [LeetCode,algorithm]
---

[2006. 差的绝对值为 K 的数对数目](https://leetcode-cn.com/problems/count-number-of-pairs-with-absolute-difference-k/)

![image-20220209003107816](https://gitee.com/cao_ziqiang/img/raw/master/20220209003312.png)

一个比较简单的办法就是$O(n^2)$的枚举。

对于每个$i$，都枚举$i$后面的所有数字$j$，若$abs(nums[i] - nums[j]) = k$，计数加一，最后返回结果即可。

```java
class Solution {
    public int countKDifference(int[] nums, int k) {
        int res = 0, n = nums.length;
        for(int i = 0; i < n; i++) {
            for(int j = i + 1; j < n; j++) {
                if(Math.abs(nums[i] - nums[j]) == k) {
                    res++;
                }
            }
        }
        return res;
    }
}
```

也可以使用哈希计数，存储每个元素$num$和其出现的次数$cnt$。

首先枚举所有的$num$，记录下其出现的次数，再枚举一趟所有的$num$，求出此时的两个数$num1 = num - k$和$num2 = num + k$。

若$num1$和$num2$都在合理区间内，则加上它们出现的次数，由于会存在重复加的情况，需要得到结果后除2即可。

```java
class Solution {
    public int countKDifference(int[] nums, int k) {
        int[] map = new int[101];
        for(var num : nums) {
            map[num]++;
        }
        int res = 0, n = nums.length;
        for(var num : nums) {
            int num1 = num + k;
            int num2 = num - k;
            if(num1 >= 1 && num1 <= 100) {
                res += map[num1];
            } 
            if(num2 >= 1 && num2 <= 100) {
                res += map[num2];
            }
        }
        return res / 2;
    }
}
```

上面的结果还可以继续优化成一趟遍历。

```java
class Solution {
    public int countKDifference(int[] nums, int k) {
        int res = 0;
        int[] map = new int[205];
        for(var num : nums) {
            map[num]++;
            if((num - k) > 0) {
                res += map[num - k];
            }
            res += map[num + k];
        }
        return res;
    }
}
```

