---
title: leetcode-299-猜数字游戏
date: 2021-11-08 10:45:00
categories: LeetCode
tags: [LeetCode,algorithm]
---

[$link$](https://leetcode-cn.com/problems/bulls-and-cows/)

<hr/>

![image-20211108104547609](https://gitee.com/cao_ziqiang/img/raw/master/20211108104547.png)

<hr/>

![image-20211108104608467](https://gitee.com/cao_ziqiang/img/raw/master/20211108104608.png)

<hr/>

思路还是比好想到的,由解释可以看出,如果相同位置的元素相同,那么A的数量应该加上1并且映射中该字符对应的数应该同时减去1,B的数量就等于有相同的映射,但是数量不同的。

```java
class Solution {
    public String getHint(String secret, String guess) {
        int n1 = secret.length(), n2 = guess.length();
        int[] map1 = new int[10];
        int[] map2 = new int[10];
        for(char ch : secret.toCharArray()) {
            map1[ch - '0']++;
        }
        for(char ch : guess.toCharArray()) {
            map2[ch - '0']++;
        }
        int cnt1 = 0, cnt2 = 0;
        for(int i = 0; i < Math.min(n1, n2); i++) {
            if (secret.charAt(i) == guess.charAt(i)) {
                cnt1++;
                map1[secret.charAt(i) - '0']--;
                map2[guess.charAt(i) - '0']--;
            }
        }
        if (Arrays.equals(map1, map2)) {
            cnt2 = n1 - cnt1;
            
        } else {
            for(int i = 0; i < 10; i++) {
                if(map1[i] != 0 && map2[i] != 0) {
                    cnt2 += Math.min(map1[i], map2[i]);
                }
            }
        }
        return cnt1 + "A" + cnt2 + "B";
    }
}
```

