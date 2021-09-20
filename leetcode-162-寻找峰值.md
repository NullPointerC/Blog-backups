---
title: leetcode-162-寻找峰值
date: 2021-09-15 09:55:10
categories: LeetCode
tags: [algorithm,Java,LeetCode]

---

[$link$](https://leetcode-cn.com/problems/find-peak-element/)

<hr/>

![image-20210915095720636](https://gitee.com/cao_ziqiang/img/raw/master/20210915095720.png)

<hr/>

首先应该很容易想到$O(n)$的解法,一趟遍历,每次取出正遍历位置的元素和$peak$元素比较,并根据大小选择是否交换.

```java
class Solution {
    public int findPeakElement(int[] nums) {
        int len = nums.length;
        if (len == 0) {
            return 0;
        }
        int ans = 0;
        int peak = Integer.MIN_VALUE;
        for (int i = 0; i < len; i++) {
            if (nums[i] > peak) {
                ans = i;
                peak = nums[ans];
            }
        }
        return ans;
    }
}
```

<hr/>

自己没有能想到$O(lgn)$的解法,参考此处[$link$](https://blog.csdn.net/smile_watermelon/article/details/47267089).

最关键的在于这点

```
规律一：如果nums[i] > nums[i+1]，则在i之前一定存在峰值元素
规律二：如果nums[i] < nums[i+1]，则在i+1之后一定存在峰值元素
```

题目也指出$nums[-1]$和$nums[n]$可以被看成是$-∞$.

所以可以使用二分搜索进行尝试,对于$front$和$back$的中间元素$mid$,判断$mid$是否比后一个元素大,是则说明峰值出现在$front$和$mid$之间的元素中,并对$nums[front..mid]$这段区间再次二分,若不大于,这说明峰值出现在$mid+1$到$back$这段区间内,并对$nums[mid+1..back]$这段区间二分

$Code$

```java
class Solution {
    public int findPeakElement(int[] nums) {
        int len = nums.length;
        if(len == 0) {
            return 0;
        }
        int front = 0, back = len - 1;
        while(front < back) {
            int mid = (back-front) / 2 + front;
            if(nums[mid] > nums[mid + 1]) {
                back = mid;
            }else{
                front = mid + 1;
            }
        }
        return front;
    }
}
```

