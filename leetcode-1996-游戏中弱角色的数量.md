---
title: leetcode-1996-游戏中弱角色的数量
date: 2022-01-28 23:41:00
categories: LeetCode
tags: [LeetCode,algorithm]
---

[1996. 游戏中弱角色的数量](https://leetcode-cn.com/problems/the-number-of-weak-characters-in-the-game/)

![image-20220128234151148](https://gitee.com/cao_ziqiang/img/raw/master/20220128234151.png)

我们可以按照攻击力降序排序，按照防御力升序排序，维护前面已经遍历过角色的防御力的最大值$defense$，如果当前角色的防御力小于$defense$，那么就说明当前角色就是相对于前面角色的弱角色，所以计数+1。

```java
class Solution {
    public int numberOfWeakCharacters(int[][] properties) {
        Arrays.sort(properties, (a, b) -> {return a[0] != b[0] ? b[0] - a[0] : a[1] - b[1];});
        int defense = properties[0][1], ans = 0;
        for(int i = 1; i< properties.length; i++) {
            if(properties[i][1] < defense) {
                ans++;
                continue;
            }
            defense = Math.max(defense, properties[i][1]);
        }
        return ans;
    }
}
```

