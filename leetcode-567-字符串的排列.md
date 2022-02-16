---
title: leetcode-567-字符串的排列
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: cc06db15
date: 2021-11-05 20:24:54
---

[$link$](https://leetcode-cn.com/problems/permutation-in-string/)

<hr/>

![image-20211105202548009](https://gitee.com/cao_ziqiang/img/raw/master/20211105202548.png)

<hr/>

吐了，写了晚上，还WA了几次，写了一个效率贼低的算法。

![image-20211105202716153](https://gitee.com/cao_ziqiang/img/raw/master/20211105202716.png)

因为字符串有很多排列方式，如果我们一种一种地去尝试，肯定是会超时的，全排列就有$n1!$种，再乘上$n2$，时间复杂度肯定爆。

那么就思考不用考虑顺序而只需关注这个模式串中每个字母的频率的解法。

先一趟遍历$s1$，得到$s1$的字符与频率的映射，再使用滑动窗口去遍历$s2$。

若当前遍历到的字符没有在$s1$中出现过，那么直接缩小窗口并前移窗口；

若当前遍历的字符在$s1$中出现过，但是它的频率已经大于$s1$中的频率，也缩小并前移窗口；

若$map$的大小和$s1$中字符数一致，则判断二者中的字符是否频率相等。

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        int n1 = s1.length();
        int n2 = s2.length();
        if (n1 > n2) {
            return false;
        }
        int[] cnt = new int[26];
        int diff = 0;
        for(char c : s1.toCharArray()) {
            if(cnt[c - 'a'] == 0) {
                diff++;
            }
            cnt[c - 'a']++;
        }
        int front = 0, back = 0;
        Map<Character, Integer> map = new HashMap<>();
        while(front < n2) {
            char letter = s2.charAt(front);      
            if(cnt[letter - 'a'] == 0 || (map.containsKey(letter) && cnt[letter - 'a']  == map.get(letter))) {
                back++;
                front = back;
                map.clear();
                continue;
            }
            map.put(letter, map.getOrDefault(letter, 0) + 1);
            front++;
            if(map.size() == diff && check(cnt, map)) {
                return true;
            }
        }
        return false;
    }
    private boolean check(int[] cnt, Map<Character,Integer> map) {
        for(int i = 0;i < cnt.length; i++) {
            if(cnt[i] != 0) {
                if (map.getOrDefault((char)('a' + i), 0) != cnt[i]) {
                    return false;
                }
            }
        }
        return true;
    }
}
```

上述滑动窗口滑的不够明显，也可以每次先固定好$n1$大小的窗口，判断从$s2$中截取$n1$长度的序列频率是否和$s1$的相同，若相同，则返回。

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
         int n1 = s1.length();
        int n2 = s2.length();
        if (n1 > n2) {
            return false;
        }
        int[] pattern = new int[26];
        for(char c : s1.toCharArray()) {
            pattern[c - 'a']++;
        }
        int[] search = new int[26];
        int back = 0, front = n1;
        while(front <= n2) {
            String sub = s2.substring(back, front);
            for(char ch : sub.toCharArray()) {
                search[ch - 'a']++;
            }
            if (Arrays.equals(pattern, search)) {
                return true;
            }
            back++;
            front++;
            Arrays.fill(search, 0);
        }
        return false;
    }
}
```

或者：

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        int n1 = s1.length();
        int n2 = s2.length();
        if (n1 > n2) {
            return false;
        }
        int[] pattern = new int[26];
        int[] search = new int[26];
        for(int i = 0; i < n1; i++) {
            pattern[s1.charAt(i) - 'a']++;
            search[s2.charAt(i) - 'a']++;
        }
        for(int i = n1; i < n2; i++) {
            if(Arrays.equals(pattern, search)) {
                return true;
            }
            // 否则移动窗口
            search[s2.charAt(i - n1) - 'a']--;
            search[s2.charAt(i) - 'a']++;
        }
        return Arrays.equals(pattern, search);
    }
}
```

Golang版本：

```go
func checkInclusion(s1 string, s2 string) bool {
    n, m := len(s1), len(s2)
    if n > m {
        return false
    }
    var cnt1, cnt2 [26]int
    for i, ch := range s1 {
        cnt1[ch-'a']++
        cnt2[s2[i]-'a']++
    }
    if cnt1 == cnt2 {
        return true
    }

    for i := n; i < m; i++ {
        cnt2[s2[i]-'a']++
        cnt2[s2[i-n]-'a']--
        if cnt1 == cnt2 {
            return true
        }
    }
    return false
}
```

