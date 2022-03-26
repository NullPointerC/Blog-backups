---
title: 剑指OfferⅡ-069-山峰数组的顶部
categories: 剑指Offer
tags:
  - 剑指Offer
hidden: true
abbrlink: 21fb1d8c
date: 2021-10-14 09:47:09
---

[$link$](https://leetcode-cn.com/problems/B1IidL/)

<hr/>

![image-20211014094837622](http://static.codenote.xyz/img/20211014094844.png)

<hr/>

比较容易想到的办法是$O(n)$的遍历方式,只需一趟全数组扫描即可找到最大值。

```java
class Solution {
    public int peakIndexInMountainArray(int[] arr) {
        int peak = 0;
        for(int i = 0; i < arr.length; i++) {
            if(arr[i] > arr[peak]) {
                peak = i;
            }
        }
        return peak;
    }
}
```

同时也可以考虑使用二分来解决，题目已经告诉我们是存在这样的$i$，且以$i$为分界，前半部分为升序排列，后半部分又以降序排列。所以可以先采用二分，找到$i$的位置。

证明：因为$arr[0]<arr[1]<arr[2]<...<arr[i]$，且$arr[i]>arr[i+1]>arr[i+2]>...>arr[arr.length-1]$。

若除了$arr[i]$以外还存在其他$arr[j]$满足条件，则说明必有$arr[j]>arr[i]$。又易知$arr[i]$为数组的最大值，所以$arr[j]$不存在且$arr[i]$唯一，所以可以使用二分来查找。

```java
class Solution {
    public int peakIndexInMountainArray(int[] arr) {
        int len = arr.length;
        if(len == 0) {
            return 0;
        }
        int front = 0, back = len - 1;
        while(front < back) {
            int mid = (back-front) / 2 + front;
            if(arr[mid] > arr[mid + 1]) {
                back = mid;
            }else{
                front = mid + 1;
            }
        }
        return front;
    }
}
```

