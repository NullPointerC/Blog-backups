---
title: leetcode-969-煎饼排序
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: af4004e2
date: 2022-02-19 11:18:48
---

[969. 煎饼排序](https://leetcode-cn.com/problems/pancake-sorting/)

![image-20220219111935173](https://gitee.com/cao_ziqiang/img/raw/master/20220219111935.png)

乍一看很难哇,都不知道怎么下手,除了交换的过程知道用双指针缩短时间外,其他的还一下子不知道怎么交换好。

还是看了题解才知道怎么做，感觉思维过程还是比较难的。

观察题目的提示：

![image-20220219112036562](https://gitee.com/cao_ziqiang/img/raw/master/20220219112036.png)

这里说到`arr`中的数就是`1 ~ arr.length`中的整数排列，所以我们考虑将每个数`arr[i]`放到数组的第`arr[i]`个位置，这样排序就完成了。

接下里的过程有点类似于冒泡排序，我们先从大的数，也就是`arr.length`开始，把最大的数先放在未排序位置的最后一个，这样，每一轮最大的数就会依次排好序在末尾。

我们建立一个`idx`数组标识每个数字`i`目前在`arr`数组中的位置，从最大的数字`arr.length`开始，找到它目前的下标`curIdx`，如果`curIdx=i`，说明它已经放对了它的位置，跳过。否则，就得到了数字`i`目前的下标`curIdx`，这是也有两种情况，如果`curIdx=1`，也就是说`i`在数组首部，把`0~i`这一段交换，就可以使`i`从`0`位置换到`i-1`位置，交换完成。如果`i`不在数组首部，而是在一个`curIdx`位置，首先需要的是把`i`交换到数组的首部，再根据在首部的情况，把`i`交换到`i`位置。

```java
class Solution {
    public List<Integer> pancakeSort(int[] arr) {
        int n = arr.length;
        // 表示i放在了arr的第i个位置
        int[] idx = update(arr);
        // System.out.println(Arrays.toString(idx));
        List<Integer> res = new ArrayList<>(n);
        // 枚举每个数字
        for(int i = n; i >= 1; i--) {
            int curIdx = idx[i];
            // 如果i已经放对了位置,也就是idx[i] = i;
            if(curIdx == i) {
                continue;
            } else {
                // 如果当前在1位置,交换1~curIdx即可
                if(curIdx == 1) {
                    res.add(i);
                    reverse(arr, i);
                    idx = update(arr);
                } else {
                    res.add(curIdx);
                    reverse(arr, curIdx);
                    res.add(i);
                    reverse(arr, i);
                    idx = update(arr);
                }
            }
        }
        return res;
    }
    private int[] update(int[] arr) {
        int n = arr.length;
        int[] idx = new int[n + 1];
        for(int i = 0; i < n; i++) {
            idx[arr[i]] = i + 1;
        }
        return idx;
    }
    // 翻转arr[0...k-1]
    private void reverse(int[] arr, int k) {
        int st = 0, ed = k - 1;
        while(st < ed) {
            int tmp = arr[st];
            arr[st] = arr[ed];
            arr[ed] = tmp;
            st++;
            ed--;
        }
    }
}
```

这里还有一个需要注意的地方，每次交换后，`idx`就发生了变化，所有需要`update`一下，因为这个小bug，也是很多次都没有能AC😂。