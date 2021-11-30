---
title: leetcode-1436-旅行终点站
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-10-01 12:07:47
---

国庆又来打卡了,祝福祖国繁荣昌盛!!!

![img](https://gitee.com/cao_ziqiang/img/raw/master/20211001121348.gif)

[$link$](https://leetcode-cn.com/problems/destination-city/)

<hr/>

![image-20211001120854669](https://gitee.com/cao_ziqiang/img/raw/master/20211001120854.png)

<hr/>

乍眼一看以为要建图,其实不然。只需要找出只有入度而没有出度的节点即可。

建立两个$Set$，一个存放入度，一个存放出度。

先遍历一遍给出的$paths$，把$paths[i][0]$放入出度城市，$paths[i][1]$放入入度城市。遍历完后，再遍历一遍入度城市，看其是否有出度，若没有，则说明找到了终点。

```java
class Solution {
    public String destCity(List<List<String>> paths) {
        int n = paths.size();
        if(n == 0) {
            return null;
        }
        Set<String> inCity = new HashSet<>();
        Set<String> outCity = new HashSet<>();
        for(int i = 0; i < n; i++) {
            outCity.add(paths.get(i).get(0));
            inCity.add(paths.get(i).get(1));
        }
        for(String city : inCity) {
            if(!outCity.contains(city)) {
                return city;
            }
        }
        return null;
    }
}
```

