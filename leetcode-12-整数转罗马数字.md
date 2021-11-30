---
title: leetcode-12-整数转罗马数字
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-09-21 10:11:47
---

[$link$](https://leetcode-cn.com/problems/integer-to-roman/)

<hr/>

![image-20210921104158988](https://gitee.com/cao_ziqiang/img/raw/master/20210921104159.png)

<hr/>

比较简单的一个办法是直接暴力建立起$map$，映射规则为数字与罗马字符。

然后根据数字的范围来选择添加

```java
class Solution {
    static Map<Integer, String> map = new HashMap<>();
    static {
        map.put(1, "I");
        map.put(4, "IV");
        map.put(5, "V");
        map.put(9, "IX");
        map.put(10, "X");
        map.put(40, "XL");
        map.put(50, "L");
        map.put(90, "XC");
        map.put(100, "C");
        map.put(400, "CD");
        map.put(500, "D");
        map.put(900, "CM");
        map.put(1000, "M");
    }
    public String intToRoman(int num) {
        StringBuilder ans = new StringBuilder();
        while(num!=0) {
            if(num >= 1000) {
                ans.append(map.get(1000));
                num -= 1000;
            }else if(num >= 900) {
                ans.append(map.get(900));
                num -= 900;
            }else if(num >= 500) {
                ans.append(map.get(500));
                num -= 500;
            }else if(num >= 400) {
                ans.append(map.get(400));
                num -= 400;
            }else if(num >= 100) {
                ans.append(map.get(100));
                num -= 100;
            }else if(num >= 90) {
                ans.append(map.get(90));
                num -= 90;
            }else if(num >= 50) {
                ans.append(map.get(50));
                num -= 50;
            }else if(num >= 40) {
                ans.append(map.get(40));
                num -= 40;
            }else if(num >= 10) {
                ans.append(map.get(10));
                num -= 10;
            }else if(num >= 9) {
                ans.append(map.get(9));
                num -= 9;
            }else if(num >= 5) {
                ans.append(map.get(5));
                num -= 5;
            }else if(num >= 4) {
                ans.append(map.get(4));
                num -= 4;
            }else if(num >= 1) {
                ans.append(map.get(1));
                num -= 1;
            }else{
                break;
            }
        }
        return ans.toString();
    }
}
```

