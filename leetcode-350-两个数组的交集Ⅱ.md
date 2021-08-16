---
title: leetcode-350-两个数组的交集Ⅱ
date: 2021-07-04 10:17:22
categories: algorithm
tags: [algorithm,Java,LeetCode]
---

[link](https://leetcode-cn.com/problems/intersection-of-two-arrays-ii/)

<hr/>

![image-20210704104927143](https://gitee.com/cao_ziqiang/img/raw/master/20210704104927.png)

又是一个好题,可惜没读懂题目意思,一开始以为是求交集的序列,以为顺序也要相同,用双指针怎么样都TTL了,无语子。然后我就去看题解了，好吧，我是fw我承认了。。。

思路：

先求出两个数组哪个是较小的那个数组，因为最后的交集肯定是取较小的那个，然后我们求出哪个是小的后，就对小的数组建立value-count的hashmap，建立好了之后，再去遍历大的数组，如果大的数组中此时遍历到的数在map中有对应关系，并且count>0的，那么就把数添加进去，然后再把count--，直到遍历完大数组

```java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        int l1 = nums1.length, l2 = nums2.length;
        int min = Math.min(l1, l2);
        Map<Integer, Integer> map = new HashMap<>();
        List<Integer> retList = new LinkedList<>();
        if(min == l1) {
            for(int i = 0; i < l1; i++) {
                map.put(nums1[i], map.getOrDefault(nums1[i], 0) + 1);
            }
            retList = compareCount(map, nums2);
        } else {
            for(int i = 0; i < l2; i++) {
                map.put(nums2[i], map.getOrDefault(nums2[i], 0) + 1);
            }
            retList = compareCount(map, nums1);
        }
        int len = retList.size();
        int[] retArray = new int[len];
        for(int i = 0; i < len; i++) {
            retArray[i] = retList.get(i);
        }
        return retArray;
    }

    /**
     * 根据传入的map和数组返回map中key值与nums中元素值的交集
     * @param map value-count哈希表
     * @param nums 元素数组
     * @return 两者的交集
     */
    private List<Integer> compareCount(Map<Integer, Integer> map, int[] nums) {
        List<Integer> retList = new LinkedList<>();
        for(int num : nums) {
            if(map.getOrDefault(num, 0) > 0) {
                retList.add(num);
                map.put(num, map.getOrDefault(num, 0) - 1);
            }
        }
        return retList;
    }
}
```

这里还有一种官方给的题解,是先进行排序,然后再使用双指针进行筛选相同元素

创建一个指针 i 指向 nums1 数组首位，指针 j 指向 nums2 数组首位,开始比较指针 i 和指针 j 的值大小，若两个值不等，则数字小的指针，往右移一位,若 nums 或 nums2有一方遍历结束，代表另一方的剩余值，都是唯一存在，且不会与之产生交集的。

```java
class Solution {
	public int[] intersect(int[] nums1, int[] nums2) {
        Arrays.sort(nums1);
        Arrays.sort(nums2);
        int i = 0, j = 0,index =  0;
        int l1 = nums1.length, l2 = nums2.length;
        int[] retArray = new int[Math.min(l1, l2)];
        while(i < l1 && j < l2) {
            if(nums1[i] < nums2[j]) {
                i++;
            }else if (nums1[i] > nums2[j]) {
                j++;
            }else {
                retArray[index] = nums1[i];
                i++;
                j++;
                index++;
            }
        }
        return Arrays.copyOfRange(retArray,0,index);
    }
}
```

![image-20210704111437005](https://gitee.com/cao_ziqiang/img/raw/master/20210704111437.png)

