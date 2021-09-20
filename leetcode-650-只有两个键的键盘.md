---
title: leetcode-650-只有两个键的键盘
date: 2021-09-19 17:10:52
categories: LeetCode
tags: [algorithm,Java,LeetCode]

---

[$link$](https://leetcode-cn.com/problems/2-keys-keyboard/)

<hr/>

![image-20210919171144683](https://gitee.com/cao_ziqiang/img/raw/master/20210919171144.png)

<hr/>

由于得到$n$个字符可能会用到前面的结果,于是我们考虑使用动态规划来求解

记$f[i]$表示为得到$i$个字符$"A"$需要的操作次数,那么我们考虑这$i$的字符可能是由前面的某些字符$Paste$而来,也可能只由$"A"$复制而来。

所以考虑两种情况

$f[i] = i,i \Rightarrow i\quad is \quad prime$

$f[i] = min(f[i],f[j] + i/j),j \le i$

```java
class Solution {
    public int minSteps(int n) {
        
        int[] f = new int[n + 1];
        Arrays.fill(f, Integer.MAX_VALUE);
        f[1] = 0;

        for(int i = 2; i <= n; i++) {
            if(isPrime(i)) {
                f[i] = i;
            }else{
                for(int j = 2; j < i; j++) {
                    if(i % j == 0) {
                        f[i] = Math.min((f[j] + (i / j)),f[i]);
                    }
                }
            }
        }
        return f[n];
    }
    
    private boolean isPrime(int n) {
        for(int i = 2; i <= Math.sqrt(n); i++) {
            if(n % i == 0) {
                return false;
            }
        }
        return true;
    }
}
```

