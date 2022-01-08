---
title: leetcode-1185-一周中的第几天
date: 2022-01-03 11:18:44
categories: LeetCode
tags: [LeetCode,algorithm]
---

[1185. 一周中的第几天](https://leetcode-cn.com/problems/day-of-the-week/)

<hr/>

![image-20220103111921878](https://gitee.com/cao_ziqiang/img/raw/master/20220103111921.png)

<hr/>

tags: (数学、模拟)

一个比较简单的办法是使用$Java$中的内置日历类：$Calendar$，不过在使用的适合略坑，需要注意，Caladar设置日期的一些问题。

在$Calendar$中，年份和日期是从1开始的，不过月份是从0开始的，既然如此，就可以在设置时将$month$相应减去1即可。

```java
import java.util.*;
class Solution {
    private static final String[] days = new String[]{"Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"};
    public String dayOfTheWeek(int day, int month, int year) {
        Calendar c = Calendar.getInstance();
        c.set(year, month - 1, day);
        return days[c.get(Calendar.DAY_OF_WEEK) - 1];
    }
}
```

还有$java8$提供的新的时间库：

```java
import java.time.LocalDate;
class Solution {
    public String dayOfTheWeek(int day, int month, int year) {
        LocalDate localDate = LocalDate.of(year,month,day);
        String [] ss = {null, "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday","Sunday"};
        return ss[localDate.getDayOfWeek().getValue()];
    }
}
```

当然，在正经面试中当然调$api$是不太被认可的，所以我们再考虑如何使用正确的方法解此题。

我们可以先确定数据范围中1971年的1月1日是星期五，故而统计在当前所求日期之前共有多少天，加上起始日期，再mod每周的天数就可以得到要求的一周之中的第几天。

```java
class Solution {
    private int[] DAYS_OF_MONTH = new int[]{31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
    private String[] NAME_OF_WEEK = new String[]{"Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"};
    public String dayOfTheWeek(int day, int month, int year) {
        int start = 4;
        start += daysOfBefore(year);
        start += getTheDayOfYear(year, month, day);
        return NAME_OF_WEEK[start % 7];
    }

    private int getTheDayOfYear(int year, int month, int day) {
        if(isLeapYear(year)) {
            DAYS_OF_MONTH[1] = 29;
        }
        int sum = 0;
        for(int i = 0; i < month - 1; i++) {
            sum += DAYS_OF_MONTH[i];
        }
        sum += day;
        return sum;
    }
    private int daysOfBefore(int year) {
        int sum = 0;
        for(int i = 1971; i < year; i++) {
            if(isLeapYear(i)) {
                sum += 366;
            } else{
                sum += 365;
            }
        }
        return sum;
    }

    private boolean isLeapYear(int year) {
        if((year % 4 == 0 && year % 100 != 0) || year % 400 == 0) {
            return true;
        }
        return false;
    }
}
```

这里在吐槽一下$leetcode$的评测机，加了$static$就不能过，不知道为什么原因。

