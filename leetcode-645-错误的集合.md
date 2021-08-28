---
title: leetcode-645-错误的集合
date: 2021-07-04 09:51:01
categories: LeetCode
tags: [algorithm,Java,LeetCode]
---

[link](https://leetcode-cn.com/problems/set-mismatch/)

<hr/>

![image-20210704095157233](https://gitee.com/cao_ziqiang/img/raw/master/20210704095157.png)

思路:做数组的题目很关键的一点是建立value-index的映射关系,所以使用hashmap来做

但是这里建立的不是value-index的关系,而是value-count的关系,而且这里为了避免空指针异常,应该尽可能多的使用<font color="blue">`getOrDefault()`</font>,因为对于缺少的元素,map中假如没有它的信息,就会报空指针异常.

重复的数字在数组中出现 2 次，丢失的数字在数组中出现 0 次，其余的每个数字在数组中出现 1 次。因此可以使用哈希表记录每个元素在数组中出现的次数，然后遍历从 1 到 n 的每个数字，分别找到出现 2 次和出现 0 次的数字，即为重复的数字和丢失的数字。

代码:

```java
class Solution {
    public int[] findErrorNums(int[] nums) {
        int l = nums.length;
        if(l == 0 || null == nums) {
            return null;
        }
        Map<Integer,Integer> map = new HashMap<>();
        for(int num : nums) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }
        int[] errors = new int[2];
        for(int i = 1; i < l; i++) {
            int count = map.getOrDefault(i, 0);
            if(count == 2) {
                errors[0]  = i;
            }else if(count == 0) {
                errors[1] = i;
            }else {
                continue;
            }
        }
        return errors;
    }
}
```

昨天也学习了java中的流,突然想到这题也可以用纯数学的理论解决

我们要找的就是两个数,第一个数为缺失的数字,第二个数字为重复的数字

重复的元素 = 当前数组和 - 去重后的数组和

缺失的元素 = 数字1-n的和 - 去重之后的数值和

```java
class Solution {
     public int[] findErrorNums(int[] nums) {
        return new int[] {
                Arrays.stream(nums).sum() - Arrays.stream(nums).distinct().sum(),
                (1 + nums.length) * nums.length / 2 - Arrays.stream(nums).distinct().sum()
        };
    }
}
```

![image-20210704100811247](https://gitee.com/cao_ziqiang/img/raw/master/20210704100811.png)

