---
title: leetcode-438-找到字符串中所有字母异位词
date: 2021-11-20 14:49:24
categories: LeetCode
tags: [LeetCode,algorithm]
---

[$link$](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/)

<hr/>

![image-20211120145007268](https://gitee.com/cao_ziqiang/img/raw/master/20211120145007.png)

<hr/>

一个比较容易想到的办法就迭代遍历$s$的每一个长为$p$长度的子串。比较这这个子串和$p$中字母频率是否相同即可。

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> ans = new ArrayList<>();
        int[] freq = new int[26];
        for(char c : p.toCharArray()) {
            freq[c - 'a']++;
        }
        int lp = p.length(), ls = s.length();
        for(int i = 0; i < ls; ++i) {
            if (i + lp > ls) {
                break;
            }
            String sub = s.substring(i, i + lp);
            int[] tmp = new int[26];
            for(int j = 0; j < sub.length(); ++j) {
                tmp[sub.charAt(j) - 'a']++;
            }
            if(Arrays.equals(tmp, freq)) {
                ans.add(i);
            }
        }
        return ans;
    }
}
```

<hr/>

上述方式为暴力法,但是可以发现其中包含了很多不必要的操作,比如,再更新下一步时,其实只涉及了$s[r+1]$和$s[l]$这两个边界的变化,但是在暴力解法中却又重新计算了一遍。

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> ans = new ArrayList<>();
        int ls = s.length(), lp = p.length();
        if (ls < lp) {
            return ans;
        }
        int[] freqs = new int[26], freqp = new int[26];
        int l = 0, r = -1;
        for(int i = 0; i < lp; ++i) {
            freqp[p.charAt(i) - 'a']++;
            freqs[s.charAt(++r) - 'a']++;
        }
        if (Arrays.equals(freqs, freqp)) {
            ans.add(l);
        }
        while(r < ls - 1) {
            freqs[s.charAt(++r) - 'a']++;
            freqs[s.charAt(l++) - 'a']--;
            if (Arrays.equals(freqs, freqp)) {
                ans.add(l);
            }
        }
        return ans;
    }
}
```

还有本题的双指针写法:

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> ans = new ArrayList<>();
        int ls = s.length(), lp = p.length();
        if (ls < lp) {
            return ans;
        }
        int[] freqs = new int[26], freqp = new int[26];
       
        for(int i = 0; i < lp; ++i) {
            freqp[p.charAt(i) - 'a']++;
        }
        int l = 0, r = 0;
        for( ; r < ls; ++r) {
            char currRight = s.charAt(r);
            freqs[currRight - 'a']++;
            while(freqs[currRight - 'a'] > freqp[currRight - 'a']) {
                freqs[s.charAt(l++) - 'a']--;
            }
            if (r - l + 1 == lp) {
                ans.add(l);
            }
        }
        return ans;
    }
}
```

这个双指针我个人感觉比上面的数组难一点,但是比较秀是真的。

做一些个人的解读，若当前$s$的右边界的字母频率比模式串$p$中的该字母的频率大，说明出现了重复，此时收缩左边界，减少这个字母的频率，直到相等就会退出循环，此时，一个字母解决好了。再继续扩大窗口的右边界，当频率都相等了，且窗口大小$r-l+1=lp$时，说明找到啦一个字母异位词的位置，加入结果中。



