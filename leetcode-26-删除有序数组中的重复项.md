---
title: leetcode-26.删除有序数组中的重复项
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
hidden: true
abbrlink: 7c0d00d4
date: 2021-06-08 22:09:07
---

<a href="https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/" style="color:red;text-decoration:none">传送门</a>

<hr/>

![leetcode-cn.com_problems_remove-duplicates-from-sorted-array_](https://gitee.com/cao_ziqiang/img/raw/master/20210608222414.png)

描述:

```html
给你一个有序数组 nums ，请你 原地 删除重复出现的元素，使每个元素 只出现一次 ，返回删除后数组的新长度。

不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。
```

思路:

```
由于题目给我们的已经是排好序的数组,我们要注意到这一点,所以隐含的条件就是如果有重复的元素,那么他们肯定是
在某一段连续区间之间,所以我们可以使用两个指针,快慢指针.
快指针代表一趟遍历数组时的下标,慢指针代表不同元素在数组中的下标
如果nums[fast]!=nums[fast-1],说明fast和前一个数的值不同,那么我们可以将不同的值复制到slow位置
接下来fast继续进行一趟遍历,遍历完成,也就将数组不同元素的个数统计好了,即slow的步长,且由[0,slow-1]的
元素即不同元素
```

代码:

```java
package cn.homyit.leetcode;

/**
 * @author Ziqiang CAO
 * @email 1213409187@qq.com
 */
public class Solution {
    public int removeDuplicates(int[] nums) {
        int n = nums.length;
        if (n == 0) {
            return 0;
        }
        int fast = 1, slow = 1;
        while (fast < n) {
            if (nums[fast] != nums[fast - 1]) {
                nums[slow++] = nums[fast];
            }
            fast++;
        }
        return slow;
    }
}
```

在评论区看到一个大佬的代码,也很简洁,思路不是双指针,但是也是前后比较,有同则复制不同元素

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int t = 0;
        for (int i = 0; i < nums.length; i ++ ) {
            if (i == 0 || nums[i] != nums[i - 1]) nums[t ++ ] = nums[i];
        }
        return t;
    }
}
```

通过截图

![image-20210608222302907](https://gitee.com/cao_ziqiang/img/raw/master/20210608222303.png)

