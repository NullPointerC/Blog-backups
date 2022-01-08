---
title: leetcode-475-供暖器
date: 2021-12-20 21:54:34
categories: LeetCode
tags: [LeetCode,algorithm]
---

[供暖器](https://leetcode-cn.com/problems/heaters/)

<hr/>

![image-20211220215720633](https://gitee.com/cao_ziqiang/img/raw/master/20211220215720.png)

<hr/>

tags: (排序、二分、双指针)

初看可能没有思路，但是我们可以发现，$houses$和$heaters$都是代表一维坐标轴的点，只需满足$\exists( houses[i]-r \le heaters[j] \le houses[i]+r)$，当$0\le i \le houses.length-1,0 \le j \le heaters.length-1$。

即对于每个房屋，要么用前面的暖气，用么用后面的暖气，二者取近的，得到了距离，对于所有房屋，选择最大的上述距离，这样就满足了所有房子能够取暖。

```java
class Solution {
    public int findRadius(int[] houses, int[] heaters) {
        int ans = 0, d = (int)1e9;
        int n1 = houses.length, n2 = heaters.length;
        Arrays.sort(heaters);
        int minHeater = heaters[0], maxHeater = heaters[n2 - 1];
        for(int i = 0; i < n1; i++) {
            if(houses[i] <= minHeater) {
                d = minHeater - houses[i];
            } else if (houses[i] >= maxHeater) {
                d = houses[i] - maxHeater;
            } else {
                int l = 0, r = n2 - 1;
                while(l < r) {
                    int mid = (l + r) >> 1;
                    if(heaters[mid] < houses[i]) {
                        l = mid + 1;
                    } else{
                        r = mid;
                    }
                }
                d = Math.min(heaters[l] - houses[i], houses[i] - heaters[l - 1]);
            }
            ans = Math.max(ans, d);
        }
        return ans;
    }
}
```

也可以使用双指针，会更加快一些。

```java
class Solution {
    public int findRadius(int[] houses, int[] heaters) {
        int ans = 0;
        int d = Integer.MAX_VALUE;
        Arrays.sort(houses);
        Arrays.sort(heaters);
        int j = 0;
        for (int i = 0; i < houses.length; i++) {
            while (j < heaters.length && heaters[j] < houses[i]) {
                j++;
            }
            if (j == 0) {
                d = heaters[j] - houses[i];
            } else if (j == heaters.length) {
                d = houses[i] - heaters[j - 1];
            } else {
                d = Math.min(heaters[j] - houses[i], houses[i] - heaters[j - 1]);
            }
            ans = Math.max(ans,d);
        }
        return ans;
    }
}
```

