---
title: leetcode-786-第K个最小的素数分数
date: 2021-11-29 18:34:47
categories: LeetCode
tags: [LeetCode,algorithm]
---

[第 K 个最小的素数分数](https://leetcode-cn.com/problems/k-th-smallest-prime-fraction/)

<hr/>

![image-20211129183537598](https://gitee.com/cao_ziqiang/img/raw/master/20211129183537.png)

tags: (排序、二分)

<hr/>

最容易想到的就是直接暴力了，因为已经是去重了的数组，所以直接把是所有组合都求出来，再排序，取第$K$个即可。

这里排序的时候可以利用一点点小技巧:

$\frac{a}{b}-\frac{c}{d}=\frac{(a\times d - c \times b)}{b\times d}$

而$b\times d$很明显大于0，故只需要判断$a\times d - c \times b$的大小即可。

```java
class Solution {
    public int[] kthSmallestPrimeFraction(int[] arr, int k) {
        int n = arr.length;
        List<int[]> combines = new ArrayList<>();
        for(int i = 0; i < n; i++) {
            for(int j = i + 1; j < n; j++) {
                combines.add(new int[] {arr[i], arr[j]});
            }
        }
        combines.sort((a, b) -> {return a[0] * b[1] - a[1] * b[0];});
        return combines.get(k - 1);
    }
}
```

也可以使用一个大小为$K$的大根堆。

```java
class Solution {
    public int[] kthSmallestPrimeFraction(int[] arr, int k) {
        int n = arr.length;
        PriorityQueue<int[]> q = new PriorityQueue<>((a,b)->Double.compare(b[0]*1.0/b[1],a[0]*1.0/a[1]));
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                double t = arr[i] * 1.0 / arr[j];
                if (q.size() < k || q.peek()[0] * 1.0 / q.peek()[1] > t) {
                    if (q.size() == k) q.poll();
                    q.add(new int[]{arr[i], arr[j]});
                }
            }
        }
        return q.poll();
    }
}
```

若堆内元素不足$K$个，直接将二元组入堆，否则判断其和堆顶二元组值的大小。

如果当前的值比堆顶元素大，那么当前元素不会是第$K$大的元素，可以舍弃；

如果当前元素比堆顶元素小，那么堆顶元素不会是第$K$大的元素，使用当前计算值替换堆顶元素。