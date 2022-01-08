---
title: leetcode-1024-视频拼接
date: 2022-01-05 21:33:03
categories: LeetCode
tags: [LeetCode,algorithm]
---

[1024. 视频拼接](https://leetcode-cn.com/problems/video-stitching/)

![image-20220105213324899](https://gitee.com/cao_ziqiang/img/raw/master/20220105213324.png)

tags: 贪心

比较常见的区间覆盖问题，这里使用贪心的解法。先对每个区间按照左端的大小开始排序，这样就按照区间的起点升序排序。再遍历每个区间，使每个区间能够覆盖掉上一个区间能够覆盖的最后一个点。

```java
class Solution {
    public int videoStitching(int[][] clips, int time) {
        int n = clips.length;
        Arrays.sort(clips, (a,b) -> {return a[0] - b[0];});
        // System.out.println(Arrays.deepToString(clips));
        // last表示当前覆盖的最后一个点
        int ans = 0, last = 0, i = 0;
        while(i < n) {
            // last-last+1这一段无法被覆盖,无法连续,可直接退出,
            if(clips[i][0] > last) {
                return -1;
            }
            // 否则去找到所有能够覆盖上一个last的区间中最靠右的那个,故而区间的左端点依然需要小于或是等于last
            int r = Integer.MIN_VALUE;
            while(i < n && clips[i][0] <= last) {
                // 当前区间可以覆盖last
                r = Math.max(r, clips[i][1]);
                // 扫描完当前位置，跳到下一个区间
                i++;
            }
            last = r;
            ans++;
            if(last >= time) {
                break;
            }
        }
        if (last >= time) return ans;
        else return -1;
    }
}
```

