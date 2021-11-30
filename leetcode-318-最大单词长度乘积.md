---
title: leetcode-318-最大单词长度乘积
date: 2021-11-17 10:45:31
categories: LeetCode
tags: [LeetCode,algorithm]
---

[$link$](https://leetcode-cn.com/problems/maximum-product-of-word-lengths/)

![image-20211117104722418](https://gitee.com/cao_ziqiang/img/raw/master/20211117104722.png)

<hr/>

最容易想到的思路应该是枚举每一个组合,所以也就会在外层有两个循环$i$和$j$。

由于每一个组合都要判断是否含有公共字母，于是对于每一组组合，使用$map$存储字母1的词频，再去字母2中看是否含有字母1的字母。并更新最大值。

```java
class Solution {
    static Map<Character, Integer> times = new HashMap<>();
    public int maxProduct(String[] words) {
        int n = words.length;
        int ans = 0;
        for(int i = 0; i < n; ++i) {
            for(int j = i + 1; j < n; ++j) {
                if (words[i].equals(words[j])) {
                    continue;
                }
                String word1 = words[i];
                String word2 = words[j];
                boolean flag = check(word1, word2);
                if(flag) {
                    ans = Math.max(ans, word1.length() * word2.length());
                }
            }
        }
        return ans;
    }
    private boolean check(String word1, String word2) {
        times.clear();
        for(char ch : word1.toCharArray()) {
            times.put(ch, times.getOrDefault(ch, 0) + 1);
        }
        for(char ch : word2.toCharArray()) {
            if(times.get(ch) != null) {
                return false;
            }
        }
        return true;
    }
}
```

以上算法的时间复杂度为：$O(n^2 \times (L))$，其中$n$为数组$words$的长度，$L$为数组中所有单词长度之和。

这里主要的耗时就出现再如何判断$word[i]$与$word[j]$是否出现有重复字母这一步。于是考虑如何减低这一步的时间复杂度。

由于是需要记录下该字母出现过，不需要考虑其次数问题，于是可以不用$map$，使用$int$的低26位来表示一个单词中的字母是否出现过。

用一个$mask$数组来存储每个字母的掩码，其中转换规则为$mask[i]$表示第$i$个单词出现过的字母，$a$对应32位整数的最后一位，$b$代表倒数第二位，以此类推。

![image-20211117103013491](https://gitee.com/cao_ziqiang/img/raw/master/20211117201118.png)

```java
class Solution {
    public int maxProduct(String[] words) {
        int n = words.length;
        int[] masks = new int[n];
        for (int i = 0; i < n; i++) {
            String word = words[i];
            int wordLength = word.length();
            for (int j = 0; j < wordLength; j++) {
                masks[i] |= 1 << (word.charAt(j) - 'a');
            }         
        }
        int maxProd = 0;
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                if ((masks[i] & masks[j]) == 0) {
                    maxProd = Math.max(maxProd, words[i].length() * words[j].length());
                }
            }
        }
        return maxProd;
    }
}
```

Golang:

```go
func maxProduct(words []string) int {
    ans, n := 0, len(words)
    mask := make([]int, n)
    for i, word := range(words) {
        for _, letter := range(word) {
            mask[i] |= 1 << (letter - 'a')
        }
    }
    for i := 0; i < n; i++ {
        for j := i + 1; j < n; j++ {
            if mask[i] & mask[j] == 0 {
                ans = max(ans, len(words[i]) * len(words[j]))
            }
        }
    }
    return ans
}
func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

