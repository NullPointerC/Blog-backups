---
title: leetcode-13-罗马数字转整数
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-06-10 11:15:03
---

<a href="https://leetcode-cn.com/problems/roman-to-integer/" style="color:red;text-decoration:none">传送门</a>

<hr/>

![leetcode-cn.com_problems_roman-to-integer_](https://gitee.com/cao_ziqiang/img/raw/master/20210610111647.png)

思路：

首先我们要知道罗马数字的计算规则,是按照从左往右的顺序依次计算每个字符对应的十进制数然后相加即可

所以,我们可以维护一张hashmap,map存储罗马字符到数值之间的映射关系.

然后依次遍历输入的字符串s.

通常情况下,罗马数字中小的数字在大的数字右边,但是存在特例是为了表示如4,40,400这样的数是,会把小的数字放在左边,这时,我们在遍历字符串的时候就可以多进行一次判断,如果当前字符对应的数值比下一个字符对应的数值小的话,说明这个数值应该是做减法而不是加法

```java
class Solution {
    public int romanToInt(String s) {
        int l = s.length();
        HashMap<String, Integer> map = new HashMap();
        map.put("I", 1);
        map.put("V", 5);
        map.put("X", 10);
        map.put("L", 50);
        map.put("C", 100);
        map.put("D", 500);
        map.put("M", 1000);
        if (l == 1) {
            return map.get(s);
        }
        int sum = 0;
        String[] split = s.split("");
        for (int i = 0; i < l; i++) {
            int value = map.get(split[i]);
            if (i < l - 1 && value < map.get(split[i + 1])) {
                sum -= value;
            } else {
                sum += value;
            }
        }
        return sum;
    }
}
```

![image-20210610112341720](https://gitee.com/cao_ziqiang/img/raw/master/20210610112341.png)

很奇怪为什么用HashMap这么慢,所以去看了一下比较快的题解,思路都是一样的,但是在这种小数据量的情况下时.使用switch-case语句块可以更快

```java
class Solution {
    public int romanToInt(String s) {
        int sum = 0;
        int preNum = getValue(s.charAt(0));
        for(int i = 1;i < s.length(); i ++) {
            int num = getValue(s.charAt(i));
            if(preNum < num) {
                sum -= preNum;
            } else {
                sum += preNum;
            }
            preNum = num;
        }
        sum += preNum;
        return sum;
    }
    
    private int getValue(char ch) {
        switch(ch) {
            case 'I': return 1;
            case 'V': return 5;
            case 'X': return 10;
            case 'L': return 50;
            case 'C': return 100;
            case 'D': return 500;
            case 'M': return 1000;
            default: return 0;
        }
    }
}
```

![image-20210610114507689](https://gitee.com/cao_ziqiang/img/raw/master/20210610114507.png)

