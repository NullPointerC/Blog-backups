---
title: leetcode-202-快乐数
categories:
  - LeetCode
tags:
  - algorithm
  - LeetCode
abbrlink: ee4db54e
date: 2021-09-08 10:49:10
---

[link](https://leetcode-cn.com/problems/happy-number/)

<hr/>

题目描述:

编写一个算法来判断一个数 $n$ 是不是快乐数。

「快乐数」定义为：

- 对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和。

- 然后重复这个过程直到这个数变为 1，也可能是 无限循环 但始终变不到 1。

- 如果 可以变为  1，那么这个数就是快乐数。

如果 $n$ 是快乐数就返回 $true$ ；不是，则返回 $false$ 。

<hr/>

示例 1：

```
输入：19
输出：true
解释：
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1
```

<center>这题真的是</center>

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210908105125.jpg)

没看题解咋都没想明白,,,把自己陷进了循环里去,看了题解,我敲,双指针还能这么用...

设置一个快指针每次进行两轮$convert$,慢指针进行一轮$convert$,如果这个数是一个快乐数,那么最后得到的肯定是$\mathrm1$,

经过$convert$后,还是$\mathit1$,所以最终快慢指针会相等,并且如果等于$1$,说明这是一个快乐数.

```java
class Solution {
    public boolean isHappy(int n) {
        int fast = convert(n);
        int slow = n;
        while (fast != slow) {
            fast = convert(fast);
            fast = convert(fast);
            slow = convert(slow);
        }
        if (fast == 1) {
            return true;
        } else {
            return false;
        }
    }

    private int convert(int n) {
        int res = 0;
        while (n != 0) {
            res += Math.pow((n % 10), 2);
            n /= 10;
        }
        return res;
    }
}
```

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210908105600.png)

也可以使用哈希来统计,因为如果这个数不是一个快乐数,经过循环迭代,还是会得到它本身,这时我们如果发现它曾经出现了,就说明已经进入了迭代的过程,这不是一个快乐数,只需要$return$即可

```java
class Solution {
    public boolean isHappy(int n) {
        Set<Integer> record = new HashSet<>();
        while (n != 1 && !record.contains(n)) {
            record.add(n);
            n = getNextNumber(n);
        }
        return n == 1;
    }

    private int getNextNumber(int n) {
        int res = 0;
        while (n > 0) {
            int temp = n % 10;
            res += temp * temp;
            n = n / 10;
        }
        return res;
    }
}
```

两种方法都是极好的方法,以后做题要多观察规律,切忌盲目下手,反而漏掉了关键条件!!!

