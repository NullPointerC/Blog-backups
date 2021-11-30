---
title: leetcode-869-重新排列得到2的幂
date: 2021-10-28 09:19:54
categories: [LeetCode]
tags: [LeetCode, algorithm]
---

[link](https://leetcode-cn.com/problems/reordered-power-of-2/)

<hr/>

![image-20211028092042500](https://gitee.com/cao_ziqiang/img/raw/master/20211028092042.png)

<hr/>

首先比较容易想到的是使用回溯的想法暴力搜索所有的可能结果。

定义$chars$ $\Rightarrow$ 给定的数字$n$转字符数组的结果。

再使用一个标记位$used$代表第$i$位的字符是否已经被使用。

定义回溯过程中的字符串$str$，若$str$的长度和原数字一样长且为2的幂时，回溯结束。

若长度还没达到，继续回溯，记得回溯后消除对自身的影响。

```java
class Solution {
    boolean flag = false;
    public boolean reorderedPowerOf2(int n) {
        if((n & (n - 1)) == 0) {
            return true;
        }
        char[] chars = String.valueOf(n).toCharArray();
        boolean[] used = new boolean[chars.length];
        dfs(chars, used, new StringBuilder());
        return flag;
    }
    private void dfs(char[] chars, boolean[] used, StringBuilder sb) {
        if(sb.length() == chars.length) {
            if(sb.charAt(0) == '0') {
                return;
            }
            long num = Long.parseLong(sb.toString());
            if(num > Integer.MAX_VALUE) {
                return;
            }
            if((num & (num - 1)) == 0) {
                flag = true;
                return;
            }
        }else{
            for(int i = 0; i < chars.length; ++i) {
                if(used[i]) {
                    continue;
                }
                used[i] = true;
                sb.append(chars[i]);
                dfs(chars, used, sb);
                used[i] = false;
                sb.deleteCharAt(sb.length() - 1);
            }
        }
    }
}
```

<hr/>

还可以不用暴力搜索，题目只想知道是否存在，我们可提前将每个为2的幂的数中1-9的数量保存起来，再判断给定的$n$中的1-9数量是否为给定的2的幂的数量。

```java
class Solution {
    private static Set<String> set = new HashSet<>();
    private static final long BOUND = (long)1e9;
    static {
        for(int i = 1; i <= BOUND; i *= 2) {
            char[] temp = new char[10];
            int num = i;
            while(num != 0) {
                ++temp[num % 10];
                num /= 10;
            }
            set.add(new String(temp));
        }
    }

    public boolean reorderedPowerOf2(int n) {
        char[] cnt = new char[10];
        while(n != 0) {
            ++cnt[n % 10];
            n /= 10;
        }
        return set.contains(new String(cnt));
    }
}
```

