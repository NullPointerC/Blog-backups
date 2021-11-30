---
title: leetcode-557-反转字符串中的单词Ⅲ
date: 2021-11-03 21:24:55
categories: LeetCode
tags: [LeetCode,algorithm]
---

[$link$](https://leetcode-cn.com/problems/reverse-words-in-a-string-iii/)

<hr/>

![image-20211103212535200](https://gitee.com/cao_ziqiang/img/raw/master/20211103212535.png)

<hr/>

思路比较简单，把每个单词先按空格分出来，然后再将每个单词反转，最后拼起来就可以了。

```java
class Solution {
    public String reverseWords(String s) {
        int n = s.length();
        if(n == 0 || "".equals(n)) {
            return new String();
        }
        String[] strs = s.split(" ");
        int words = strs.length;
        for(int i = 0; i < words; i++) {
            char[] chars = strs[i].toCharArray();
            reverse(chars);
            strs[i] = new String(chars);
        }
        // System.out.println(Arrays.toString(strs));
        StringBuilder sb = new StringBuilder();
        for(int i = 0; i < words; i++) {
            sb.append(strs[i]);
            if(i != words - 1) {
                sb.append(" ");
            }
        }
        return sb.toString();
    }

    private void reverse(char[] s) {
        int n = s.length;
        for(int i = 0; i < n / 2; i++) {
            int j = n - i - 1;
            s[i] ^= s[j];
            s[j] ^= s[i];
            s[i] ^= s[j];
        }
    }
}
```

<hr/>

再来一个lambda表达式的

```java
class Solution {
    public String reverseWords(String s) {
        return String.join(" ", Arrays.stream(s.split(" ")).map(x -> new StringBuilder(x).reverse().toString()).toArray(CharSequence[]::new));
    }
}
```

