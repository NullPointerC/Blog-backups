---
title: leetcode-986-区间列表的交集
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: 6328c311
date: 2021-11-20 13:43:44
---

[$link$](https://leetcode-cn.com/problems/interval-list-intersections/)

<hr/>

![image-20211120134446265](https://gitee.com/cao_ziqiang/img/raw/master/20211120134446.png)

<hr/>

一个比较容易想到的办法就是利用双指针。但是这个移动也不是乱移动，需要分情况移动。

如果$firstList[i][0]\lt secondList[j][0]$时，此时先判断$A$和$B$是否有重叠部分，若有则找出重叠部分，并根据哪个区间遍历完毕来决定移动哪个指针，细节较多。

```java
class Solution {
    public int[][] intervalIntersection(int[][] firstList, int[][] secondList) {
        List<int[]> ansList = new ArrayList<>();
        int i = 0, j = 0, n1 = firstList.length, n2 = secondList.length;
        while(i < n1 && j < n2) {
            int[] section1 = firstList[i];
            int[] section2 = secondList[j];
            if (section1[0] < section2[0]) {
                if (section1[1] < section2[0]) {
                    // 没有交集的情况
                    i++;
                } else {
                    ansList.add(new int[]{Math.min(section1[1], section2[0]), Math.min(section1[1], section2[1])});
                    if (section2[1] < section1[1]) {
                        j++;
                    } else {
                        i++;
                    }
                }
            } else {
                if (section1[0] > section2[1]) {
                    j++;
                } else {
                    ansList.add(new int[]{Math.min(section1[0], section2[1]), Math.min(section1[1], section2[1])});
                    if (section1[1] < section2[1]) {
                        i++;
                    } else {
                        j++;
                    }
                }
            }
        }
        int[][] ans = new int[ansList.size()][2];
        for(int k = 0; k < ansList.size(); ++k) {
            ans[k] = ansList.get(k);
        }
        return ans;
    }
}
```

以上判断的方式是比较繁琐且容易出错的，可以可以对于每一组$firstList[i]$和$secondList[j]$，判断这一组的$left$和$right$。若$left\le right$，则说明有交集部分，那么此时添加这个交集，并且看哪个区间的右部分更小，选择移动这个指针。

```java
class Solution {
    public int[][] intervalIntersection(int[][] firstList, int[][] secondList) {
        List<int[]> ansList = new ArrayList<>();
        int i = 0, j = 0, n1 = firstList.length, n2 = secondList.length;
        while(i < n1 && j < n2) {
            int left = Math.max(firstList[i][0], secondList[j][0]);
            int right = Math.min(firstList[i][1], secondList[j][1]);
            if (left <= right) {
                ansList.add(new int[]{left, right});
            }
            if (firstList[i][1] < secondList[j][1]) {
                i++;
            } else {
                j++;
            }
        }
        // System.out.println(ansList);
        int[][] ans = new int[ansList.size()][2];
        for(int k = 0; k < ansList.size(); k++) {
            ans[k] = ansList.get(k);
        }
        return ans;
    }
}
```

