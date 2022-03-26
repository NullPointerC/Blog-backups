---
title: AcWing-2058-笨拙的手指
categories: AcWing
tags:
  - AcWing
  - algorithm
hidden: true
abbrlink: 8f5117d1
date: 2022-01-02 21:04:38
---

[2058.笨拙的手指](https://www.acwing.com/problem/content/description/2060/)

<hr/>

![image-20220102210547432](http://static.codenote.xyz/img/20220102210547.png)

<hr/>

tags(位运算，暴力)

题目提到了给定的两个数都只有一位错了，那么我们可以枚举这两个数在不同位错了的情况，若改正后，即$str\_a[i]$和$str\_b[j]$分别转换后变为$sum\_a = sum\_b$，则说明此时即是我们要找的正确结果，输出即可。

于是可以嵌套两层循环，分别枚举两个给定字符的每一位即可。

这里枚举要注意，对于$2$进制，非$0$即$1$，所以比较好枚举，而对于$3$进制，则需要枚举其他的两种情况。

以下代码参考了大佬的解法，使用$ra[i] \text{^}=1$。

因为字符$'1'$的$ascii$码对应的二进制为$11,0001$，字符$'0'$对应的$ascii$码为$11,0000$。与$1$异或正好可以使得$0 \rightarrow1$，$1\rightarrow0$。

对于$3$进制，若为$k$，即跳过错误的，尝试其他的$2$种是否可以满足。

```cpp
#include <bits/stdc++.h>
using namespace std;
int main()
{
    string a, b;
    cin >> a >> b;
    for (int i = 0; i < a.size(); i++)
    {
        for (int j = 0; j < b.size(); j++)
        {
            for (char k = '0'; k <= '2'; k++)
            {
                string ra = a;
                ra[i] ^= 1;
                string rb = b;
                if (b[j] == k)
                {
                    continue;
                }
                rb[j] = k;
                int x = 0, y = 0;
                for (int i = 0; i < ra.size(); i++)
                {
                    x = x * 2 + ra[i] - '0';
                }
                for (int i = 0; i < rb.size(); i++)
                {
                    y = y * 3 + rb[i] - '0';
                }
                if (x == y)
                {
                    printf("%d\n", x);
                }
            }
        }
    }
    return 0;
}
```

Java：

```java
import java.util.*;
public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        String a, b;
        a = in.nextLine();
        b = in.nextLine();
        for(int i = 0; i < a.length(); i++) {
            for(int j = 0; j < b.length(); j++) {
                for(char k = '0'; k <= '2'; k++) {
                    char[] ra = a.toCharArray();
                    ra[i] ^= 1;
                    char[] rb = b.toCharArray(); 
                    if(b.charAt(j) == k ) {
                        continue;
                    } else {
                        rb[j] = k;
                    }
                    int x = 0, y = 0;
                    for(int l = 0; l < ra.length; l++) {
                        x = x * 2 + ra[l] - '0'; 
                    }
                    for(int l = 0; l < rb.length; l++) {
                        y = y * 3 + rb[l] - '0';
                    }
                    if(x == y) {
                        System.out.println(x);
                    }
                }
            }
        }
    } 
}
```

