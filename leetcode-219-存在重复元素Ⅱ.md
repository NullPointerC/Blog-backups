---
title: leetcode-219-存在重复元素Ⅱ
date: 2021-06-21 21:34:23
categories: LeetCode
tags: [algorithm,Java,LeetCode]
---

<a href="https://leetcode-cn.com/problems/contains-duplicate-ii/">传送门</a>

<hr/>

题目描述:

<pre>
    给定一个整数数组和一个整数 k，判断数组中是否存在两个不同的索引 i 和 j，使得 nums [i] = nums [j]，并
    且 i 和 j 的差的 绝对值 至多为 k。
</pre>

<hr/>

示例1:

<pre>
    输入: nums = [1,2,3,1], k = 3
    输出: true
</pre>

示例2:

<pre>
    输入: nums = [1,0,1,1], k = 1
    输出: true
</pre>

实例3:

<pre>
    输入: nums = [1,2,3,1,2,3], k = 2
    输出: false
</pre>

思路:

<pre>
    这个题目我们首先要看出是重复元素,那么我们在遍历的时候,则需要记忆化重复元素以及它出现的下标,那么我们可以使用hashmap来进行记忆化,并且当发现重复元素时,则进行比较二者的索引值是否至多为k,则说明是小于等于k,比较若不小于.则可以更新map,因为题目说的是至多,而我们正向遍历,到了后面肯定是比第一次出现的位置更远的,而比第二次出现的位置更近,所以我们更新map,继续遍历,直至最后找到差的绝对值至多为k的两个元素则退出循环。
</pre>

代码：

```java
class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        int l = nums.length;
        /*边界条件判断*/
        if(l == 0) {
            return false;
        }
        boolean flag = false;
        /*创建value和index的映射关系*/
        HashMap<Integer, Integer> map = new HashMap();
        for(int i = 0; i < l ;i++) {
            if(!map.containsKey(nums[i])) {
                map.put(nums[i],i);
            }else {
                if(Math.abs(map.get(nums[i]) - i) <= k) {
                    flag = true;
                    break;
                }else {
                    map.put(nums[i],i);
                }
            }
        }
        return flag;
    }
}
```

![image-20210621220144717](https://gitee.com/cao_ziqiang/img/raw/master/20210621220144.png)

<hr/>

在评论区看到一个大佬的做法很有意思,有创新点

```java
class Solution {
	public boolean containsNearbyDuplicate(int[] nums, int k) {
        int[][] allNums = new int[nums.length][2];
        for (int i = 0; i < nums.length; i++) {
            allNums[i][0] = nums[i];
            allNums[i][1] = i;
        }
        Arrays.sort(allNums, new Comparator<int[]>(){
            @Override
            public int compare(int[] a, int[] b) {
                if (a[0] == b[0]) {
                    return a[1] - b[1];
                } else {
                    return a[0] - b[0];
                }
            }
        });
        for (int i = 1; i < nums.length; i++) {
            if (allNums[i][0] == allNums[i - 1][0] && allNums[i][1] - allNums[i - 1][1] <= k) return true;
        }
        return false;
    }
}
```

思路:

<pre>
    创新点在于先根据value,index的顺序存入allNums二维数组中的每一维子数组的0,1位置,然后按照allNums的每一维度子数组0和1排好序,然后如果是value相同的,那么肯定是在相邻位置,只需要判断1位置的值是否小于等于k即可
</pre>

![image-20210621222539330](https://gitee.com/cao_ziqiang/img/raw/master/20210621222539.png)