---
title: leetcode-482-密钥格式化
categories: [LeetCode]
tags:
  - algorithm
  - LeetCode
date: 2021-10-04 10:48:16
---

[$link$](https://leetcode-cn.com/problems/license-key-formatting/)

<hr/>

![image-20211004104901637](https://gitee.com/cao_ziqiang/img/raw/master/20211004104901.png)

<hr/>

比较简单的模拟,先将给定的$str$去除破折号$'-'$并且将每个字母转为大写。后再将$str$按$len(s) / k$个部分分割，如果有余数，则第一个部分的长度就为$mod$，后其他部分为$k$。

```java
class Solution {
    public String licenseKeyFormatting(String s, int k) {
        s = s.replaceAll("-","").toUpperCase();
        int l = s.length();
        if(l == 0) {
            return "";
        }
        int parts = l / k;
        int mod = l % k;
        StringBuilder sb = new StringBuilder();
        if(mod == 0) {
            // 每部分可以均分为长度为k的
            for(int i = 0; i < parts;i++) {
                sb.append(s.substring(i * k, (i + 1) * k));
                sb.append('-');
            }
            return sb.substring(0, sb.length() - 1);
        }else{
            sb.append(s.substring(0, mod));
            sb.append('-');
            for(int i = 0; i < parts; i++) {
                sb.append(s.substring(mod + i * k, mod + (i + 1) * k));
                sb.append('-');
            }
            return sb.substring(0, sb.length() - 1);
        }
    }
}
```

