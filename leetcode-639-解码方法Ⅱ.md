---
title: leetcode-639-解码方法Ⅱ
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: f2ce971b
date: 2021-09-27 10:11:34
---

[$link$](https://leetcode-cn.com/problems/decode-ways-ii/)

![image-20210927101206788](https://gitee.com/cao_ziqiang/img/raw/master/20210927101206.png)

<hr/>

本题可以参考“[leetcode-91-解码方法](https://leetcode-cn.com/problems/decode-ways/)”。对于字符串$s$，可将其划分为$s[0],s[1],s[2]...s[s.length()-1]$。

并假设$dp[i]$表示为$s[0...i-1]$的编码种数，对于$dp[i]$来说，它可以由一个字符组成，也可以由两个字符组成。

取$s[i-1]$为$second$，$s[i-2]$为$first$。

如果由$second$来判断：

- 若其为$'*'$，那么它就对应着$[1...9]$中的任意一种编码，共有9种方案，因此状态转移为：$dp[i] = 9 \times dp[i-1]$
- 若其为$'0'$，那么无法被解码，$dp[i] = 0$
- 若其为$'1-9'$，分别对应其中的一种编码，因此：$dp[i] = dp[i-1]$

如果由$first$和$second$共同来判断，此时需要分别判断：

- 如果$first$和$second$均为$'*'$，那么它们分别对应着$'11-26'$中的任意一种编码方案，因此状态转移为：$dp[i] = 15 \times dp[i-2]$
- 如果仅有$second$为$'*'$，当$first$为1时，$second$可以在$[1...9]$中任意选择，$dp[i] = 9 \times dp[i-2]$，如果$first$为2，$second$可以在$[1...6]$中选择，$dp[i] = 6\times dp[i-2]$，如果为其他情况，则无法编码；
- 如果$first$和$second$均不为$'*'$时，那么只有当$first\times10+second$的值≤26时才可以被解码，$dp[i] = dp[i-2]$。

```java
class Solution {
    public int numDecodings(String s) {
      if(s == null || s.length() == 0)
        return 0;
    if(s.length() == 1)
        return s.charAt(0) == '0' ? 0 : s.charAt(0) == '*' ? 9 : 1;
    if(s.charAt(0) == '0')
        return 0;

    char[] chars = s.toCharArray();
    long[] dp = new long[s.length() + 1];
    dp[0] = 1;
    dp[1] = chars[0] == '*' ? 9 : 1;
    for(int i = 2; i <= chars.length ; i ++){
        char first = chars[i - 2]; //表示第i - 1个字符
        char second = chars[i - 1]; //表示第i个字符

        //对于dp[i - 1]
        if(second == '*')
            dp[i] += 9 * dp[i - 1];
        else if(second > '0')
            dp[i] += dp[i - 1];

        //对于dp[i - 2]
        if(first == '*'){
            if(second == '*') {
                dp[i] += 15 * dp[i - 2];
            }else if(second <= '6')
                dp[i] += 2 * dp[i - 2];
            else
                dp[i] += dp[i - 2];
        }else if(first == '1' || first == '2'){
            if(second == '*'){
                dp[i] += first == '1' ? 9 * dp[i - 2] : 6 * dp[i - 2];
            }else if((first - '0') * 10 + second - '0' <= 26){
                dp[i] += dp[i - 2];
            }
        }
        dp[i] %= 1000000007;
    }
    return (int)dp[s.length()];
	}
}
```

