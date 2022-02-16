---
title: leetcode-3-无重复字符的最长字串
categories: LeetCode
tags:
  - LeetCode
  - algorithm
abbrlink: 5b64bdfe
date: 2021-11-05 17:30:10
---

[$link$](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

<hr/>

![image-20211105173111361](https://gitee.com/cao_ziqiang/img/raw/master/20211105173111.png)

<hr/>

注意到这里说的是字串，而非子序列，所以很自然地想到滑动窗口上，如果是子序列，则可以考虑使用动态规划。

设置窗口初始大小为0，如果窗口的下一个字符未在当前窗口中出现过，那么窗口前沿移动，增加该字符，并更新结果。

否则，窗口后沿移动，重置窗口大小。

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length();
        if(n == 0) {
            return 0;
        }
        int ans = Integer.MIN_VALUE;
        Set<Character> set = new HashSet<>();
        int left = 0, right = 0;
        while(right < n) {
            if (!set.contains(s.charAt(right))) {
                set.add(s.charAt(right));
                // 滑动
                right++;
                ans = Math.max(right - left, ans);
            } else{
                // 含有重复元素,重置窗口
                set.clear();
                left++;
                right = left;
            }
        }
        return ans;
    }
}
```

再补一个Go版本的

```go
func lengthOfLongestSubstring(s string) int {
    n := len(s)
    if n == 0 {
        return 0
    }
    ans := -1
    var set = make(map[uint8]int)
    left := 0
    right := 0
    for right < n {
        if set[s[right]] == 0 {
            set[s[right]]++
            right++
            if ans < (right - left) {
                ans = right - left
            }
        } else {
            set = make(map[uint8]int)
            left++
            right = left
        }
    }
    return ans
}
```

在评论区看到一个大佬的做法，认为很值得借鉴。

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        // 记录字符上一次出现的位置
        int[] last = new int[128];
        Arrays.fill(last, -1);
        int n = s.length();
        int ans = 0;
        int start = 0; // 窗口开始位置
        for(int i = 0; i < n; i++) {
            // 上一次出现的位置
            int letter = s.charAt(i);
            // 如果没有出现过在当前窗口，那么还是原start
            start = Math.max(start, last[letter] + 1);
            // 更新ans
            ans = Math.max(ans, i - start + 1);
            // 更新字符上次出现的位置
            last[letter] = i;
        }
        return ans;
    }
}
```

补一个Go的

```go
func lengthOfLongestSubstring(s string) int {
    n := len(s)
    if n == 0 {
        return 0
    }
    var last [128]int
    for  i := 0; i < 128; i++ {
        last[i] = -1
    }
    start := 0
    ans := -1
    for i := 0; i < n; i++ {
        letter := s[i]
        if start < (last[letter] + 1) {
            start = last[letter] + 1
        }
        if ans < i - start + 1 {
            ans = i - start + 1
        }
        last[letter] = i
    }
    return ans
}
```

