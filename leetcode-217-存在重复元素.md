---
title: leetcode-217-存在重复元素
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-07-02 16:31:15
---

看到leetcode有个[活动](https://leetcode-cn.com/study-plan/data-structures/?progress=kzlb5)是3天学数据结构,正好复习一遍数据结构

[传送门](https://leetcode-cn.com/problems/contains-duplicate/)

<hr/.

题目描述:

<pre>
给定一个整数数组，判断是否存在重复元素。
如果存在一值在数组中出现至少两次，函数返回 true 。如果数组中每个元素都不相同，则返回 false 。
</pre>

示例1:

<pre>
输入: [1,2,3,1]
输出: true
</pre>



示例2:

<pre>
输入: [1,2,3,4]
输出: false
</pre>

示例3:

<pre>
输入: [1,1,1,3,3,4,3,2,4,2]
输出: true
</pre>

思路:

这里我选择先排序,因为如果是排好序的数组,如果有重复元素,那么一定是在相邻位置,只需要一趟遍历即可,加上排序的时间复杂度,可以在O(nlgn)内完成

代码:

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        int l = nums.length;
        if(l == 0 || nums == null) {
            return false;
        }
        Arrays.sort(nums);
        for(int i = 1; i < l; i++) {
            if(nums[i-1] == nums[i]) {
                return true;
            }
        }
        return false;
    }
}
```

也可以使用hashmap来做一个k-v映射关系即可

```java
class Solution {
	public boolean containsDuplicate(int[] nums) {
        int l = nums.length;
        if(l == 0 || nums == null) {
            return false;
        }
        HashMap<Integer, Integer> map = new HashMap();
        for(int i = 0; i < l ;i++) {
            if(!map.containsKey(nums[i])) {
                map.put(nums[i],1);
            }else {
                return true;
            }
        }
        return false;
    }
}
```

也可以使用set来

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        int l = nums.length;
        if(l == 0 || nums == null) {
            return false;
        }
        HashSet<Integer> set = new HashSet<>();
        for(int i = 0;i < l; i++) {
            if(!set.add(nums[i])) {
                return true;
            }
        }
        return false;
    }
}
```

在评论区也有看到炫技的,使用java8中的stream,把数组转IntSream,然后调用distinct()先去重然后再调用count计数,最后看是否和nums.length相等即可

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        return IntStream.of(nums).distinct().count() != nums.length;
    }
}
```

关于IntStream后面也有自己去了解,这里推荐[一篇博文](https://www.jianshu.com/p/461429a5edc9)

后面有机会会重新把Java8全部了解一遍,感觉Java还是挺先进的,虽然现在对云原生的支持不是特别的好,但是Java语言本身也在不断进步着,相信Java在未来还是能占据很久的市场

![image-20210702172507080](https://gitee.com/cao_ziqiang/img/raw/master/20210702172507.png)

