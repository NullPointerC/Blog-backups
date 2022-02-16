---
title: leetcode-128-最长连续序列
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: c430e00
date: 2021-09-16 16:02:10
---

[link](https://leetcode-cn.com/problems/longest-consecutive-sequence/)

<hr/>

![image-20210916160424483](C:/Users/86159_0rm4bdi.LAPTOP-6H5LCDIN/AppData/Roaming/Typora/typora-user-images/image-20210916160424483.png)

<hr/>

如果不要求$O(n)$的算法,比较容易想到的一个办法是先进行排序,再一趟遍历即可,但是对于一般的数组,即使是$O(n)$的一些排序算法也难以做到,因为在本题中数组长度开到了$10^5$这个级别,开辟这么大的数组比较占用内存.

假如我们使用一般的排序算法,那么瓶颈就是在排序上,时间复杂度一般为$O(nlog{n})$.排序后,再进行一趟遍历,根据相邻数字的差来判断连续序列即可

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        int n = nums.length;
        if(n == 0 || n == 1) {
            return n;
        }
        Arrays.sort(nums);
        int ans = 1;
        int len = 1;
        for(int i = 1;i<n;i++) {
            if(nums[i] - nums[i-1] == 1) {
                len++;
            }else if(nums[i] == nums[i - 1]) {
                continue;
            }else{
                ans = Math.max(ans,len);
                len = 1;
            }
        }
        // 防止数组排序后恰好为连续的数组
        ans = Math.max(ans, len);
        return ans;
    }
}
```

<hr/>

由于遍历数组需要$O(n)$的复杂度,所以需要考虑对数组元素操作为$O(1)$的方式。

在遍历过程中,我们使用$map$来存储$nums[i] \Rightarrow length_{[left...right]}$。

即存储$nums[i]$包含$nums[i]$的这个序列的长度。

对于初始$map$为空,在遍历过程中,逐渐将$nums[i]$和它的区间长度添加进$map$中去。

并检索它的前一个数和后一个数的区间长度,然后更新当前数的区间长度，更新完当前区间后，还需要更新当前区间至左右端点的区间长度。

$Code$

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        
        int n = nums.length;
        if(n == 0 || n == 1) {
            return n;
        }

        Map<Integer,Integer> map = new HashMap<>();
        int ans = 0;
        int maxLength = 0;
        for(int i = 0;i<n;i++) {
            if(!map.containsKey(nums[i])) {
                int leftLength = map.getOrDefault(nums[i] - 1,0);
                int rightLength = map.getOrDefault(nums[i] + 1,0);
                int currLength = leftLength + rightLength + 1;
                if(currLength > ans) {
                    ans = currLength;
                }
                map.put(nums[i], currLength);
                map.put(nums[i] - leftLength, currLength);
                map.put(nums[i] + rightLength, currLength);
            }
        }
        return ans;
    }
}
```

