---
title: leetcode-1-两数之和
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: 9d976f6b
date: 2021-07-03 12:59:18
---

[link](https://leetcode-cn.com/problems/two-sum/submissions/)

<hr/>

![image-20210703130019004](https://gitee.com/cao_ziqiang/img/raw/master/20210703130019.png)

思路:

最简单的办法就是双指针遍历,找到了<font color="blue">`nums[i]+nums[j]==target`</font>的<font color="blue">`i,j`</font>就退出循环即可

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int l = nums.length;
        if(l == 0 || null == nums) {
            return null;
        }
        for(int i = 0; i < l - 1; i++) {
            for(int j = i + 1; j < l; j++) {
                if(nums[i] + nums[j] == target) {
                    return new int[] {i,j};
                }
            }
        }
        return null;
    }
}
```

再想想有没有更好的办法,一般我们做涉及数组和它的下标的问题我们都可以使用哈希表来建立value-index的映射关系,然后这样就可以在O(n)时间内找到value和value之间的关系,避免了双指针的双层遍历

```java
class Solution {
    public int[] twoSum(int[] nums,int target) {
        int l = nums.length;
        if(l == 0 || null == nums) {
            return null;
        }
        Map<Integer,Integer> map = new HashMap<>();
        for(int i = 0; i < l; i++) {
            map.put(nums[i],i);
        }
        for(int i = 0; i < l; i++) {
            int remain = target - nums[i];
            if(map.containsKey(remain) && map.get(remain) != i) {
                return new int[] {i,map.get(remain)};
            }
        }
        return null;
    }
}
```

这里还可以更加优化一点,化2次for为1次

```java
class Solution {
    public int[] twoSum(int[] nums,int target) {
        int l = nums.length;
        if(l == 0 || null == nums) {
            return null;
        }
        Map<Integer,Integer> map = new HashMap<>();
        for(int i = 0; i < l; i++) {
            int remain = target - nums[i];
            if(map.containsKey(remain)) {
                return new int[]{i,map.get(remain)};
            }
            map.put(nums[i],i);
        }

        return null;
    }
}
```

![image-20210703140528082](https://gitee.com/cao_ziqiang/img/raw/master/20210703140528.png)

