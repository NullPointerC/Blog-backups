---
title: AcWing-2041-干草堆
date: 2022-01-03 12:09:07
categories: AcWing
tags: [AcWing,algorithm]
hidden: true
---

[2041. 干草堆](https://www.acwing.com/problem/content/2043/)

题目描述

<pre>
贝茜对她最近在农场周围造成的一切恶作剧感到抱歉，她同意帮助农夫约翰把一批新到的干草捆堆起来。
开始时，共有 N 个空干草堆，编号 1∼N。
约翰给贝茜下达了 K 个指令，每条指令的格式为 A B，这意味着贝茜要在 A..B 范围内的每个干草堆的顶部添加一个新的干草捆。
例如，如果贝茜收到指令 10 13，则她应在干草堆 10,11,12,13 中各添加一个干草捆。
在贝茜完成了所有指令后，约翰想知道 N 个干草堆的中值高度——也就是说，如果干草堆按照高度从小到大排列，位于中间的干草堆的高度。
方便起见，N 一定是奇数，所以中间堆是唯一的。
请帮助贝茜确定约翰问题的答案。
</pre>

输入格式

<pre>
第一行包含 N 和 K。
接下来 K 行，每行包含两个整数 A,B，用来描述一个指令。
</pre>

输出格式

<pre>
输出完成所有指令后，N 个干草堆的中值高度。
</pre>

数据范围

<pre>
1≤N≤106,
1≤K≤25000,
1≤A≤B≤N
</pre>

tags: 差分、排序

```java
import java.util.*;
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt(), k = sc.nextInt();
        int[] a = new int[n];
        while(k-- > 0) {
            int A = sc.nextInt(), B = sc.nextInt();
            a[A-1]++;
            if(B < n) {
                a[B]--;
            }
        }
        for(int i = 1;i < n;i++) {
            a[i] = a[i-1] + a[i];
        }
        Arrays.sort(a);
        System.out.println(a[n/2]);
    }
}
```

这里再记录一下差分是什么，什么时候用差分。

首先回答什么是差分：如果数组$A$是数组$B$的前缀和，那么数组$B$就是数组$A$的差分。

举个栗子：

![img](https://gitee.com/cao_ziqiang/img/raw/master/20220103124646.png)

什么时候用差分：我们构造一个数组的差分数组，是为了频繁对数组中某一区间进行同一操作，例如将序列中$[l,r]$之间的每个数都加上$c$这一操作，可能执行$n$次且每次的$c$不同，如果对原数组进行操作，每次操作都会花费$O(n)$的时间复杂度，如果使用该数组的差分数组进行操作，那么每次操作的复杂度就为$O(1)$。然后求差分数组的前缀和即为所求的结果。

如考虑给数组$s$的区间$[l, r]$中每个数加上$c$，等价于对差分数组$q$操作：$q[l]+=c,q[r+1]-=c$，因为$q[i]=s[i]-s[i-1]$。

以下给出C++的模板用于构造差分。

```cpp
#include<bits/stdc++.h> 
using namespace std; 
int n,m,q,last,sum[10086],b[10086],s[10086]; 
int main() 
{ 
	cin>>n;//n个数  
	for(int i=1,x;i<=n;i++) 
	{ 
		cin>>x;//这里实际上不需要a数组,视题而异  
		b[i]=x-last;//得到差分数组  
		last=x;//别忘了变化last变量  
	} 
	cin>>m;//m次操作 
	for(int i=1,l,r,del;i<=m;i++) 
	{ 
		 cin>>l>>r>>del;//在[l,r] 加上del 
		 b[l]+=del,b[r+1]-=del;  
	} 
	for(int i=1;i<=n;i++) 
	{ 
		s[i]=s[i-1]+b[i];//这里是处理我们的s数组  
		sum[i]=sum[i-1]+s[i];//处理我们的sum数组.  
	} 
	cin>>q;//q个询问  
	for(int i=1,l,r;i<=q;i++) 
	{ 
		cin>>l>>r; //询问[l,r]的区间和. 
		cout<<sum[r]-sum[l-1]<<endl; //输出即可.  
	} 
}
```

故而本题C++代码如下：

```cpp
#include <iostream>
#include <algorithm>
using namespace std;
const int N = 1e6 + 10;
int s[N], q[N];
// 构造以及计算差分数组
//给s区间[l, r]中的每个数加上c等价于对数组q操作：q[l] += c, q[r + 1] -= c
void insert(int l, int r, int c)
{
    q[l] += c;
    q[r + 1] -= c;
}
int main()
{
    int n, k;
    cin >> n >> k;
    // 如果s[i]有初始值使用下面代码，赋初始值同时构造差分数组q
    // for(int i = 1; i <= n; i ++)
    // {
    //     s[i] = 0;
    //     insert(i, i, s[i]);
    // }
    while(k --)
    {
        int a, b;
        cin >> a >> b;
        insert(a, b, 1);  //每次在a到b之间草堆添加一堆
    }
    for(int i = 1; i <= n; i ++) s[i] = s[i - 1] + q[i];  //求出差分数组q的前缀和
    sort(s, s + n);  //排序
    printf("%d\n", s[n / 2]);  //求中位数
    return 0;
}
```

同时看到一个大佬的Java输入模板，在这里记录一下。

```java
import java.io.*;
import java.util.*;
static class InputReader {
    public BufferedReader reader;
    public StringTokenizer tokenizer;

    public InputReader() {
        reader = new BufferedReader(new InputStreamReader(System.in), 32768);
        tokenizer = null;
    }

    public String next() {
        while (tokenizer == null || !tokenizer.hasMoreTokens()) {
            try {
                tokenizer = new StringTokenizer(reader.readLine());
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
        return tokenizer.nextToken();
    }

    public int nextInt() {
        return Integer.parseInt(next());
    }

    public long nextLong() {
        return Long.parseLong(next());
    }
}
static int gcd(int a, int b) { return b == 0 ? a : gcd(b, a % b); }
static int lcm(int a, int b) { return (a * b) / gcd(a, b); }
```

