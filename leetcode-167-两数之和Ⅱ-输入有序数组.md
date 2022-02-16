---
title: leetcode-167-两数之和Ⅱ-输入有序数组
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: a5c7e74f
date: 2021-11-02 21:25:13
---

[$link$](https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/)

<hr/>

![image-20211102212549575](https://gitee.com/cao_ziqiang/img/raw/master/20211102212549.png)

<hr/>

一个比较朴素的办法就是两层循环的去两数之和为给定的$target$的两个元素。

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int n = numbers.length;
        for(int i = 0; i < n; i++) {
            for(int j = i + 1; j < n; j++) {
                if(target - numbers[i] == numbers[j]) {
                    return new int[]{i + 1, j + 1};
                }
            }
        }
        return null;
    }
}
```

时间复杂度为$O(n^2)$，空间复杂度$O(1)$。

也可以使用map来存储value-index的映射关系。

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        Map<Integer,Integer> map = new HashMap<>();
        int n = numbers.length;
        for(int i = 0; i < n; i++) {
            if (map.containsKey(target - numbers[i])) {
                int[] ans = new int[2];
                int j = map.get(target - numbers[i]);
                if (i < j) {
                    return new int[] {i + 1 , j + 1};
                }else{
                    return new int[] {j + 1 , i + 1};
                }
            }
            map.put(numbers[i], i);
        }
        return null;
    }
}
```

也可以使用双指针，因为这是有序数组，所以设置两指针$l$和$r$，当$l$和$r$对应的元素之和为$target$时，则将其返回，否则就判断两数之和比$target$的大小，若更小，则说明要使得和大一些，所以$l$指针右移，使结果变大，否则$r$指针左移。

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int n = numbers.length;
        int l = 0, r = n - 1;
        while(l < r) {
            if(numbers[l] + numbers[r] == target) {
                return new int[]{l + 1, r + 1};
            }else if ((numbers[l] + numbers[r]) < target) {
                l++;
            }else{
                r--;
            }
        }
        return null;
    }
}
```

